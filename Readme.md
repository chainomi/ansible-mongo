ansible-galaxy collection install community.mongodb

ansible-playbook -i inventory-mongodb.ini site.yaml


# confirm os compatibility with percona server
view os info - `lsb_release -a`
codename for debian 12 is bookworm
`ansible_distribution_release` gets the codename for the os running

confim version compatiblity - https://www.percona.com/services/policies/percona-software-support-lifecycle#mongodb
debian 12 only supports PDMDB v7.0

versions for pdmb 7 after repo install - `sudo apt-cache madison percona-server-mongodb`


# permissions for conf file
-rw-r--r-- 1 root root 1471 May 28 13:14 /etc/mongod.conf

# command for connecting to server locally

`mongosh --port 27017  --authenticationDatabase "admin" -u "admin" -p`
`mongosh --port 27017  --authenticationDatabase "test" -u "bob" -p`



sample repo
https://github.com/devslm/ansible-mongodb/tree/master

