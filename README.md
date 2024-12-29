![image-20-1024x789](https://github.com/user-attachments/assets/efe391a2-6840-44b8-b87b-c9deba242e73)

 
## Terraform Module - Argo Rollouts | 🚀🚀🚀 
Argo Rollouts is a Kubernetes controller and set of CRDs which provide advanced deployment capabilities such as blue-green, canary, canary analysis, experimentation, and progressive delivery features to Kubernetes.
Rollouts integrates with ingress controllers and service meshes, leveraging their traffic shaping abilities to gradually shift traffic to the new version during an update. Additionally, Rollouts can query and interpret metrics from various providers to verify key KPIs and drive automated promotion or rollback during an update.

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

🔨 Example : Config

```
###-Canary Release
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
        app: canary
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

---

###-Blue-Green
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: bluegreen
  labels:
    app: bluegreen
spec:
  replicas: 3
  revisionHistoryLimit: 1
  selector:
    matchLabels:
      app: bluegreen
  template:
    metadata:
      labels:
        app: bluegreen
    spec:
      containers:
      - name: bluegreen
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
    blueGreen:
      autoPromotionEnabled: false
      activeService: bluegreen
      previewService: bluegreen-preview
```
