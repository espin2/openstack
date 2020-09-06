**environment: <br>**
-ubuntu 18.04 <br>

-controller-1 RAM 4GB, vCPU 4<br>
ens3:10.40.180.10(management)<br>
ens8:not configured(external)<br>

install kolla-ansible

```
sudo apt-get update
sudo apt-get install python3-dev libffi-dev gcc libssl-dev
sudo apt-get install python3-pip
sudo apt-get install python-pip
sudo pip install -U pip
sudo pip3 install -U pip
sudo pip install -U 'ansible<2.10'
sudo pip install kolla-ansible
sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla
cp -r /usr/local/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
cp /usr/local/share/kolla-ansible/ansible/inventory/* .
```

```
vi /etc/hosts
127.0.0.1 localhost
10.40.180.10 controller1
```


test inventory reachable<br>
`ansible -i all-in-one all -m ping`

generate password untuk environment openstack<br>
```
kolla-genpwd

```
konfigurasi kolla-ansible

```
vi /etc/kolla/globals.yml

kolla_base_distro: "ubuntu"
kolla_install_type: "source"
openstack_release: "ussuri"
kolla_internal_vip_address: "10.40.180.250"
network_interface: "ens3"
#neutron_external_interface: "ens3"
```
deploy openstack
```
kolla-ansible -i ./all-in-one bootstrap-servers
kolla-ansible -i ./all-in-one prechecks
kolla-ansible -i ./all-in-one deploy
```

###after installation###<br>
install openstack-cli<br>
`pip install python-openstackclient`

generate password<br>
`kolla-ansible post-deploy`<br>
check your admin password<br>
`cat /etc/kolla/admin-openrc.sh`

show all container openstack environment<br>
`docker ps`

check dashboard openstack
http://ip_controller/horizon
