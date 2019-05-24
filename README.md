# springboot-chart

springboot chart for helm

Configurable parameters

```yaml
#可配置参数

#**********************************************************#
#                      == Deployment ==                    #
#**********************************************************#

######---类型设置 (可选为Deployment、StatefulSet，默认为：Deployment)---######
kind: Deployment

######---镜像设置---######
image:
  repository: "mydlqclub/springboot-helloworld"
  tag: "0.0.1"
  pullPolicy: "IfNotPresent"

######---副本数设置---######
replicas: 1

######---更新策略---####
updateStrategy:
  type: RollingUpdate

######---Resources资源限制设置---######
resources:
  limits:
    memory: 512Mi
    cpu: 1000m
  requests:
    memory: 256Mi
    cpu: 500m

######---Pod podAnnotations设置---######
podAnnotations:
#  prometheus.io/path: "/"

######---terminationGracePeriodSeconds优雅关闭Pod设置(默认30s)---######
terminationGracePeriodSeconds: 10

######---环境变量ENV设置---######
env: 
  - name: "JAVA_OPTS"
    value: "-Dserver.port=8080"
  - name: "APP_OPTS"
    value: "" 

######---环境变量EnvFrom设置---######
envFrom:
  #- configMapRef:
  #    name: env-config

######---Port端口设置---######
ports: 
  - name: server
    containerPort: 8080
    protocol: TCP
  - name: management
    containerPort: 8081
    protocol: TCP

######---Read && Live 探针设置---######
probe:
  port: 8081
  path: /actuator/health
  livenessProbe:
    initialDelaySeconds: 40
    periodSeconds: 10
    successThreshold: 1
    failureThreshold: 10
    timeoutSeconds: 15
  readinessProbe:
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    failureThreshold: 10
    timeoutSeconds: 15

######---启动节点设置设置---######
nodeSelector: []

######---Volume && VolumeMounts设置---######
mount: 
  # volumeMounts: []
  # volumes: []
  # volumeClaimTemplates: [] #(部署类型必须是：StatefulSet)

######---Affinity设置---######
affinity: {}

######---Tolerations容忍设置---######
tolerations: []

#**********************************************************#
#                      == Service ==                       #
#**********************************************************#

service:
  ######---Service type设置 (可以设置为ClusterIP、NodePort、None)---######
  type: ClusterIP
   ######---Service Labels设置---######
  labels: 
    # svcEndpoints: actuator
  ######---Service Annotations设置---######
  annotations: {}
  ######---Service Ports设置---######
  ports: 
    - name: server
      port: 8080
      targetPort: 8080
      protocol: TCP
      # 如果type为NodePort,则可以设置下面的nodePort端口,否则请不要设置
      # nodePort: 30080 
    - name: management
      port: 8081
      targetPort: 8081
      protocol: TCP
      # 如果type为NodePort,则可以设置下面的nodePort端口,否则请不要设置
      # nodePort: 30081
```