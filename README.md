This repository helps lunch a wordpress application via an AWS Cloudformation template in two 
ways.  Either running an Ansible Script or loading the template inside to create stack.

The stack creates a VPC with one public subnet and two private subnets. The Public subnet has a WebServer that allows traffic to HTTP 80 only. The Private subnets have a RDSMySQL Database. There are two Security Groups: One for the WebServer and the other for the Database. One internet Gateway. One Route Table and one for internet Gateway attached to the Public Subnet

1.	Via an Ansible Playbook run, in that case the stack will be created automatically with the default values. 
To do that, assuming that you have ansible installed in your playground machine with python, boto3 and git installed. You have to edit /etc/ansible/hosts file to add the localhost
       [local]
       localhost

.You have to add the credentials of the user who will provision the resources. That user should have the permissions to provision..VPC..EC2 instance…RDS database…, if not the Stack will fail. The root user should think of the principle of least privilege and create the access key and secret access key..so create a aws directore…create a file named credentials and add access key and secret access key in that file

[armand_gcp@hitashi]$mkdir ~/.aws/
[armand-gcp@hitashi]$cd ~/.aws/
[armand-gcp@hitashi]$vi credentials
[default]
       aws_access_key_id=the_user_access_key
aws_secret_access_key=the_user_secret_access_key


Now clone the repo
[armand_gcp@hitashi]$git clone https://github.com/armandk/hitashi-demo-ansible.git

Then
[armand_gcp@hitashi]$ cd hitashi-demo-ansible

And run the following to provision the stack
       [armand_gcp@hitashi]$ ansible-playbook -i /etc/ansible/hosts hitashi-domo-
        stack.yml --verbose
 
You can access the wordpress application from this URL  http://3.228.5.18/wordpress/wp-admin/install.php 


2.	Note: Very important…The user assign to me for the exercise doesn’t have permissions to run the template via Ansible nor to load it from a file to create the stack, after log into the AWS Console….The second option will ask the user to add input parameters.

You can see that I was not able to create a S3 bucket to upload the template file
 


         Here I am not able to Create a Stack (Screen below)
 



3.	To run the stack  Login to the AWS Console and go to Cloudformation services
Click create Stack –
Under ‘Specify template’  copy this URL , under ‘Amazon S3 URL’ https://open2theworld.s3.amazonaws.com/hitashi-demo-cloudformation.template
                And follow the rest until Stack completion..You will add parametes on you own and can see the 
                 application at http://{webserverIP}/wordpress/wp-admin/install.php

 

