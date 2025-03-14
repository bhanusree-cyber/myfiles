To install Terraform on your EC2 instance, let's focus on resolving the current issue. From the previous steps, the issue seems to be related to the HashiCorp GPG key verification. If you already have Terraform installed locally, and you want to install Terraform on your EC2 instance using the appropriate repository, follow these steps to resolve the key issue and proceed with the installation:

Steps to Install Terraform on EC2:
Remove the current problematic repository (if necessary):


sudo rm /usr/share/keyrings/hashicorp-archive-keyring.gpg
sudo rm /etc/apt/sources.list.d/hashicorp.list


Re-add the correct GPG key:


curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
Add the HashiCorp repository:


echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
Update the apt package list:


sudo apt update
If you encounter the NO_PUBKEY error again, you can use this workaround to fix it.


sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys AA16FCBCA621E701
Install Terraform:


sudo apt install -y terraform
Verify the Terraform installation:


terraform -v


Alternative Method (Direct Installation via Binary)
If the repository installation continues to give issues, you can manually download the Terraform binary from HashiCorp's official website and install it.

Steps to Install Terraform via Binary:
Download the latest Terraform release:


curl -LO https://releases.hashicorp.com/terraform/$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version')/terraform_$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version')_linux_amd64.zip
Install unzip (if not already installed):


sudo apt install unzip
Unzip the downloaded Terraform binary:


unzip terraform_*.zip
Move the Terraform binary to /usr/local/bin/:


sudo mv terraform /usr/local/bin/
Verify the installation:


terraform -v


If you want to upload a file called myfile.txt from your local machine to the home directory of your EC2 instance:<img width="854" alt="image" src="https://github.com/user-attachments/assets/d8908e7a-b2de-43e6-9814-29a1a3848dd4" />



scp -i /home/user/my-aws-key.pem /home/user/myfile.txt ubuntu@54.123.45.67:/home/ubuntu/
Replace /home/user/my-aws-key.pem with the actual path to your private key.
Replace /home/user/myfile.txt with the file you want to upload.
Replace 54.123.45.67 with your EC2 instance's public IP.
