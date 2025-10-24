# Production Deployment Guide

## Overview

This guide provides step-by-step instructions for deploying the Recruitment Platform to a production environment using Kubernetes, Docker, and modern DevOps practices.

## Prerequisites

### Infrastructure Requirements
- **Kubernetes Cluster**: 1.25+ with at least 3 worker nodes
- **Load Balancer**: NGINX Ingress Controller or similar
- **Storage**: Persistent volumes for database and file storage
- **DNS**: Domain names pointing to your cluster
- **SSL Certificates**: Let's Encrypt or custom certificates

### Software Requirements
- **kubectl**: Configured to access your cluster
- **Docker**: For building custom images
- **Helm**: For package management (optional)

## 1. Environment Setup

### 1.1 Create Namespace

```bash
kubectl create namespace recruitment-platform
kubectl config set-context --current --namespace=recruitment-platform
```

### 1.2 Install Ingress Controller

```bash
# Using Helm
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace recruitment-platform \
  --set controller.replicas=2
```

### 1.3 Install Cert-Manager

```bash
# Using Helm
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true
```

## 2. Secrets Management

### 2.1 Database Secrets

```bash
kubectl create secret generic database-secrets \
  --namespace recruitment-platform \
  --from-literal=sa-password="YourStrongProdPassword" \
  --from-literal=connection-string="Server=recruitment-database;Database=Recruitment;User Id=recruitment_user;Password=YourStrongProdPassword;TrustServerCertificate=true;"
```

### 2.2 JWT Secrets

```bash
kubectl create secret generic jwt-secrets \
  --namespace recruitment-platform \
  --from-literal=secret="your-256-bit-secret-key-for-production"
```

### 2.3 MinIO Secrets

```bash
kubectl create secret generic minio-secrets \
  --namespace recruitment-platform \
  --from-literal=access-key="your-minio-access-key" \
  --from-literal=secret-key="your-minio-secret-key"
```

## 3. Storage Configuration

### 3.1 Persistent Volumes

```yaml
# database-pv.yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: sql-server-pv
spec:
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: fast-ssd
  # Configure based on your storage provider
```

### 3.2 Storage Classes

```yaml
# storage-class.yml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/aws-ebs  # or your provider
parameters:
  type: gp3
  fsType: ext4
```

## 4. Database Deployment

### 4.1 Deploy SQL Server

```bash
kubectl apply -f infrastructure/k8s/database-deployment.yml
```

### 4.2 Verify Database Deployment

```bash
kubectl get pods -l app=recruitment-database
kubectl get pvc
```

### 4.3 Run Database Migrations

```bash
# Get the migration pod
kubectl run migration-pod \
  --image=your-registry/recruitment-api:latest \
  --rm -it --restart=Never \
  --env="ConnectionStrings__DefaultConnection=Server=recruitment-database-service;Database=Recruitment;User Id=sa;Password=YourStrongProdPassword;TrustServerCertificate=true;" \
  -- dotnet ef database update
```

## 5. Backend Deployment

### 5.1 Deploy Backend API

```bash
kubectl apply -f infrastructure/k8s/backend-deployment.yml
```

### 5.2 Verify Backend Deployment

```bash
kubectl get pods -l app=recruitment-api
kubectl get services
kubectl get ingress
```

### 5.3 Check API Health

```bash
curl https://api.yourplatform.com/health
```

## 6. Supporting Services

### 6.1 Deploy Redis

```yaml
# redis-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: recruitment-platform
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:7-alpine
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "256Mi"
            cpu: "100m"
          limits:
            memory: "512Mi"
            cpu: "200m"
```

### 6.2 Deploy MinIO

```yaml
# minio-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: minio
  namespace: recruitment-platform
spec:
  replicas: 1
  selector:
    matchLabels:
      app: minio
  template:
    metadata:
      labels:
        app: minio
    spec:
      containers:
      - name: minio
        image: minio/minio:latest
        ports:
        - containerPort: 9000
        - containerPort: 9001
        env:
        - name: MINIO_ACCESS_KEY
          valueFrom:
            secretKeyRef:
              name: minio-secrets
              key: access-key
        - name: MINIO_SECRET_KEY
          valueFrom:
            secretKeyRef:
              name: minio-secrets
              key: secret-key
        command: ["minio", "server", "/data", "--console-address", ":9001"]
        volumeMounts:
        - name: minio-data
          mountPath: /data
      volumes:
      - name: minio-data
        persistentVolumeClaim:
          claimName: minio-storage-pvc
```

## 7. AI Services Deployment

### 7.1 Deploy CV Parser

```bash
kubectl apply -f infrastructure/k8s/ai-cv-parser-deployment.yml
```

### 7.2 Deploy Job Matcher

