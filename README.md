![image-20-1024x789](https://github.com/user-attachments/assets/efe391a2-6840-44b8-b87b-c9deba242e73)

 
## Terraform Module - Argo Rollouts | ⭐⭐⭐ 
Argo Rollouts is a Kubernetes controller and set of CRDs which provide advanced deployment capabilities such as blue-green, canary, canary analysis, experimentation, and progressive delivery features to Kubernetes.
Rollouts (optionally) integrates with ingress controllers and service meshes, leveraging their traffic shaping abilities to gradually shift traffic to the new version during an update. Additionally, Rollouts can query and interpret metrics from various providers to verify key KPIs and drive automated promotion or rollback during an update.

🎯 Features
```
✅ Blue-Green update strategy
✅ Canary update strategy
✅ Fine-grained, weighted traffic shifting
✅ Automated rollbacks and promotions
✅ Manual judgement
✅ Customizable metric queries and analysis of business KPIs
✅ Ingress controller integration: NGINX, ALB
✅ Service Mesh integration: Istio, Linkerd, SMI
✅ Metric provider integration: Prometheus, Wavefront, Kayenta, Web, Kubernetes Jobs
```

🚀 

Technologies that we use here for module creation : Kubernetes & Terraform and Argo Rollouts


🔨 Example : Config 

## AWS
```
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: canary
spec:
  replicas: 5
  revisionHistoryLimit: 1
  selector:
    matchLabels:
      app: canary
  template:
    metadata:
      labels:
        app: canary-demo
    spec:
      containers:
      - name: canary
        image: argoproj/rollouts-demo:blue
        imagePullPolicy: Always
        ports:
        - name: http
          containerPort: 8080
          protocol: TCP
        resources:
          requests:
            memory: 32Mi
            cpu: 5m
  strategy:
    canary:
      canaryService: canary-preview
      steps:
      - setWeight: 20
      - pause: {}
      - setWeight: 40
      - pause: {duration: 10}
      - setWeight: 60
      - pause: {duration: 10}
      - setWeight: 80
      - pause: {duration: 10}
```
