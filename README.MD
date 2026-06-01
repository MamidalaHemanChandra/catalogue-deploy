Great. After setting up the VPC peering, verify these items in order:

### 1. Confirm Peering is Active

```bash
aws ec2 describe-vpc-peering-connections \
  --vpc-peering-connection-ids pcx-xxxxxxxx
```

Status should be:

```text
active
```

### 2. Verify Route Tables

**Default VPC route table**

```
Destination: <roboshop-dev CIDR>
Target: pcx-xxxxxxxx
```

**roboshop-dev VPC route table**

```
Destination: <default VPC CIDR>
Target: pcx-xxxxxxxx
```

### 3. Verify Security Groups

Allow traffic from the Jenkins VPC CIDR to the EKS cluster and worker nodes as needed, especially TCP 443.

### 4. Test from Jenkins Server

Check AWS authentication:

```bash
aws sts get-caller-identity
```

Update kubeconfig:

```bash
aws eks update-kubeconfig \
  --region <region> \
  --name <cluster-name>
```

Test cluster access:

```bash
kubectl get nodes
```

If `kubectl get nodes` works, Jenkins should be able to deploy to EKS successfully.

If you encounter an error (timeout, unauthorized, unable to connect, etc.), paste the exact message and I'll help diagnose it.
