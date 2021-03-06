# How to deploy mongodb replica set
*This is for real-time synchronization from primary to all secondary mongodb members*
1. In **mongo_replic.yml**. Change port exposure  to desired(if any). Change db directory(if needed).
2. Run `docker-compose -f mongo_replic.yml up -d` on the primary machine.
3. Rename the container and adjust ports for secondary machines. Run `docker-compose -f mongo_replic.yml up -d` on the secondary machines. 
Run it for multiple times if multiple nodes are needed.
4. In **replSet.sh**. Replace the`<Primary_Address>:<Primary_Port>` with primary machine's public IP address or logical DNS hostname. Replace `<Secondary_Address>:<Secondary_Port>`with all secondary machines' info respectively.
5. Run `replSet.sh` **ON AND ONLY ON** primary machine to initiate replica set

## Test
1. Run `mongo mongodb://<Primary_Address>:<Primary_Port>,<Secondary_Address>:<Secondary_Port>/?replicaSet=rs0`to access the replica  set service
2. Run `rs.status()` and make sure the primary node has `"stateStr" : "PRIMARY"`.

## Force a member to become primary (optional)
1. Run `rs.freeze(300)` in mongo shell of all members that stay secondary
2. Run `rs.stepDown(120)` in mongo shell of current primary member
3. The only member left will be elected as primary
