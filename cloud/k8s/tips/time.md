## k8s时间问题



### pod时区和时间统一为宿主机设置

```yaml
apiVersion: settings.k8s.io/v1alpha1
kind: PodPreset
metadata:
  name: timezone-podpreset
spec:
  selector:
    matchLabels:
  ## 时区和时间统一为宿主机的设置
  volumeMounts:
    - mountPath: /etc/localtime
      name: tz-config
      readOnly: true
  volumes:
    - name: tz-config
      hostPath:
        path: /etc/localtime
```



