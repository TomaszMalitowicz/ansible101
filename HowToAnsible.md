Ansible Training:
dnsmasq
192.168.100.6

ifconfig -a | grep inet

ansible istalacja na ubuntu

sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
ansible --version


po ponownym odpaleniu maszyny
cd ansible/
source venv27/bin/activate
touch ansible.cfg
[defaults]
inventory = hosts
host_key_checking = False

touch hosts
[all]
centos1




ansible all -m ping

ssh-keygen


ssh-copy-id centos1

ansible all -m ping

ansible all -i centos1, -m ping
ansible all -m debug --args='msg="This is the custom debug message"'
ansible all -m debug --args='msg="This is the custom debug message" verbosity=3'
ansible all -vvv -m debug --args='msg="This is the custom debug message" verbosity=3'


dodaje nowe maszyny do pliku hosts

test pingu

ansible '*' -m ping

dodajemy klucze do każdej maszyny: 
for host in ubuntu1 ubuntu2 ubuntu3 centos2 centos3; do ssh-copy-id ${host}; done

ansible all -m ping -o

ansible all --list-hosts
ansible ~.*3 --list-hosts

#lista dostepnych modułów
ansible-doc --list 

ansible all -m file -a 'path=/tmp/test state=touch'
ansible all -m file -a 'path=/tmp/test state=file mode=600'
ansible all -m copy -a 'src=/tmp/x dest=/tmp/x'
ansible all -m copy -a 'remote_src=yes src=/tmp/x dest=/tmp/y'


ansible all -m command -a 'hostname' -o



ansible all -a 'touch /tmp/test_copy_module creates=/tmp/test_copy_module'


ansible all -a 'rm /tmp/test_copy_module removes=/tmp/test_copy_module'

ansible all -m file -a'path=/tmp/test_copy_module state=absent'

ansible centos1 -m file -a 'path=/tmp/test_modules.txt state=touch mode=600' 
ansible centos1 -a 'ls -altr /tmp/test_modules.txt'
ansible centos1 -m fetch -a 'src=/tmp/test_modules.txt dest=/tmp/test_modules.txt'



ansible-playbook motd_playbook.yml

ansible centos1 -m setup -a 'filter=ansible_memfree_mb'

ansible$ ansible centos3 -m setup | grep ansible_distribution
ansible$ ansible all -a 'hostname -s' -o

ansible all -m setup -a filter='ansible_distribution*'

hash

ansible centos -m user -a "name=cwaniaczek state=absent remove=yes"
ssh centos2 -l root ls -alrt /home/sara
ssh centos3 -l james ls -altr /home/james/



ansible vault:

ansible-vault encrypt_string --ask-vault-pass --name 'ansible_become_pass' 'password'
New Vault password:
Confirm New Vault password:
ansible_become_pass: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31643665376561633036343265366237356336613934313337626564356533633830303665663930
          3830643962303239393830303532333533306262303965390a303536393066306261366362336133
          32626232623839613232386562653037626566353933666261333438653235363563663835343537
          3430653536363466310a303431383866386139363736363637383165653334336264333664303737
          6336
Encryption successful

ansible --ask-vault-pass all -m ping -o

ansible-vault  encrypt external_vault_vars.yml
ansible-playbook --ask-vault-pass vault_playbook.yml
ansible-vault decrypt external_vault_vars.yml


ansible-vault --vault-id @password_file view external_vault_vars.yml



ansible-vault encrypt --vault-id vars@prompt external_vault_vars.yml
ansible-vault encrypt_string --vault-id ssh@prompt --name 'ansible_become_pass' 'password'
ansible-playbook --vault-id vars@prompt --vault-id ssh@prompt vault_playbook.yml
ansible-vault encrypt --vault-id playbook@prompt vault_playbook.yml


ansible-playbook --vault-id vars@prompt --vault-id ssh@prompt --vault-id playbook@prompt vault_playbook.yml
Vault password (vars):
Vault password (ssh):
Vault password (playbook):

ansible-playbook nginx_playbook.yml --tags 'install-epel'
ansible-playbook nginx_playbook.yml --tags 'install-nginx'
ansible-playbook nginx_playbook.yml --tags 'patch-nginx'
ansible-playbook nginx_playbook.yml --tags 'nginx-template,nginx-running,nginx-open-firewall'

ansible-playbook nginx_playbook.yml --skip-tags 'patch-nginx'


ansible-galaxy init Nginx


New AWS_ACCESS_KEY_ID='AKIATHU3XMIYEEVVYBVH'
New AWS_SECRET_ACCESS_KEY='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'


export AWS_ACCESS_KEY_ID='AKIATHU3XMIYEEVVYBVH'
export AWS_SECRET_ACCESS_KEY='5L8iLDClST7V4Je8iMx1lRbYItJbYNDswIvAoqeq'

nowe ubuntu 10.0.2.15./24
your name: Tomasz
server name: ubuntu-ansible-master
username: ansible-t
pass: 458845tm


export aws_access_key_id=AKIATHU3XMIYEEVVYBVH
export aws_secret_access_key=5L8iLDClST7V4Je8iMx1lRbYItJbYNDswIvAoqeq
aws_access_key_id
aws_secret_access_key
"aws_access_key": null,
"aws_secret_key": null,

mkdir -p group_vars/all/
touch playbook.yml
ansible-vault create group_vars/all/pass.yml
New Vault password:
Confirm New Vault password:
ec2_access_key: AKIATHU3XMIYEEVVYBVH
ec2_secret_key: 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

do playbooka w każdej sekcji np ec2 czy s3 dodaj:           
aws_access_key: "{{ec2_access_key}}"
aws_secret_key: "{{ec2_secret_key}}"
ansible-playbook playbook.yml --ask-vault-pass


