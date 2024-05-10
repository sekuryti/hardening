# Kubernetes Hardening

### Secure EKS Cluster

#### Cloud Infrastructure Security
- **VPC Layout**: Design a VPC that isolates different environments and restricts access.
- **Dedicated IAM Role for EKS Cluster Creation**: Use dedicated IAM roles for creating and managing the EKS cluster to limit permissions.
- **Cluster Resource Tagging**: Implement tagging to manage resources effectively and track costs.
- **Control SSH Access to Nodes**: Restrict SSH access to nodes, preferably using a bastion host.
- **EC2 Security Groups for Nodes**: Configure security groups to tightly control inbound and outbound traffic to nodes.
- **Don’t Install the Kubernetes Dashboard**: Avoid using the Kubernetes dashboard as it can be a security risk if not properly secured.
- **AWS Fargate for Nodeless EKS**: Use AWS Fargate to run containers without managing servers or clusters, suitable for specific types of workloads.

#### Authentication and Authorization
- **--anonymous-auth=false**: Disable anonymous authentication to the kubelet's HTTPS endpoint.
- **X509 client certificate authentication**: Enable and enforce client certificate authentication for added security.
- **Authorization (RBAC, Node, ABAC, or Webhook)**: Use RBAC and avoid setting `--authorization-mode` to AlwaysAllow. Prefer Webhook mode for enhanced security.

#### Admission Control
- **Gatekeeper**: Use Gatekeeper for policy enforcement in the cluster.
- **Enable NodeRestriction admission plugin**: This limits a kubelet to only modify its own node resources.
- **AlwaysPullImages and DenyEscalatingExec**: Use these admission controllers to enforce security best practices.

#### Pod Security
- **Restrict privileged containers**: Ensure that containers do not run with privileged access unless absolutely necessary.
- **Run processes as non-root**: Use the `runAsUser` and `AllowPrivilegeEscalation=false` settings to enhance security.
- **Limit use of hostPath**: If necessary, restrict which prefixes can be used and set the volume as read-only to mitigate risks.

#### Network Security
- **Network Policies**: Implement network policies to control traffic flow at the IP address or port level.
- **Traffic control and Encryption**: Use AWS load balancers with encryption and define security groups and CNIs to manage network traffic securely.

#### Secrets Management
- **Kubernetes secrets**: Use Kubernetes secrets for managing sensitive data and ensure they are not stored in config maps.
- **Encryption at rest**: Ensure that secrets and other sensitive data are encrypted at rest.

#### Container and Image Security
- **Use trusted registries**: Only use images from trusted registries and ensure they are signed.
- **Image vulnerability scanning**: Regularly scan images for vulnerabilities.
- **Dockerfile best practices**: Avoid running containers as root, and do not expose the Docker daemon socket.

#### Runtime and Infrastructure Security
- **SELinux, AppArmor, and Seccomp**: Use these tools to enforce security policies and isolate resources.
- **Falco**: Implement Falco for runtime security monitoring and anomaly detection.
- **Periodic security assessments**: Use tools like kube-bench to assess compliance with CIS benchmarks.

#### Logging and Monitoring
- **Enable audit logs**: Ensure that all cluster activities are logged and audit logs are enabled.
- **Monitor logs**: Use tools like CloudWatch Logs Insights and Falco to analyze and monitor log data.

This guide incorporates current best practices for securing a Kubernetes environment, particularly for AWS EKS, and addresses various aspects from infrastructure setup to runtime security.

