# server_provisions
server provisioning recipe (ansible)

### how to use

```
$vagrant up
$vagrant ssh-config --host 192.168.33.10 >> ~/.ssh/config
$pip install ansible
$ssh-add # git clone private repository by ssh-forward
$ansible-playbook -i hosts site.yml
```
