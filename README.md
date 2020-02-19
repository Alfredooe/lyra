## Lyra - rapidly and painlessly deploy Minecraft servers from Discord

Lyra lets you deploy Minecraft server instances via Docker with control through a Discord bot. It has the functionality implemented to let users pay only for what they use by uploading/downloading servers to and from a private Backblaze B2 bucket when stopped and started, taking the storage load off the server. MySQL database to store user data not included.

![Screenshot](https://i.ibb.co/p3Xgs6W/image.png)

## Licensing

Lyra uses the GPL 3.0 license. This means that any components that use Lyra source code must use the same open-source GPL 3.0 license themselves. If you would like to use the Lyra source code for commercial use, please email **mail at tiebe.co** with more information.

## Contributors
Lyra was developed by [cosmopath](https://github.com/cosmopath) and [Alfredooe](https://github.com/Alfredooe) with contributions from [DashLt](https://github.com/DashLt) and [Nicholas Rosati](https://github.com/hydranoid620).

---

## Structure

The front-end of Lyra, where the user and the Discord bot communicate is located in **script.py**. The back-end, which pulls data from the database and server to return it to the front-end is located in **server.py**.
        
## How to extend the backend 

- Define a new function in  the class DockerCommandServer. (tip: if this deals with referencing (not creating) a container by name, put @gets_container on the line before; now, the variable container will always lead to a Container object
- Specify any necessary arguments (and make sure when implementing the frontend to pass these!)
- Do whatever you need to do, and return a tuple (this, thing). The first value is either "success" or "failure", and the last value is any return info you want to pass to the frontend that can be stuffed in a JSON object. If you're not returning anything, write None.
- In the conection_made function, under self,commands, add a new dictionary entry. The key should be the command you'd call the function with from the frontend, and the value should be the function you want to call. (No parenthesis!)
- You're done!

Tips:
start, stop, and delete_container() are all easy examples of how to write a function.
To test, run sudo -u dockercontroller python3 server.py in a screen or another terminal. Then, run sudo -u discord nc /tmp/docker.socket, and send over a JSON formatted message like {"command": "start", "args": {"container": "8675309"}}
