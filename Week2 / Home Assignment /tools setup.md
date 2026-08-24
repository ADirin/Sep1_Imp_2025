Below is a basic setup guideline for installing Git, Jenkins, Docker, and Kubernetes on a Windows operating system. Please note that the steps may vary slightly based on the specific versions available at the time of installation, so it's advisable to check for the latest versions.



## Install JDK

Make sure you have installed JDK, either version 17, 21, or 25 — for example, version 21 can be downloaded here: [JDK 21](https://www.oracle.com/java/technologies/javase/jdk21-archive-downloads.html).

You can check whether Java is already installed on your machine via the console:

​```
C:\> java --version
​```

- If you get a "java is not recognized" error, search for "JDK download" on Google and download it to your local machine's Program Files. After installation, copy the path: `C:\Program Files\Java\jdk-21\bin`
1. Open the Windows search bar and type "Edit the system environment variables"
![Edit System](/Images/EditSystem.jpg)

2. In the System Properties window, click "Environment Variables"

![Edit System](/Images/path.jpg)
4. Under System Variables, find "Path", click "Edit", then click "New"

![Edit System](/Images/new_path.jpg)
5. Paste the path `C:\Program Files\Java\jdk-21\bin` into the new field, then click "OK"




### 1. Install Git:
   - **Download Git:**
![GIT](/Images/git-github.gif)

     - Visit the official Git website: [Git Downloads](https://git-scm.com/downloads)
     - Download the latest version for Windows.
   - **Installation:**
     - Run the downloaded installer.
     - Follow the installation wizard, accepting the default settings.
     - Choose the appropriate options (e.g., adjusting the PATH environment) during installation.
   - **Verification:**
     - Open a command prompt and type `git --version` to ensure Git is installed and accessible.
     - if you receive error follow step 1-4 similar as JDK installation

### 2. Install Docker:

![Docker](/Images/docker_3.gif)

   - **Download Docker Desktop:**
     - Visit the official Docker website: [Docker Desktop](https://www.docker.com/products/docker-desktop)
     - Download the Docker Desktop for Windows.
   - **Installation:**
     - Run the downloaded installer.
     - Follow the installation wizard, accepting the default settings.
     - Ensure that Hyper-V is enabled on your Windows machine (Docker Desktop requires Hyper-V).
   - **Verification:**
     - Open a command prompt and type `docker --version` to confirm Docker installation.
     - Run `docker run hello-world` to test a basic Docker container.

### 3. Install Kubernetes (Minikube):
![Minikube](/Images/minikube.gif)
   - **Download Minikube:**
     - Visit the official Minikube GitHub repository:
     - [Minikube install](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)
     - Download the latest Minikube executable (.exe) file.
   - **Installation:**
     - Move the downloaded executable to a directory in your system PATH.
     - Open a command prompt and run `minikube version` to verify the installation.
   - **Start Minikube Cluster:**
     - Run `minikube start` to create a local Kubernetes cluster.
     - Minikube will automatically configure `kubectl` to use the Minikube cluster.
   - **Verification:**
     - Run `kubectl version` to ensure `kubectl` is configured correctly.
     - Run `kubectl get nodes` to confirm that Minikube is running.

### 4. Install Jenkins:
![Jenkins](/Images/jenkins_1.gif)

   - **Download Jenkins:**
     - Visit the official Jenkins website: [Jenkins Downloads](https://www.jenkins.io/download/)
     - Download the Windows installer package (usually a .zip or .msi file).
   - **Installation:**
     - Run the downloaded installer.
     - Follow the installation wizard, specifying the installation directory and choosing the recommended plugins.
   - **Access Jenkins:**
     - Once installed, Jenkins should be accessible at `http://localhost:8080/` in a web browser.
     - Follow the instructions to unlock Jenkins, providing the initial administrator password.
   - **Installation Complete:**
     - Continue with the Jenkins setup by installing additional plugins and configuring global settings.
     - **Note**
     - Jenkins works only jdk 11, 17 and 21 make sure you have install one of these JDK (do not forget to update the class path)
     - further instuction how to run jenkin in the lecture
       