```bash
kubectl apply -f infrastructure/k8s/ai-job-matcher-deployment.yml
```

## 8. Frontend Deployment

### 8.1 Deploy Web Application

```yaml
# frontend-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recruitment-web
  namespace: recruitment-platform
spec:
  replicas: 2
  selector:
    matchLabels:
      app: recruitment-web
  template:
    metadata:
      labels:
        app: recruitment-web
    spec:
      containers:
      - name: web
        image: your-registry/recruitment-web:latest
        ports:
        - containerPort: 80
        env:
        - name: REACT_APP_API_URL
          value: "https://api.yourplatform.com/api"
        - name: REACT_APP_WS_URL
          value: "wss://api.yourplatform.com"
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
```

## 9. Monitoring Setup

### 9.1 Deploy Prometheus

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace
```

### 9.2 Deploy Grafana

```bash
helm install grafana prometheus-community/grafana \
  --namespace monitoring \
  --set adminPassword="YourAdminPassword"
```

### 9.3 Deploy ELK Stack

```bash
kubectl apply -f infrastructure/monitoring/elk-stack.yml
```

## 10. Security Configuration

### 10.1 Network Policies

```yaml
# network-policy.yml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: recruitment-api-policy
  namespace: recruitment-platform
spec:
  podSelector:
    matchLabels:
      app: recruitment-api
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: recruitment-web
    - namespaceSelector:
        matchLabels:
          name: ingress-nginx
  egress:
  - to: []
    ports:
    - protocol: TCP
      port: 1433  # SQL Server
    - protocol: TCP
      port: 6379  # Redis
    - protocol: TCP
      port: 9200  # Elasticsearch
```

### 10.2 Pod Security Standards

```yaml
# pod-security-policy.yml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: recruitment-platform-psp
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
    - ALL
  volumes:
    - 'configMap'
    - 'secret'
    - 'persistentVolumeClaim'
  runAsUser:
    rule: 'MustRunAsNonRoot'
  seLinux:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'MustRunAs'
    ranges:
    - min: 1
      max: 65535
```

## 11. Scaling Configuration

### 11.1 Horizontal Pod Autoscaling

```yaml
# hpa.yml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: recruitment-api-hpa
  namespace: recruitment-platform
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: recruitment-api
  minReplicas: 3
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

### 11.2 Cluster Autoscaling

Configure your cloud provider's cluster autoscaler to automatically adjust node count based on resource demands.

## 12. Backup Strategy

### 12.1 Database Backups

```yaml
# database-backup-cronjob.yml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: database-backup
  namespace: recruitment-platform
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: mcr.microsoft.com/mssql/tools:latest
            command:
            - /bin/bash
            - -c
            - |
              # Backup database
              sqlcmd -S recruitment-database-service -U sa -P $(SA_PASSWORD) \
                -Q "BACKUP DATABASE Recruitment TO DISK='/backup/recruitment_$(date +%Y%m%d_%H%M%S).bak'"
            env:
            - name: SA_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: database-secrets
                  key: sa-password
          restartPolicy: OnFailure
```

### 12.2 Configuration Backups

```bash
# Backup Kubernetes manifests
kubectl get all,ingress,svc,pvc,pv,secret,configmap \
  --namespace recruitment-platform \
  -o yaml > backup/recruitment-platform-backup-$(date +%Y%m%d_%H%M%S).yml
```

## 13. Health Checks and Monitoring

### 13.1 Application Health Checks

```bash
# Check API health
curl -f https://api.yourplatform.com/health

# Check database connectivity
kubectl exec deployment/recruitment-database -- \
  /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P $SA_PASSWORD -Q "SELECT 1"

# Check Redis connectivity
kubectl exec deployment/redis -- redis-cli ping
```

### 13.2 Monitor Resource Usage

```bash
# Check pod resource usage
kubectl top pods --namespace recruitment-platform

# Check node resource usage
kubectl top nodes

# Check persistent volume usage
kubectl exec deployment/recruitment-database -- df -h
```

## 14. Troubleshooting

### 14.1 Common Issues

**Pods Not Starting**
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

**Services Not Accessible**
```bash
kubectl get endpoints
kubectl get ingress
```

**High Resource Usage**
```bash
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```

**Database Connection Issues**
```bash
kubectl exec deployment/recruitment-api -- \
  dotnet ef database update
```

### 14.2 Log Analysis

```bash
# View application logs
kubectl logs -l app=recruitment-api --tail=100

# View database logs
kubectl logs -l app=recruitment-database

# View ELK stack logs
kubectl logs -l app=elasticsearch
kubectl logs -l app=logstash
kubectl logs -l app=kibana
```

## 15. Maintenance

### 15.1 Regular Updates

