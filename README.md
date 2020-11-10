# ansible-gitops-demo

This demo uses Ansible Tower's ability to provide a webhook interface to a job template, or workflow template, and integrates provisioning, build, and deploy job templates in a workflow triggered by a Github push event.  Please don't use this in production.  This demo uses a simple NodeJS web application as an example, but the playbooks could be modified to accommodate many build/deploy processes.  

## Requirements
Ansible Tower 3.7.x
AWS account with permissions to create ec2 instances
AWS Access and Secret keys for account
An existing security group in AWS with port 22 and 80 open

## Steps to setup demo (*Going to work on automating this setup later)
1. Create a project with this git repo in Ansible Tower
2. Create the provision/deprovision Job Templates using the Tags
  - passing 'provision' in the tags field of the job template will provision the instance
  - passing 'deprovision' in the tags field of the job template will terminate the instance
3. Create the build and deploy job templates from the playbooks
4. Create a Workflow Template, with the following order:
  - deprovision
  - provision
  - build
  - deploy
Note: After the build and deploy steps you can add an 'on failure' branch to the workflow and deprovision the server if you want.
5. On the workflow template, enable the webhook and select the GitHub type.  
6. On the simple-web-app git repo (fork it so you can make changes to it yourself) setup a webhook according to githubs instructions and copy and paste the webhook URL for your Tower Webhook.  Be prepared, it will kick of the webhook instantly as a test, make sure in the response you get a `{"message": "Job queued."}` response and check in Tower to make sure the workflow is running.
