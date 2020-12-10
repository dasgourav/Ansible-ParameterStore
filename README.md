# Connect to Remote Machine using Systems Manager Parameter Store

The following Ansible Project is part of a Tutorial which contains all the necessary steps to achieve the objective, including how to run the Playbook.

Objective:
The goal here is to do a basic Infra Setup in the AWS with a Pre-configured Ansible Controller Node and a Remote Node. The Ansible Controller will fetch the remote instance, Private Key, from SSM Store Parameter and use it to connect to the Target Node and execute the Ansible Playbook. Don't worry, in the Tutorial, all required steps automated through Cloudformation Template.  Thus, Non-AWS users won't find it difficult & easy to grasp.

How Does it Work:
Ansible controller machine that runs on an EC2 instance should use an IAM role, the role supplies temporary permissions that Ansible controller machine can use when they make calls to AWS SSM Parameter store to retrieve the Secure String without using Password-based authentication mechanism. When we launch an EC2 instance, we specify an IAM role to associate with the instance. Applications that run on the instance can then use the role-supplied temporary credentials to sign API requests.

Quick Point to Discuss: 
The Role SecureStringParameterStore is doing the magic here it is calling the AWS Parameter Store and retrieving the Secrets (Private Key) and storing it temporarily in the Ansible Controller Node. Thus, by providing correct permission to the Ansible Control Machine, it can fetch the Remote Host, login credentials safely from a Cloud Vault during a full Ansible run and store it temporarily until the execution completed.
In the Following Role, (SecureStringParameterStore) , you need to Update the Right Region where you have stored the Cloudformation & SSM Parameter Store, to retrieve the Secure String from the SSM Parameter Store. Here we have used us-east-1 which denotes N.Virginia Region. (Click here to check the AWS Region Code).

Scope for improvement :
1) We can directly store Server Login Credentials in the Systems Manager Parameter Store and retrieve the Credentials only during a Full Ansible run. Similarly, we did in the Role SecureStringParameterStore, without exposing/storing the credentials inside the Ansible Controller Node.
2) For Error-Handling, we can use Ansible rescue blocks (https://docs.ansible.com/ansible/latest/user_guide/playbooks_blocks.html) so that if any Task fails the Retrieved private Key removed safely from the Ansible Controller Node.