**Application Updates**
```bash
# Update backend image
kubectl set image deployment/recruitment-api api=your-registry/recruitment-api:v2.0.0

# Update frontend image
kubectl set image deployment/recruitment-web web=your-registry/recruitment-web:v2.0.0
```

**Security Updates**
```bash
# Update base images
kubectl rollout restart deployment/recruitment-api
kubectl rollout restart deployment/recruitment-web
```

### 15.2 Certificate Renewal

```bash
# Check certificate status
kubectl get certificates

# Force certificate renewal if needed
kubectl annotate certificate recruitment-api-tls \
  cert-manager.io/issue-temporary-certificate="true"
```

## 16. Rollback Procedures

### 16.1 Application Rollback

```bash
# Check rollout history
kubectl rollout history deployment/recruitment-api

# Rollback to previous version
kubectl rollout undo deployment/recruitment-api

# Rollback to specific revision
kubectl rollout undo deployment/recruitment-api --to-revision=2
```

### 16.2 Database Rollback

```bash
# Restore from backup
kubectl exec deployment/recruitment-database -- \
  /opt/mssql-tools/bin/sqlcmd \
  -S localhost \
  -U sa \
  -P $SA_PASSWORD \
  -Q "RESTORE DATABASE Recruitment FROM DISK='/backup/recruitment_previous.bak' WITH REPLACE"
```

## 17. Cost Optimization

### 17.1 Resource Optimization

**Right-size Pods**
```bash
# Analyze current resource usage
kubectl top pods --all-namespaces

# Adjust resource requests/limits based on actual usage
kubectl edit deployment recruitment-api
```

**Use Spot Instances** (if supported by your cloud provider)
```yaml
nodeSelector:
  cloud.google.com/gke-spot: "true"  # GCP
  # or
  # karpenter.sh/capacity-type: spot  # AWS
```

### 17.2 Storage Optimization

**Database Optimization**
- Implement read replicas for read-heavy workloads
- Archive old data to cheaper storage
- Implement database sharding if needed

**File Storage Optimization**
- Implement lifecycle policies for old files
- Use different storage classes for different access patterns
- Compress static assets

## 18. Performance Tuning

### 18.1 Application Tuning

**Backend Optimization**
```bash
# Enable response compression
# Configure connection pooling
# Implement Redis caching for frequently accessed data
```

**Database Optimization**
```sql
-- Create appropriate indexes
CREATE INDEX IX_Applications_Status ON Applications(Status);
CREATE INDEX IX_Vacancies_Status_Province ON Vacancies(Status, Province);

-- Update statistics
EXEC sp_updatestats;

-- Monitor slow queries
SELECT * FROM sys.dm_exec_query_stats ORDER BY total_elapsed_time DESC;
```

### 18.2 Infrastructure Tuning

**Kubernetes Optimization**
```bash
# Enable resource quotas
kubectl apply -f resource-quotas.yml

# Configure pod disruption budgets
kubectl apply -f pdb.yml

# Optimize node placement
kubectl apply -f node-affinity.yml
```

## 19. Security Hardening

### 19.1 Network Security

```yaml
# Restrict external access
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: recruitment-platform
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### 19.2 Secret Management

```bash
# Use external secret management (e.g., AWS Secrets Manager, Azure Key Vault)
kubectl apply -f external-secrets.yml

# Rotate secrets regularly
kubectl create job secret-rotation --from=cronjob/secret-rotation
```

## 20. Compliance and Auditing

### 20.1 Audit Logging

```yaml
# Enable audit logging in Kubernetes
apiVersion: audit.k8s.io/v1
kind: Policy
metadata:
  name: audit-policy
rules:
- level: Metadata
  namespaces: ["recruitment-platform"]
```

### 20.2 Data Retention

```yaml
# Log retention policy
apiVersion: v1
kind: ConfigMap
metadata:
  name: log-retention-policy
data:
  log-retention-days: "90"
  audit-log-retention-days: "2555"  # 7 years for compliance
```

## Support

For deployment issues:

- **Emergency Contact**: ops@yourplatform.com
- **Documentation**: https://docs.yourplatform.com/deployment
- **Monitoring Dashboard**: https://grafana.yourplatform.com
- **Status Page**: https://status.yourplatform.com

## Checklist

- [ ] Infrastructure prerequisites installed
- [ ] Secrets and configuration created
- [ ] Storage volumes provisioned
- [ ] Database deployed and migrated
- [ ] Backend API deployed and healthy
- [ ] Supporting services deployed
- [ ] Frontend deployed and accessible
- [ ] Monitoring and alerting configured
- [ ] Security policies applied
- [ ] Backup strategy implemented
- [ ] Documentation updated

---

**Congratulations!** Your Recruitment Platform is now deployed to production! 🎉