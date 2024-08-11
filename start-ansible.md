# getting start
## prepare enviorment 
python -m pip install virtualenv
virtualenv env 
source env/bin/activate
pip install -r ./requirements.yml

## add your hosts
make sure to add your hosts in `/host_var` 
change `ansible.cfg` to select `.hosts.ini` in `inventory      = mik.hosts.ini`
 or make changes in `mik.hosts.ini`

## add auth of mikrotiks 
### first 
encrypt password by
```
ansible-vault encrypt_string <password_source> '<string_to_encrypt>' --name '<string_name_of_variable>'
```
put the password in `open internet-playbook.yml` in `password:`