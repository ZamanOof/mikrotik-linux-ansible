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
ansible-vault encrypt_string --name 'mikrotikpassword'
```
قم بكتابة الرمز ليقوم انسايبل بفتحه من خلاله
ثم كرره
بعدها ضعع رمز الراوتر
وفي حال عدم وجود سطر جديد اضغط مرتين ctrl+d
بعدها قم بنسخ التشغير
ليكون مقارب لهذا
```
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          61633830313336373366636334653635373332366237653139613333363534306239353436313931
          3835633632626434663437316563633165363032663331650a356135663631313534306437306566
          34383165353736653938623434656265356438666461663331373433386434346437646463643464
          3533316238626163650a373666623562303735363432643862303639653732653635333165383334
          3331
```
![alt text](image.png)
put the password in `open internet-playbook.yml` after `password:`
nano secrets.txt
put ansible decrypt password
run
```
ansible-playbook ssh-config.yaml --vault-id mikrotikpassword@secrets.txt

```