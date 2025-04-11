# Deploying a Java Web Application on AWS EC2 with Tomcat

This guide outlines the steps to deploy a Java web application (onlinebookstore) on an AWS EC2 instance using Tomcat.

## STEP 1: Launch EC2 Instance

1.  **Launch Instance:**
    * Navigate to the EC2 dashboard in the AWS Management Console.
    * Click "Launch instance.
    * Provide a name for your instance.
    * Select an appropriate Amazon Machine Image (AMI) (e.g., Ubuntu).
    * Choose or create a key pair for SSH access.
    * Configure security group settings to allow inbound traffic on port 8080.

## STEP 2: Connect to EC2 via SSH

1.  **SSH Connection:**
    * Open a terminal and use the following command to connect to your EC2 instance:
        ```bash
        ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
        ```
    * Replace `your-key.pem` with the path to your key pair file and `<your-ec2-public-ip>` with the public IP address of your EC2 instance.
2.  **Install Git (if needed):**
    * Check if Git is installed:
        ```bash
        git --version
        ```
    * If Git is not installed, install it:
        ```bash
        sudo apt update
        sudo apt install git -y
        ```
3.  **Clone Repository:**
    * Clone the onlinebookstore repository:
        ```bash
        git clone https://github.com/shashirajraja/onlinebookstore.git
        ```
4.  **Navigate to Repository:**
    * Change directory to the cloned repository:
        ```bash
        cd onlinebookstore
        ```

## STEP 3: Install Maven

1.  **Install Maven:**
    ```bash
    sudo apt install maven -y
    ```
2.  **Check Maven Version:**
    ```bash
    mvn -version
    ```
3.  **Maven Build:**
    ```bash
    mvn validate
    mvn test
    mvn compile
    mvn package
    ```
    * The `mvn package` command will generate the `onlinebookstore.war` file in the `target` directory.

## STEP 4: Install Tomcat

1.  **Navigate to /opt:**
    ```bash
    cd /opt
    ```
2.  **Download Tomcat:**
    ```bash
    wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.102/bin/apache-tomcat-9.0.102.tar.gz 
    ```
3.  **Extract Tomcat:**
    ```bash
    tar -xvzf apache-tomcat-9.0.102.tar.gz
    ```
4.  **Navigate to Tomcat Bin Directory:**
    ```bash
    cd apache-tomcat-9.0.102/bin
    ```
5.  **Start Tomcat:**
    ```bash
    ./startup.sh
    ```

## STEP 5: Access Tomcat Server

1.  **Access Tomcat via Browser:**
    * Open a web browser and enter the public IP address of your EC2 instance. You should see the Tomcat default page.
2.  **Configure Tomcat Manager Access:**
    * Find the context.xml files:
        ```bash
        find /opt/apache-tomcat-9.0.102/ -name context.xml
        ```
    * Edit the context.xml files located in `./webapps/host-manager/META-INF/context.xml` and `./webapps/manager/META-INF/context.xml`. Comment out or remove the valve that restricts access to the manager applications.
    * Navigate to the Tomcat configuration directory:
        ```bash
        cd /opt/apache-tomcat-9.0.102/conf
        ```
    * Edit the `tomcat-users.xml` file and add user credentials for the manager and host-manager roles:
        ```xml
        <tomcat-users>
            <user username="admin" password="yourpassword" roles="manager-gui,admin-gui,manager-script,host-manager"/>
        </tomcat-users>
        ```
    * Replace `yourpassword` with a strong password.
    * Restart Tomcat.
3.  **Login to Tomcat Manager:**
    * Refresh the browser and access the Tomcat Manager application (e.g., `http://<your-ec2-public-ip>/manager/html`).
    * Log in with the credentials you added to `tomcat-users.xml`.

## STEP 6: Deploy the Web Application

1.  **Copy WAR File to Tomcat Webapps Directory:**
    ```bash
    cp /home/ubuntu/onlinebookstore/target/onlinebookstore.war /opt/apache-tomcat-9.0.102/webapps/
    ```
    ![](./Images/29.png)
    
2.  **Access the Application:**
    * Tomcat will automatically deploy the WAR file.
    * Open a web browser and access the application using the URL: `http://<your-ec2-public-ip>/onlinebookstore`.
    * You should see the onlinebookstore web page.
    ![](./Images/book.png)