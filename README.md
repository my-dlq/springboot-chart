# springboot-chart

- **springboot-chart-1.x：** Based on the helm 2.0 +
- **springboot-chart-2.x：** Based on the helm 3.0 +

## describe

springboot chart for helm

## Configurable Parameters

```yaml
#**********************************************************#
#          ====== springboot template ======               #
#**********************************************************#

#**********************************************************#
#                      == Service ==                       #
#**********************************************************#
## Service Type设置 (可以设置为ClusterIP、NodePort、None) ##
service:
  type: ClusterIP
  ports: 
    - name: server
      port: 8080
      targetPort: 8080
      protocol: TCP
    - name: management
      port: 8081
      targetPort: 8081
      protocol: TCP

## ========== service CluserIP配置示例 ========== #####
# service:
#   type: ClusterIP
#   ports: 
#     - name: server
#       port: 8080
#       targetPort: 8080
#       protocol: TCP
## ========== service NodePort配置示例 ========== #####      
# service:
#   type: ClusterIP
#   ports: 
#     - name: server
#       port: 8080
#       targetPort: 8080
#       nodePort: 30080
#       protocol: TCP

#**********************************************************#
#                      == Image ==                         #
#**********************************************************#

image:
  repository: mydlqclub/springboot-helloworld
  tag: 0.0.1
  pullPolicy: IfNotPresent

replicas: 1

imagePullSecrets: []
restartPolicy: Always

#**********************************************************#
#                       == Env ==                         #
#**********************************************************#

env: []

## ========== env配置示例 ========== #####
# env: 
#   - name: "JAVA_OPTS"
#     value: "-Dserver.port=8080"
#   - name: "APP_OPTS"
#     value: "" 

#**********************************************************#
#                       == Port ==                         #
#**********************************************************#

ports: 
  - name: server
    containerPort: 8080
    protocol: TCP
  - name: management
    containerPort: 8081
    protocol: TCP

#**********************************************************#
#                   == PodAnnotations ==                   #
#**********************************************************#

podAnnotations: {}

## ========== readinessProbe配置示例 ========== #####
# podAnnotations：
#   prometheus.io/scrape: "true"
#   prometheus.io/port: "9030"
#   prometheus.io/path: "/"

#**********************************************************#
#                      == Probe ==                         #
#**********************************************************#

livenessProbe: []
readinessProbe: []

## ========== livenessProbe配置示例 ========== #####
# livenessProbe: 
#   httpGet:
#     path: /
#     port: http
## ========== readinessProbe配置示例 ========== #####
# readinessProbe:
#   httpGet:
#     path: /
#     port: http

#**********************************************************#
#                 == serviceAccount ==                     #
#**********************************************************#

serviceAccountName: {}

## ========== serviceAccount配置示例 ========== #####
# serviceAccountName: admin

#**********************************************************#
#                  == podSecurity ==                       #
#**********************************************************#

podSecurityContext: {}
securityContext: {}

## ========== podSecurityContext配置示例 ========== #####
# podSecurityContext: 
  # fsGroup: 2000
## ========== securityContext配置示例 ========== #####
# securityContext: 
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

#**********************************************************#
#                    == Resources ==                       #
#**********************************************************#

resources: 
  limits:
    cpu: 2000m
    memory: 384Mi
  requests:
    cpu: 500m
    memory: 128Mi

## ========== resources配置示例 ========== #####
# resources: 
#   limits:
#     cpu: 2000m
#     memory: 384Mi
#   requests:
#     cpu: 500m
#     memory: 128Mi

#**********************************************************#
#                   == NodeSelector ==                     #
#**********************************************************#

nodeSelector: {}

## ========== tNodeSelector配置示例 ========== #####
# nodeSelector: 
#   kubernetes.io/hostname: k8s-node-2-13

#**********************************************************#
#                    == Tolerations ==                     #
#**********************************************************#

tolerations: []

## ========== tolerations配置示例 ========== #####
# tolerations: 
#   - operator: Exists

#**********************************************************#
#                      == Affinity ==                      #
#**********************************************************#

affinity: {}

## ========== node affinity配置示例 ========== #####
# affinity: 
#   nodeAffinity: 
#     requiredDuringSchedulingIgnoredDuringExecution:  
#       nodeSelectorTerms:
#         - matchExpressions:
#             - key: kubernetes.io/hostname
#               operator: In
#               values:
#                 - k8s-node-2-12
#     preferredDuringSchedulingIgnoredDuringExecution: 
#       - weight: 1   #取值范围1-100
#         preference:
#           matchExpressions:
#             - key: kube
#               operator: In
#               values:
#                 - test

#**********************************************************#
#                      == mount ==                      #
#**********************************************************#

mount:
  volumeMounts: []
  volumes: []

## ========== mount配置示例 ========== #####
# mount:
#   volumeMounts: 
#     - name: spring-app-config
#       mountPath: /opt/deployments/config
#       readOnly: true  
#     - name: data-dir
#       mountPath: /data
#   volumes: 
#     - name: spring-app-config
#       configMap:
#         name: config
#         items:
#         - key: application.yml
#           path: application.yml
#     - name: data-dir
#       persistentVolumeClaim:
#         claimName: datadir-pvc

#**********************************************************#
#                      == Ingress ==                       #
#**********************************************************#

ingress:
  enabled: false
  # hostname: app.mydlq.club
  # path: /
  # service: test
  # port: 80
  # annotations: {}
  # tls:
  #   - secretName: mydlq-tls
  #     hosts:
  #       - www.mydlq.club
  ## 配置多个域名与路径
  ## hosts:
  ## - name: cloud.mydlq.club
  ##   path: /
  ##   service: test1
  ##   port: server
  ## - name: www.mydlq.club
  ##   path: /
  ##   service: test2
  ##   port: server
  
## ========== http协议Ingress配置示例 ========== #####
# ingress:
#   enabled: true
#   hostname: www.mydlq.club
#   path: /
#   service: traefik-dashboard
#   port: 8080
#   annotations: 
#     kubernetes.io/ingress.class: traefik   
#     traefik.ingress.kubernetes.io/router.entrypoints: web
## ========== https协议Ingress配置示例 ==========  #####
# ingress:
#   enabled: true
#   hostname: cloud.mydlq.club
#   path: /
#   service: kubernetes-dashboard
#   port: 443
#   annotations: 
#     kubernetes.io/ingress.class: traefik   
#     traefik.ingress.kubernetes.io/router.tls: "true"
#     traefik.ingress.kubernetes.io/router.entrypoints: websecure
#   tls:
#     - secretName: mydlq-tls 
#       hosts:
#         - cloud.mydlq.club
```