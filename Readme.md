- # getting start
- ## Prepare Enviorment
	-
	  ```shell
	  git clone -b master git@gitlab.com:ataba1/ansible/mikrotik-linux-ansible.git
	  cd mikrotik-linux-ansible/
	  python3 -m pip install virtualenv
	  virtualenv env
	  source env/bin/activate
	  pip install -r ./requirements.txt
	  ansible-galaxy collection install -r requirements.yml 
	  source env/bin/activate
	  #make sure your are on the right place
	  which ansible
	  ```
- ## add your target hosts
	- editable variable/changes
		- change/add host in `mik.hosts.ini` with `ansible_host` and `ansible_ip` see example 
		  > remember the name should be similar in `host_var/` like example  
		- make sure to add your hosts in `host_var/` by clone  `example.yml` and the name same as
		-
		  ```
		  cp host_vars/example.yml host_vars/NewHost.yml
		  nano host_vars/khv1srv.yml
		  ```
		- change the **required**
		  | Variable | Description | Belong to | Location | Change |
		  |---|---|---|---|---|
		  | hosts | set target host or group | ansible | open internet-playbook.yml | required |
		  | encoding | set supported encode | mikrotik | open internet-playbook.yml | optional |
		  | netchain | .. | mikrotik | open internet-playbook.yml | optional |
		  | nataction | .. | mikrotik | open internet-playbook.yml | optional |
		  | interfacelist | .. | mikrotik | open internet-playbook.yml | optional |
		  | natstatus | .. | mikrotik | open internet-playbook.yml | optional |
		  | hostname | ip or domain for mikrotik | mikrotik | open internet-playbook.yml | required |
		  | username | user of mikrotik | mikrotik | open internet-playbook.yml | required |
		  | password | password of mikrotik | mikrotik | open internet-playbook.yml | required |
		  | montip | target host ip | target host | host_vars Folder | required |
		  | montport | ports of that host | target host | host_vars Folder | required |
		  | montproto | using TCP or UDP.. | target host | host_vars Folder | required |
		  | monthost | the host domain | target host | host_vars Folder | required |
		  | user | user to access SSH of target host | target host | host_vars Folder | required |
		  | hostslist | extra domains to open on nat | ansible | host_vars Folder | optional |
		  | inventory | select location of it | ansible | ansible.cfg | optional |
		  | user | default user for target hosts for ssh | ansible | ansible.cfg | optional |
		  | [monttest] | group that contain all hosts | ansible | mik.hosts.ini | optional |
		  | ansible_ssh_private_key_file | for SSH key [more](https://docs.ansible.com/ansible/latest/inventory_guide/connection_details.html#setting-up-ssh-keys) | ansible | mik.hosts.ini | optional |
- ## add auth of mikrotik
	- ### not encrypted
		- make sure to change the variables in `open internet-playbook.yml`
			- password
			- username
	- ### encrypted
		- encrypt password by
		-
		  ```
		  ansible-vault encrypt_string --name 'mikrotikpassword'
		  ```
		-
		- put the ansible password for encrypt/decrypt
		- then put the password of router
		- then ctrl+d twice for no new line else ctrl+d once
		- then generated encrypted text should be near this:
		  ```
		  !vault |
		  $ANSIBLE_VAULT;1.1;AES256
		  61633830313336373366636334653635373332366237653139613333363534306239353436313931
		  3835633632626434663437316563633165363032663331650a356135663631313534306437306566
		  34383165353736653938623434656265356438666461663331373433386434346437646463643464
		  3533316238626163650a373666623562303735363432643862303639653732653635333165383334
		  3331
		  ```
		- then copy it and add it to variable for now is `password:` in `open internet-playbook.yml`
- ## add SSH key to host
	- make sure to add your public key from `~/.ssh/id_ed25519.pub` or `~/.ssh/id_rsa.pub` to the **target host** in `.ssh/authorized_keys`
- ## run ansible playbook
	- if you use playbook manually then to run playbook use:
	  ```
	  ansible-playbook open\ internet-playbook.yml --private-key ~/.ssh/id_ed25519 --ask-vault-pass
	  ```
	- use automaticlly by save it to script or file and give it right permission then:
		- like secrets.txt put inside it the ansible decrypt password then
		-
		  ```
		  ansible-playbook open\ internet-playbook.yml --private-key ~/.ssh/id_ed25519 --vault-id mikrotikpassword@secrets.txt
		  ```
- ## Other purpose 
    - if everything going well and want to add new personal purpose like install something etc
    make sure to add your ansible code at `## action` with/without Pause it's up to you
- ## possible future
    - add dict support for allowlist website and it's own port
    - make more improvement like dealing it as a template for other purpose update/upgrade/install new packages