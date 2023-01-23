# ghost-deploy-ec2
Ansible playbook for deployment of ghost blog platform on EC2 or any other VM. It deploys docke containers on any VM host. Template for running EC2 instance is kidly provided by Andrian Cantril. Checkout his [aws-cfn-snippets](https://github.com/acantril/aws-cfn-snippets) repository on GitHub.


### Create ssh key
1. Connect to AWS web console
2. Open EC2 service, go to -> Network/Security -> Key Pairs. Select .pem format for Mac/Linux computer. Use name ghost-admin and download to local machine
3. Set permissions:
```bash
mv Downloads/ghost-admin.pem .ssh/
chmod 0600 .ssh/ghost-admin.pem
```

### Deploy instance using CloudFormation
1. Open CloudFormation service, go to Stacks -> Create stack
2. Specify template -> Upload a template file - > Choose file `ec2instance.yml`
3. Provide ssh key `ghost-admin.pem` set name for the stack `ghost-ec2-instance` and follow the creation procedure
4. Enable CloudFormation creating an IAM resources
5. When node will be created, check stack ghost-ec2-instance Outputs for the public IP addres and DNS name
6. Test ssh connection:

```bash
ssh -i .ssh/ghost-admin.pem ubuntu@3.93.76.128
```

### Configure Ansible

1. Change directory:
```bash
cd ansible
```
2. Add server IP and change variables to hosts inventory file `ansible/hosts`
3. Test host connectivity:
```bash
ansible all -i hosts -m ping
```
It should be no error at this point, otherwise go back and check avaliability of the instance!

4. Install requirements for ansible playbook
```bash
ansible-galaxy install -g -r requirements.yml --force
```

### Deploy server configuration with Ansible
```bash
ansible-playbook -i hosts playbook.yml
```
### Next steps
1. Add Docker with Nginix and Lets Encryps for TLS encryption
2. Use autogenerated random passwords for internal components
3. Better backup strategy