# docker-tick-stack

This is the docker file for deploying tick stack on a docker swarm cluster, using an already existing tick-stack.


For using this stack, cd into desired method and follow readme file.




 - Change the desired version of each image.
 - Mounting points:
   - On origin Tick machine: Change the mounting points file paths of each image into disired file paths on one node manager.
   - On new machine: Change mounting point file paths
 - Change node manager name under 
```
constraints: 
  - node.hostname = $MACHINE_HOST_NAME
```


