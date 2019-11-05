# Lyra

Deploying Minecraft server instances via Docker, with control through a Discord bot. Lets users pay only for what they use by uploading/downloading servers to and from a private Backblaze B2 bucket when stopped and started. MySQL database to store user data not included.

# A word abount the license

Lyra uses the GPL 3.0 license. This means that any components that use Lyra source code must use the same open-source GPL 3.0 license themselves. If you would like to use the Lyra source code for commercial use please email **mail at superstomp.io**

---

# Code snippets

List containers command 

    @bot.command()
    async def containerlist(ctx):
        jsonData = await async_comms('list', {})
        await ctx.send(jsonData['result'])
    
Reading / Writing 

    Defining reader and writer
        reader, writer = await asyncio.open_connection('127.0.0.1', 8888)

    Sending
        json.dumps({'command': command, 'args', {'arg': 'one', 'arg_': 'two'}})

    Receiving
        return = await reader.read(100)
        json.loads(return)
        
# Backend guide 

- Define a new function in  the class DockerCommandServer. (tip: if this deals with referencing (not creating) a container by name, put @gets_container on the line before; now, the variable container will always lead to a Container object
- Specify any necessary arguments (and make sure when implementing the frontend to pass these!)
- Do whatever you need to do, and return a tuple (this, thing). The first value is either "success" or "failure", and the last value is any return info you want to pass to the frontend that can be stuffed in a JSON object. If you're not returning anything, write None.
- In the conection_made function, under self,commands, add a new dictionary entry. The key should be the command you'd call the function with from the frontend, and the value should be the function you want to call. (No parenthesis!)
- You're done!

Tips:
start, stop, and delete_container() are all easy examples of how to write a function.
To test, run sudo -u dockercontroller python3 server.py in a screen or another terminal. Then, run sudo -u discord nc /tmp/docker.socket, and send over a JSON formatted message like {"command": "start", "args": {"container": "8675309"}}
