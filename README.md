# OCP-ingress-edit
By default the ingress operator setups the security groups as 0.0.0.0/0 we need to change this to a more controlled network.


To use this you will need oc connected to a cluster, AWS CLI commands access, and Ansible installed on your localhost.


#Command:

ansible-playbook ingress-setup.yaml



#What is does:

This will pull the current config template from the cluster and save it local to the playbook as ingress.yaml.

Next it will construct the "loadBalancerSourceRanges" value using the provided IPs as well as querying aws for the NatGateway's IP address. this construction is saved as registered value.

Then the script will update the ingress.yaml file locally and add the "loadBalancerSourceRanges" values using the "blockinfile" command from Ansible.

Lastly the modification is applied to the cluster using the oc apply -f ingress.yaml command


Now when the cluster reboots it will not reset the Security Groups of the Load balancers to 0.0.0.0/0 but t the defined values we just set.


