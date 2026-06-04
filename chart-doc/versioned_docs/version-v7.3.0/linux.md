---
sidebar_position: 1
---

# Linux / MAC

**Estimate time: 20 minutes (approximate)**

### Prerequisites

Before starting the installation, ensure your system meets the following requirements:

#### 1. Software Installation & Configuration
* **Docker**: Install Docker Engine. Follow the official guides for [Linux](https://docs.docker.com/desktop/install/linux/) or [MAC](https://docs.docker.com/desktop/install/mac/).
* **Docker Compose (V2)**: Ensure `docker compose` is installed and accessible.
* **Permissions**: Ensure your user account has permissions to run Docker commands (added to the `docker` group) or you have `sudo` access.

#### 2. System Resource Requirements
Running the entire formsflow.ai stack requires sufficient system resources. Ensure Docker is allocated at least:
* **Memory (RAM)**: Minimum **4 GB** (8 GB or more recommended). *Your machine should have at least 8 GB-16 GB total RAM.*
* **CPUs**: Minimum **4 Cores**.
* **Disk Space**: At least **10 GB** of free disk space.

#### 3. General Requirements
* **Active Internet Connection**: Required to download the installer ZIP and pull Docker images (approx. 2-3 GB).
* **Web Browser**: A modern web browser (e.g., Google Chrome, Mozilla Firefox, Safari) to access the formsflow.ai UI.

## Step 1: Download the GitHub Repository

In this initial step, download the **Forms Flow AI Deployment** GitHub repository by simply clicking [**` Here `**](https://github.com/AOT-Technologies/forms-flow-ai-deployment/archive/refs/heads/release/v7.3.0.zip)

A zip file will be downloaded.


## Step 2: Extract the downloaded .zip file

![extracted folder preview](../static/img/linux/extracted.png)


**Now double click and open the exctracted folder and go to the `scripts` directory:**

![scripts dir preview](../static/img/linux/scripts-dir.png)


There you can see  **install.bash** file:

![install file preview](../static/img/linux/install-file.png)


Now right click anywhere in the file manager  and click **open in terminal**:

![.bat file preview](../static/img/linux/open-in-terminal.png)


Now type `sudo su` in the terminal to gain elevated privilages whcih is required for the installation procedure:

![sudo permissions](../static/img/linux/sudo-su.png)
- You can type `ls` to see the **install.sh** file there


## Step 3: Install using install.bash file

Type `./install.sh` to start the installation:

- It may ask for **Do you want to continue** because it may not tested in the Docker version you have. Just enter **'y'** and proceed with the installation.

![installation start](../static/img/linux/installation-start.png)


a) Verify the IP address is valid or incorrect after that. If true, provide **‘y’** as the answer, or else answer **‘n’**:

![IP Address prompt](../static/img/linux/ip-address-prompt.png)

b) Now to install “Analytics” enter ‘y’:

![Redash analytics prompt](../static/img/linux/analytics-prompt.png)
- If you need Redash Analytics Engine in the installation, provide **‘y’** as the answer, or else answer **‘n’**. (To know more about Redash Analytics Engine, please visit [Redash](https://redash.io/help/) ).

c) Now it will ask for *Redash API Key*:
![redash api key prompt](../static/img/7.3.0/linux/redash-prompt.png)
- The Redash application should be available for use at port defaulted to 7000. Open http://localhost:7001/ on your machine and register with any valid credentials:
 ![redash landing page](../static/img/7.3.0/redash-landing.png)

- Then the API Key can be found at settings > Account > API Key.
 ![redash api page](../static/img/7.3.0/linux/redash-api-page.png)

d) After the installation process has been completed it will show **formsflow.ai is successfully installed**.

![Completed](../static/img/linux/complete.png)

Once successfully installed, you can access the formsflow.ai web application in your browser via `http://localhost:3000` or `http://{your-ip-address}:3000`.

![Web Application](../static/img/web-application.png)

---

