# Interview_Questions

### **Que: What is a Terraform State File?**

A **Terraform state file** (`terraform.tfstate`) is a file that **tracks the current state** of your infrastructure managed by Terraform.

### **Why is it Needed?**

- Terraform **compares the state file** with the actual cloud infrastructure to know what needs to be **added, changed, or deleted**.
- Without it, Terraform wouldn't know what resources already exist.

### **How Does It Work?**

1. When you run `terraform apply`, Terraform creates or updates resources and **stores their details** (like IDs, IPs) in the **state file**.
2. The next time you run Terraform, it reads the state file to decide what changes are needed.

---

**Que: how to take Jenkins servers backups**

**method:1**

**Use a Backup Plugin**

Jenkins has plugins that automate the backup process. Some popular options include:

- **ThinBackup**: A lightweight plugin for scheduling and managing backups.
- **Backup Plugin**: Another plugin for creating and restoring backups.

### a. Steps to Use ThinBackup:

1. Install the **ThinBackup** plugin:
    - Go to **Manage Jenkins > Manage Plugins > Available** and search for "ThinBackup".
2. Configure ThinBackup:
    - Go to **Manage Jenkins > ThinBackup**.
    - Set the backup directory, schedule, and options.
3. Run a manual backup or wait for the scheduled backup.

### method:2

**Copying the `JENKINS_HOME` Directory (Simplest, but Requires Server Downtime):**

This is the most straightforward approach: you simply copy the entire `JENKINS_HOME` directory. However, it requires Jenkins to be shut down to ensure data consistency. Therefore, it is *not ideal for production servers*.

- **How to do it:**
    1. Stop Jenkins gracefully.
    2. Copy the entire `JENKINS_HOME` directory to your backup location. You can use tools like `cp -rp` (Linux/Unix) or `xcopy` (Windows). Example (Linux):Bash
        
        `sudo systemctl stop jenkins  # Stop Jenkins
        cp -rp /var/lib/jenkins /path/to/backup/location/jenkins_backup_$(date +%Y%m%d%H%M%S)
        sudo systemctl start jenkins   # Restart Jenkins`
        
    3. Restart Jenkins.
- **Limitations:**
    - Requires downtime.
    - Can be slow for large instances.
    - Doesn't provide incremental backups.

---

Que: how to migrate docker images from one server to another server using docker save

**Save the Docker Image as a `.tar` File**:

- Use the `docker save` command to export the image to a `.tar` file:
    
    bash
    
    Copy
    
    ```
    docker save -o <output_file_name>.tar <image_name>:<tag>
    ```
    

**Step 2: Transfer the `.tar` File to the Destination Server**

### Using `scp`:

bash

Copy

```
scp <output_file_name>.tar <username>@<destination_server_ip>:/path/to/destination/
```

- Replace `<username>` with the username on the destination server.
- Replace `<destination_server_ip>` with the IP address or hostname of the destination server.
- Replace `/path/to/destination/` with the directory where you want to save the file on the destination server.

**Step 3: Load the Docker Image on the Destination Server**

- Use the `docker load` command to load the image from the `.tar` file:
    
    bash
    
    Copy
    
    ```
    docker load -i <output_file_name>.tar
    ```
    

---

**1. How to delete last 7 days logs.**

You can use the `find` command in Linux to delete logs older than 7 days:

```bash
find /var/log -type f -mtime +7 -exec rm -f {} \\;

```

- `/var/log`: Directory where logs are stored.
- `mtime +7`: Files modified more than 7 days ago.
- `exec rm -f {} \\;`: Executes the `rm` command to delete the files.

---

### **2. How do you check application logs.**

- Use `tail`, `cat`, or `less` commands:
    
    ```bash
    tail -f /var/log/application.log
    cat /var/log/application.log
    less /var/log/application.log
    
    ```
    
- For containerized apps, use `docker logs` or `kubectl logs`:
    
    ```bash
    docker logs <container_id>
    kubectl logs <pod_name>
    
    ```
    

---

### **3. How do you access a running container.**

- For Docker:
    
    ```bash
    docker exec -it <container_id> /bin/bash
    
    ```
    
- For Kubernetes:
    
    ```bash
    kubectl exec -it <pod_name> -- /bin/bash
    
    ```
    

---

### **4. How do you check disk space.**

Use the `df` command:

```bash
df -h

```

- `h`: Human-readable format.

---

### **5. How to check processes running.**

Use the `ps` command:

```bash
ps aux

```

- `aux`: Displays all running processes.

---

### **6. What is soft and hard links and difference.**

- **Hard Link**: Points directly to the inode of a file. Deleting the original file doesn’t affect the hard link.
- **Soft Link (Symbolic Link)**: Points to the file name. Deleting the original file makes the soft link invalid.

---

### **7. How to copy file local to remote.**

Use `scp`:

```bash
scp local_file.txt user@remote_host:/path/to/destination

```

---

### **8. How do you access an instance from CLI.**

Use SSH:

```bash
ssh -i <key.pem> user@public_ip

```

---

### **9. How do you manage secrets in k8s, Jenkins, Docker.**

- **Kubernetes**: Use `Secrets` objects.
- **Jenkins**: Use the Credentials plugin or HashiCorp Vault.
- **Docker**: Use Docker Secrets or environment variables.

---

### **10. What are layers in Docker.**

Docker images are made up of read-only layers. Each layer represents an instruction in the Dockerfile. Layers are cached to optimize build times.

---

### **11. What are different services in k8s.**

- **ClusterIP**: Internal service.
- **NodePort**: Exposes the service on a static port.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.
- **ExternalName**: Maps a service to an external DNS name.

---

### **12. How to access the application from k8s.**

- Use `kubectl port-forward`:
    
    ```bash
    kubectl port-forward <pod_name> 8080:80
    
    ```
    
- Expose the service using `NodePort` or `LoadBalancer`.

---

### **13. How to make high availability in k8s.**

- Use multiple master nodes for the control plane.
- Deploy applications across multiple nodes using `ReplicaSets` or `Deployments`.
- Use `PodDisruptionBudgets` and `HorizontalPodAutoscaler`.

---

### **14. How do we connect 3 containers running.**

- Use Docker networks:
    
    ```bash
    docker network create my_network
    docker run --network my_network --name container1 ...
    docker run --network my_network --name container2 ...
    docker run --network my_network --name container3 ...
    
    ```
    
- In Kubernetes, use `Services` and `ClusterIP`.

---

### **15. What is Docker mounts and volumes.**

- **Mounts**: Bind mounts or tmpfs mounts for temporary storage.
- **Volumes**: Persistent storage managed by Docker.

---

### **16. How to manage more containers in k8s.**

- Use `Deployments` and `ReplicaSets` to manage multiple containers.
- Use `Helm` for managing complex applications.

---

### **17. What happens when control and master node fails.**

- If the master node fails, the cluster becomes unavailable unless you have a highly available setup with multiple master nodes.

---

### **18. What is ingress and egress.**

- **Ingress**: Traffic coming into the network.
- **Egress**: Traffic going out of the network.
