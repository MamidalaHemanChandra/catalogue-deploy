If you want your Jenkins agent in the default VPC to access an EKS cluster in roboshop-dev VPC, then VPC Peering is a valid and common solution.

Here’s a clean, practical setup guide.

✅ VPC Peering Solution (Default VPC ↔ roboshop-dev VPC)
🔷 1. Create VPC Peering Connection

Go to AWS Console:

VPC → Peering Connections → Create Peering Connection

Requester VPC: Default VPC (Jenkins)
Accepter VPC: roboshop-dev VPC (EKS)

Then:

Accept the request in the roboshop-dev VPC account/region
🔷 2. Update Route Tables (VERY IMPORTANT)
In Default VPC (Jenkins side)

Add route:

Destination	Target
roboshop-dev VPC CIDR (e.g. 10.x.x.x/16)	pcx-xxxx
In roboshop-dev VPC (EKS side)

Add route:

Destination	Target
Default VPC CIDR (e.g. 172.31.0.0/16)	pcx-xxxx
🔷 3. Security Group Rules
EKS Control Plane / API Access

If EKS endpoint is private or restricted:

Allow inbound:

Port: 443
Source: Default VPC CIDR or Jenkins SG
Worker Nodes (if needed)

If Jenkins interacts with pods/services:

Allow required service ports (NodePort, 80, 443, etc.)
🔷 4. DNS Resolution (Important for EKS)

Enable in peering connection:

✅ Allow DNS resolution from peer VPC

Also ensure:

EKS VPC has:
enableDnsHostnames = true
enableDnsSupport = true
🔷 5. Verify Connectivity from Jenkins Agent

From Jenkins EC2:

ping <EKS VPC private IP>

Test Kubernetes API:

curl -k https://<eks-cluster-endpoint>

Or:

aws eks update-kubeconfig --name roboshop-dev --region <region>
kubectl get nodes
⚠️ Key Limitations of VPC Peering
❌ No transitive routing (A ↔ B ↔ C won’t work)
❌ CIDR blocks must NOT overlap
❌ Manual route management required
💡 When VPC Peering is NOT ideal

If you plan to scale architecture (many VPCs), prefer:

AWS Transit Gateway (better long-term)
Private Jenkins inside EKS VPC (simplest DevOps design)
🧠 Recommended Architecture (Best Practice)

For CI/CD with EKS:

👉 Put Jenkins agent inside roboshop-dev VPC (same VPC as EKS)
OR
👉 Use Transit Gateway if multiple environments exist

If you want, I can help you next with:
Terraform for VPC Peering setup
Jenkins pipeline to deploy to EKS
Fixing kubeconfig / IAM auth issues after peering
Transit Gateway design vs Peering comparison