## Step 4: Mail-Configuration

For the **email-configuration**, follow the steps below:

![configuration folder](../static/img/linux/config-dir.png)

Create a folder inside the configuration folder(Inside docker-compose directory) named **bpm-mail-config**.

![mail configuration file](../static/img/linux/config.png)

Create a file name **mail.config.properties** inside the **bpm-mail-config** folder that just created and copy the below contents and update the values as needed:

```bash
# Send mails via SMTP. The given settings are for Gmail 
mail.transport.protocol=smtp

mail.smtp.host=smtp.gmail.com
mail.smtp.port=465
mail.smtp.auth=true
mail.smtp.ssl.enable=true
mail.smtp.socketFactory.port=465
mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory

# Poll mails via IMAPS.
mail.store.protocol=imaps
mail.imaps.host=imap.gmail.com
mail.imaps.port=993
mail.imaps.timeout=10000

mail.sender=donotreply
mail.sender.alias=DoNotReply

mail.attachment.download=true
mail.attachment.path=attachments

# Credentials
mail.user=CHANGEME@gmail.com
mail.password=CHANGEME

```

- Now run the container to verify the changes.


## Verifying the Installation status

> The following applications will be started and can be accessed in your browser.

 Srl No | Service Name | Usage | Access | Default credentials (userName / Password)|
