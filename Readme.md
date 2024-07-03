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

sudo systemctl list-units --type=service --state=running

sample repo
https://github.com/devslm/ansible-mongodb/tree/master

setup for high availiability

https://severalnines.com/blog/how-to-deploy-percona-server-mongodb-for-high-availability/

For replication - need to initiate cluster before adding users?

rs.initiate( {

       _id: "my-mongodb-rs",

       members: [

       { _id: 0, host: "172.31.22.123:27017" },

       { _id: 1, host: "172.31.21.159:27017" },

       { _id: 2, host: "172.31.16.217:27017" }

       ] })

rs.status()

user data
echo "node_type=master" >> /etc/environment

echo "node_type=replica" >> /etc/environment


aws ec2 describe-tags \
--region "$(ec2-metadata -z | cut -d' ' -f2 | sed 's/.$//')" \
--filters "Name=resource-id,Values=$(ec2-metadata --instance-id | cut -d " " -f 2)" \
--query 'Tags[?Key==`node_type`].Value' \
--output text
       

mongo --eval "rs.status()" localhost:27017

mongo --eval \
'rs.initiate({_id: "my-mongodb-rs", 
       members:[{_id: 0,host: "172.31.22.123:27017"},
       {_id:1,host:"172.31.21.159:27017"},
       {_id:2,host:"172.31.16.217:27017" }
       ]})' localhost:27017