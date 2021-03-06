Getting started
===============

The easiest way to run the deployement is to use the **kargo-cli** tool.
A complete documentation can be found in its [github repository](https://github.com/kubespray/kargo-cli).

Here is a simple example on AWS:

* Create instances and generate the inventory

```
kargo aws --instances 3
```

* Run the deployment

```
kargo deploy --aws -u centos -n calico
```

Building your own inventory
---------------------------

Ansible inventory can be stored in 3 formats: YAML, JSON, or inifile. There is
an example inventory located
[here](https://github.com/kubernetes-incubator/kargo/blob/master/inventory/inventory.example).

You can use an
[inventory generator](https://github.com/kubernetes-incubator/kargo/blob/master/contrib/inventory_builder/inventory.py)
to create or modify an Ansible inventory. Currently, it is limited in
functionality and is only use for making a basic Kargo cluster, but it does
support creating large clusters. It now supports
separated ETCD and Kubernetes master roles from node role if the size exceeds a
certain threshold. Run inventory.py help for more information.

Example inventory generator usage:

```
cp -r inventory my_inventory
declare -a IPS=(10.10.1.3 10.10.1.4 10.10.1.5)
CONFIG_FILE=my_inventory/inventory.cfg python3 contrib/inventory_builder/inventory.py ${IPS}
```

Starting custom deployment
--------------------------

Once you have an inventory, you may want to customize deployment data vars
and start the deployment:

```
# Edit my_inventory/groups_vars/*.yaml to override data vars
ansible-playbook -i my_inventory/inventory.cfg cluster.yaml -b -v \
  --private-key=~/.ssh/private_key
```

See more details in the [ansible guide](ansible.md).