--- | --- | --- | --- | --- 
1|`Keycloak`|Authentication|`http://localhost:8080`| `admin/changeme`
2|`forms-flow-forms`|form.io form building (Note: Form.io UI is disabled by default). This must be started earlier for resource role id's creation|`http://localhost:3001`|`admin@example.com/changeme`
3|`forms-flow-analytics`|Redash analytics server, This must be started earlier for redash key creation|`http://localhost:7001`|Use the credentials used for registration / [Default user credentials](https://github.com/AOT-Technologies/forms-flow-ai-deployment/blob/main/docs/forms-flow-ai-properties.md)
4|`forms-flow-web`|formsflow Landing web app|`http://localhost:3000`|[Default user credentials](https://github.com/AOT-Technologies/forms-flow-ai-deployment/blob/main/docs/forms-flow-ai-properties.md)
5|`forms-flow-api`|API services|`http://localhost:5001`|`Authorization tocken from keycloak role based user credentials`
6|`forms-flow-bpm`|Camunda integration|`http://localhost:8000/camunda`| [Default user credentials](https://github.com/AOT-Technologies/forms-flow-ai-deployment/blob/main/docs/forms-flow-ai-properties.md)
7|`forms-flow-data-layer`|GraphQL integration|`http://localhost:5500/queries`| 


## Uninstall Formsflow (Optional)

Uninstalling is optional and can be performed whenever you wish to remove the formsflow.ai stack from your machine. To uninstall formsflow installed through quick installation, follow the steps:
- Go to the folder you exctracted earlier and go to the `scripts` directory
  - There you can see **uninstall.bash** file
- Now right click anywhere in the file manager  and click **open in terminal**:
- Type `chmod +x uninstall.bash` to give executable permission to the file
  - If you type `ls` you can see the uninstall.bash in green color means it has now executable permission
- Now just type `./uninstall.bash`
  - It will prompt you **to uninstall formsflow.ai installation** click **'y'** and proceed with the installation.

![uninstall formsflow](../static/img/linux/uninstall.png)


If you face any issues while installing ,please connect with [us](https://github.com/AOT-Technologies/forms-flow-ai/issues).

---

## Frequently Asked Questions (FAQ)

This section covers common issues, error messages, and troubleshooting steps encountered during the installation and setup of formsflow.ai.

### Q: "Port is already allocated" or connection errors (e.g., port 3000, 8080, 3001)
> [!WARNING]
> This error occurs when another process or service is already running on one of the ports formsflow.ai requires.

* **Possible Cause**: You have a local database, a web application, or another Docker container running on ports like `8080` (Keycloak), `3000` (Web app), `3001` (Forms), or `7001` (Analytics).
* **Solution**:
  1. Find and terminate the process using the port:
     * **Windows (PowerShell)**:
       ```powershell
       netstat -ano | findstr :3000
       # Stop the process using the PID found
       Stop-Process -Id <PID> -Force
       ```
     * **Linux / MAC**:
       ```bash
       sudo lsof -i :3000
       # Kill the process
       kill -9 <PID>
       ```
  2. Alternatively, modify the port mapping in the `docker-compose.yml` file under the respective service.

### Q: Container exits with Code 137 (Out of Memory)
> [!IMPORTANT]
> Running the entire formsflow.ai stack requires minimum resource allocations in Docker.

* **Possible Cause**: Docker Desktop is running with default low memory configurations (e.g., 2 GB), causing database or Camunda containers to get killed by the OOM killer.
* **Solution**:
  1. Open Docker Desktop settings.
  2. Navigate to **Resources** > **Advanced** (or resource allocation settings).
  3. Increase Memory allocation to at least **4 GB to 8 GB** and CPUs to **4**.
  4. Click **Apply & restart** and run the install script again.

### Q: "Invalid redirect_uri" error on Keycloak login page
* **Possible Cause**: The application client redirect URLs registered in Keycloak do not match the IP address or host domain name you are currently using to access the site.
* **Solution**:
  1. Access the Keycloak Admin Console at `http://localhost:8080/auth/admin` (default credentials: `admin/changeme`).
  2. Navigate to **Clients** > select the client (e.g., `forms-flow-web`).
  3. Under **Valid Redirect URIs**, add your current access URL (e.g., `http://{your-ip-address}:3000/*` or `http://localhost:3000/*`).
  4. Add the URL to **Web Origins** as well to prevent CORS issues.
  5. Click **Save**.

### Q: Services fail to authenticate or token validation fails
* **Possible Cause**: The host IP or domain configured in Keycloak does not resolve correctly inside the Docker network.
* **Solution**:
  * Double-check your `.env` settings. Ensure `KEYCLOAK_URL` is set to the host IP (e.g., `http://{your-ip-address}:8080`) instead of `http://localhost:8080` if containers need to reach it externally.

### Q: "Connection refused" or database migration fails on startup
> [!NOTE]
> DB startup order is managed, but slow disk I/O can occasionally cause other services to start before the database is ready.

* **Possible Cause**: PostgreSQL or MongoDB has not finished initializing schema structures when Camunda or the Web API starts up.
* **Solution**:
  1. Simply restart the containers using:
     ```bash
     docker compose restart forms-flow-bpm forms-flow-api
     ```
  2. If the database credentials were changed, ensure the corresponding variables (`POSTGRES_USER`, `POSTGRES_PASSWORD`, etc.) are updated and match across all dependent services in `docker-compose.yml`.

### Q: PersistentVolumeClaim (PVC) stuck in Pending state
* **Possible Cause**: No default StorageClass is defined in your Kubernetes cluster, or your cluster lacks the necessary provisioner (e.g., AWS EBS CSI Driver).
* **Solution**:
  1. Check the status of PVCs:
     ```bash
     kubectl get pvc -n <namespace>
     ```
  2. Ensure the Amazon EBS CSI driver is installed and has the correct IAM permissions configured on your EKS cluster:
     ```bash
     helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
     helm repo update
     helm install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver \
       --namespace kube-system
     ```

### Q: Ingress Controller fails to route traffic or returns 502/503 Service Unavailable
* **Possible Cause**: The Nginx ingress controller pod is not running, or ingress hostnames do not match target service ports.
* **Solution**:
  1. Verify the Ingress pods are running:
     ```bash
     kubectl get pods -n ingress-nginx
     ```
  2. Ensure the Helm values have the correct service hostname matching the custom domain setup:
     ```yaml
     ingress:
       hostname: app.yourdomain.com
     ```
