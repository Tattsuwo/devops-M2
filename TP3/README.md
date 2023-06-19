Se connecter au serveur distant pour utiliser son terminal :
ssh -i ~/.ssh/id_rsa centos@chhoathierry.takima.cloud


mettre le nom de domaine pour que Ansible le trouve :
sudo nano /etc/ansible/hosts


envoyer ping au serveur distant :
ansible all -m ping --private-key=~/.ssh/id_rsa -u centos


mettre Apace sur le serveur distant :
ansible all -m yum -a "name=httpd state=present" --private-key=~/.ssh/id_rsa -u centos --become


creation d'une page html sur le serveur distant :
ansible all -m shell -a 'echo "<html><h1>Hello World</h1></html>" >> /var/www/html/index.html' --private-key=~/.ssh/id_rsa -u centos --become


lancer l'Apache service :
ansible all -m service -a "name=httpd state=started" --private-key=~/.ssh/id_rsa -u centos --become

aller sur le site web pour constater l'installation : 
chhoathierry.takima.cloud

-----------------------------------------------------------------------------------------

ansible all -i ansible/inventories/setup.yml -m ping

Facts are prefixed by ansible_ and represent information derived from speaking with your remote systems.


avoir l'OS distribution du serveur distant : 
ansible all -i ansible/inventories/setup.yml -m setup -a "filter=ansible_distribution*"


retirer Apache sur le serveur distant :
ansible all -i ansible/inventories/setup.yml -m yum -a "name=httpd state=absent" --become


pour executer le playbook :
ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml

remplacer dnf par yum car on a installé dans le playbook.yml


----------------------------

création des differents roles : 
ansible-galaxy init roles/launch-proxy

modidier le task pour chacun des roles

et creer un playbook qui initialise tout les roles .
