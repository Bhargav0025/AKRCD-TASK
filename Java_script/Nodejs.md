# Deploying a React Application on AWS EC2 with Apache2

This guide outlines the steps to deploy a React application (CollectTracker) on an AWS EC2 instance using Apache2.

## STEP 1: Launch EC2 Instance

1.  **Launch Instance:**
    * Navigate to the EC2 dashboard in the AWS Management Console.
    * Click "Launch instance.
    * Provide a name for your instance.
    * Select an appropriate Amazon Machine Image (AMI) (e.g., Ubuntu).
    * Choose or create a key pair for SSH access.
    * Configure security group settings to allow inbound traffic on port 80 (HTTP).

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
    * Clone the CollectTracker repository:
        ```bash
        git clone https://github.com/Bighairymtnman/CollectTracker.git
        ```
4.  **Navigate to Repository:**
    * Change directory to the cloned repository:
        ```bash
        cd CollectTracker
        ```

## STEP 3: Install Dependencies

1.  **Install Node.js:**
    ```bash
    sudo apt update
    sudo apt install nodejs -y
    ```
2.  **Check Node.js Version:**
    ```bash
    node -v
    ```
3.  **Install npm:**
    ```bash
    sudo apt install npm -y
    ```
4.  **Check npm Version:**
    ```bash
    npm -v
    ```
5.  **Navigate to Client Directory:**
    ```bash
    cd Client
    ls
    ```
6.  **Build the React Application:**
    ```bash
    cd CollectTicket
    npm run build
    ```
    * This command will create a `build` folder containing the production-ready application.

## STEP 4: Install Apache2 to Deploy

1.  **Install Apache2:**
    ```bash
    sudo apt update
    sudo apt install apache2 -y
    ```
2.  **Start Apache2:**
    ```bash
    sudo systemctl start apache2
    ```
3.  **Copy Build Files to Apache2's html directory:**
    ```bash
    sudo cp -r ~/CollectTracker/Client/CollectTicket/build/* /var/www/html/
    ```
    ![](./Images/tracke.png)

## STEP 5: Access the Application

1.  **Access Application via Browser:**
    * Open a web browser and enter the public IP address of your EC2 instance.
    * You should see the CollectTracker application.
    ![](./Images/track.png)