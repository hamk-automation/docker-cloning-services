# node-red under docker
To build a node-red docker image on an existing node-red:
   - Clone and cd into this repo.
   - Copy flows.json and package.json from existing node-red into this directory
   - To build file, run command:
   ```
   docker build -t node-red .
   ```
   - To run this image in detach mode:
   ```
   docker run -it -d -p 1880:1880 --name=mynodered node-red
   ```
Node-red is now available on port 1880. To change desired port simply change -p tag:
```
-p 8000:1880
```
