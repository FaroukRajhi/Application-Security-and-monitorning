# Learning Objectives

Learn how to install the pass secrets manager utility
Learn how to initialize pass with a Gnu Privacy Guard (GPG) key
Learn how to securely store secrets using the pass CLI (command-line-interface)
Learn how to retrieve stored secrets using the pass CLI (command-line-interface)
Learn how to clean up pass to secure your computer

# What is a Secure Development Environment?

As a developer, you must ensure that security isn’t an afterthought in your application’s development process. Security should be included throughout the software development lifecycle (SDLC), but it isn’t enough.

If the development environment isn’t secure, it’s difficult to accept that code developed there is also secure. There are several simple steps you can use to secure your development environment against risk:

Securely storing secrets required for your production application.
Secure the internet connection. Use a VPN, if necessary.
Implement a firewall with strong ingress/egress policies.
Regularly check for open ports and closing ports not needed.
Use Docker containers for development, if possible, and use separate computers for development tasks and business tasks.
Logging the behaviors in your developer’s environments.
Use multifactor authentication to prevent identity theft.
Add additional security for developers who need to access the production environment from their developer machines.
Track all commits and changes made by developers, for future reference, in case problems arise.

# prerequisites

Developers require secrets like credentials for cloud API keys and other passwords to work with the computers they use daily. However, not every developer understands how to protect and keep these secrets secure.

Suppose you were a developer following insecure code practices. You have a file called insecure.txt that insecurely stores important passwords in plain text.

Let’s download the insecure secrets file needed for this lab.

# Your tasks

1. pen a terminal from the top menu bar with Terminal > New Terminal and make sure that you are in the /home/project folder.

cd /home/project

2. Run the following wget command to acquire the file needed for the lab:


wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0267EN-SkillsNetwork/labs/module4/data/insecure.txt -O ~/insecure.txt

3. Run the following cat command to view the file contents:

cat ~/insecure.txt

As you can see, the file is storing important secrets in plain text. This is certainly not a desirable situation. In the next section, you’ll learn how to securely store these secrets using pass.

# Step 1: Securing the secrets using pass

In this step, we will download and install a Linux-based password manager called pass. We will use pass to generate a secret Gnu Privacy Guard (GPG) key and use it to create a credential store locally on your computer. pass will allow you to store your secrets securely

# Your Task

1. First, install pass in your environment by running the following Linux commands in the command prompt

sudo apt update
sudo apt install -y pass

2. Next, use the gpg command to generate a secret GPG key on your local machine.

gpg --full-generate-key

Follow all of the installation prompts until the installation and configuration process is complete. Press [enter] to accept the default options for the key type, and the other remaining settings. Then press y to confirm.

Note: Remember the password you provided, as this will be your master password.

# Step 2: Initializing Pass

Now that we have generated a GPG key, we can initialize pass with the GPG ID.

This step only needs to be performed once. Then you can save as many secrets as needed afterwards.

# Your task

1. Use the ID of the GPG key you created in Step 1.

gpg --list-secret-keys --keyid-format LONG | grep sec

In the resulting output, you’ll see a line resembling the following:

sec   rsa2048/ABCDEFGH01234567 2019-07-31 [SC]

This is the line that contains your GPG key ID. In this example, the GPG key ID is ABCDEFGH01234567. Copy your key’s ID to the clipboard

2. Use the pass init command to initialize pass with your GPG key ID.

pass init {paste your gpg key here}

* Results

pass init ABCDEFGH01234567
mkdir: created directory '/home/theia/.password-store/'
Password store initialized for ABCDEFGH01234567

# Step 3: Creating secrets

Now you’re ready to create secrets and store them securely. In this step, you’ll use the pass insert command to insert secrets into the secrets manager key store.


# Your task

1. Run the following command to obtain the value for the key IBM_CLOUD_API_KEY from the ~/insecure.txt file:

cat ~/insecure.txt| grep IBM_CLOUD_API_KEY | grep -o "\".*" | grep -o "[a-z,A-Z,0-9]*"

2. Run the following command to create a new secret called IBM_CLOUD_API_KEY in pass:

pass insert IBM_CLOUD_API_KEY

3. Copy and paste the value for IBM_CLOUD_API_KEY you obtained in Step 1.

Let’s create another secret for the password store.

1. Run the command to obtain the value for the key. (This is the same procedure you followed in Step 1 on the previous page). This time, you’ll use PROD_ADMIN_PASS for the new secret. Run the following command in the command prompt and copy the output to the clipboard:

cat ~/insecure.txt| grep PROD_ADMIN_PASS | grep -o "\".*" | grep -o "[a-z,A-Z,0-9]*"

pass insert PROD_ADMIN_PASS

# Step 4: Retrieving secrets

1. Let’s use the show command to display the IBM_CLOUD_API_KEY with pass to verify that the secret was inserted properly:

pass show IBM_CLOUD_API_KEY

You may be asked to enter your master password (GPG key). If so, enter it and continue. Of course, you can perform this same procedure for any password you saved with pass.

2. Let’s show the IBM_CLOUD_API_KEY with pass to verify the secret was inserted properly:

pass show PROD_ADMIN_PASS

pass makes it easy to retrieve stored secrets. Entering your GPG password, when prompted, allows pass to display them for you. If an attacker or others gain access to your computer, they can’t access your passwords or stored secrets since they won’t know what your GPG password is.



