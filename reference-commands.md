# Kubernetes PersistentVolume Setup Command Reference Guide

### Project content table
- [Section 1: Core Project Workflow](#section-1-core-project-workflow)
- [Section 2: Advanced Operations](#section-2-advanced-operations)
- [Section 3: Production Guide](#section-3-production-guide)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Complete command reference for Kubernetes storage setup  
> **Learning Focus**: Storage orchestration in Kubernetes  
> **Note**: Review commands before execution in production environments.

## Section 1: Core Project Workflow

### Step 1: Create and Verify PersistentVolume
```bash
# Apply the PV configuration
kubectl apply -f logs-pv.yaml

# Verify PV creation
kubectl get pv logs-pv -o wide

# Check PV details
kubectl describe pv logs-pv

# Verify PV status
kubectl get pv logs-pv --show-labels
```

### Step 2: Create and Verify PersistentVolumeClaim
```bash
# Apply the PVC configuration
kubectl apply -f logs-pvc.yaml

# Verify PVC creation
kubectl get pvc logs-pvc -o wide

# Check PVC binding status
kubectl describe pvc logs-pvc

# Verify PVC labels
kubectl get pvc logs-pvc --show-labels
```

### Step 3: Deploy and Verify Nginx Pod
```bash
# Deploy the Pod
kubectl apply -f nginx-pod.yaml

# Check Pod status
kubectl get pod nginx -o wide

# Verify Pod details
kubectl describe pod nginx

# Check Pod labels
kubectl get pod nginx --show-labels
```

### Final Step: Cleanup
```bash
# Delete Pod
kubectl delete pod nginx

# Delete PVC
kubectl delete pvc logs-pvc

# Delete PV
kubectl delete pv logs-pv

# Verify all resources are removed
kubectl get pod,pvc,pv
```

## Section 2: Advanced Operations

### Advanced Storage Management
```bash
# Check storage class
kubectl get storageclass

# List all PV and PVC
kubectl get pv,pvc

# Check volume bindings
kubectl get pv,pvc --show-labels

# View detailed storage information
kubectl describe pv logs-pv | grep -A5 Status
kubectl describe pvc logs-pvc | grep -A5 Status
```

### Troubleshooting
```bash
# View Pod events
kubectl get events --field-selector involvedObject.name=nginx

# Check Pod logs
kubectl logs nginx

# Verify volume mounts
kubectl exec nginx -- mount | grep nginx

# Check volume permissions
kubectl exec nginx -- ls -la /var/log/nginx

# View Pod configuration
kubectl get pod nginx -o yaml
```

## Section 3: Production Guide

### Production Setup
```bash
# Check node resources
kubectl describe node

# Verify storage classes
kubectl get storageclass -o wide

# Check resource quotas
kubectl describe resourcequota
```

### Monitoring
```bash
# Monitor PV/PVC status
kubectl get pv,pvc --watch

# Check system events
kubectl get events --sort-by=.metadata.creationTimestamp

# View Pod metrics
kubectl top pod nginx
```

### Backup Operations
```bash
# Create backup directory
mkdir -p ~/storage-backup

# Copy logs from Pod
kubectl cp nginx:/var/log/nginx/ ~/storage-backup/

# Verify backup
ls -la ~/storage-backup/nginx/
```

## Learning Notes

1. Always verify resource creation with kubectl get and describe
2. Monitor events for troubleshooting binding issues
3. Check Pod logs and events for mount problems
4. Use --watch flag for real-time monitoring
5. Maintain regular backups of persistent data

---

> 💡 **Best Practice**: Verify resource status after each creation step

> ⚠️ **Warning**: Always backup data before cleanup operations

> 📝 **Note**: Monitor events for binding and mounting issues