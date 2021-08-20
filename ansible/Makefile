AP := ansible-playbook
mode := mode=apply
env := env=aws

provision:
	$(AP) -e "$(mode) cloud_type=aws $(env)" provcluster.yml

initcluster :
    $(AP) -i run/runhosts -e "$(mode) env_type=flanneld $(env)" $@.yml