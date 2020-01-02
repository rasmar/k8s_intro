# ConfigMaps

```sh
kubctl create configmap [cm-name] --from-file [path-to-file] // create configmap from file
kubctl create configmap [cm-name] --from-env-file [path-to-env-file] // create configmap from env file
```

Creating from file adds the configuration nested in `filename.file_extension:` namespace
Creating from env file adds the configuration directly without a namespace

You can also create a configmap directly through the command line with `from-literal`

```sh
kubctl create configmap [cm-name] --from-literal=enemies=aliens --from-literal=lives=3
```

## Accessing ConfigMap

```yml
apiVersion: apps/v1
...
spec:
  template:
  ...
  spec:
    containers: ...
    env:
      - name: ENEMIES // env variable name
        valueFrom:
          configMapKeyRef:
            key: app-settings // ConfigMap name
            name: enemies // ConfigMap key
```

```yml
apiVersion: apps/v1
...
spec:
  template:
  ...
  spec:
    containers: ...
    envFrom:
      - configMapKeyRef:
        name: app-settings
```

```yml
apiVersion: apps/v1
...
spec:
  template:
  ...
  spec:
    volumes:
      - name: app-config-vol
        configMap:
          name: app-settings
    containers:
      volumeMounts:
        - name: app-config-vol
          mountPath: /etc/config
```
