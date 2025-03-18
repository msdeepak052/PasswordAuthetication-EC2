# PasswordAuthetication-EC2

login with username and password 

Changing PasswordAuthentication to yes in sshd_config is enough on Amazon Linux, but on RHEL PasswordAuthentication also appears in /etc/ssh/sshd_config.d/50-cloud-init.conf

To authenticate to your EC2 instance using a password instead of a PEM file, you need to enable password authentication and set a password for the user account you want to log in with. Here are the steps to do this:

# Step 1: Connect to Your EC2 Instance Using SSH Key
Since you currently have access to your EC2 instance using the PEM file, connect to it using SSH:


ssh -i /path/to/your-key.pem ubuntu@ec2-3-110-118-133.ap-south-1.compute.amazonaws.com

# Step 2: Set a Password for the User
Once you are logged in, set a password for the ubuntu user (or whichever user you want to use for password authentication):


sudo passwd ubuntu
You will be prompted to enter a new password. Make sure to choose a strong password.

# Step 3: Enable Password Authentication
Open the SSH configuration file:

sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf
PasswordAuthentication yes

If PasswordAuthentication is set to no, change it to yes. If it is commented out (with a #), uncomment it.
Save and exit the editor (in nano, press CTRL + X, then Y, and then Enter).

# Step 4: Restart the SSH Service
Restart the SSH service to apply the changes:

service ssh restart

# Step 5: Test Password Authentication
Open a new terminal and try to connect to your EC2 instance using the password:


ssh ubuntu@ec2-3-110-118-133.ap-south-1.compute.amazonaws.com
You should be prompted for a password. Enter the password you set earlier.

# Important Security Considerations

Security Risks: Enabling password authentication can expose your instance to brute-force attacks. It's generally recommended to use SSH key pairs for authentication.
Use Fail2Ban: Consider installing and configuring tools like Fail2Ban to protect against brute-force attacks.
Disable Password Authentication: If you no longer need password authentication, it's a good practice to revert the changes and disable it again.
