---
share: true
---


```
You can have CLIs and IDEs like IntelliJ IDEA connect to a remote docker engine.

For docker CLI, this is easy; just add a remote context as given [here](https://code.visualstudio.com/docs/containers/ssh):
	`docker context create tux --docker "host=ssh://andy@tux"`
(this assumes you have non-interactive login set up, eg using an SSH agent )

Then  run `docker context use tux` or use `-c`  
```shell
andy@macbook ~ % docker -c tux images      

REPOSITORY                                  TAG           IMAGE ID       CREATED         SIZE

bitnami/kafka                               3.4           51c2c11ad668   46 hours ago    557MB

node-docker                                 latest        1d6dd88cbfa3   9 days ago      199MB

```

VS Code also recognises 'Docker Contexts: Use' and shows the containers/images etc in its tool windows.

## IntellIJ IDEA
IntelliJ IDEA Ultimate  also supports SSH contexts [see here](https://www.jetbrains.com/help/idea/settings-docker.html), but the Community Edition does not. You can still do it!

### Option 1: Use SSH port forwarding
1. on the remote host, run `unix:///var/run/docker.sock` and not the endpoint, something like `unix:///var/run/docker.sock` for the default one.
2. Use ssh local port forwarding, e.g.:
```shell 
ssh -vnNTL ~/.remote-docker.sock:/var/run/docker.sock tux.local
docker context create  tuxssh --docker host=unix:///Users/andy/.remote-docker.sock

tuxssh

Successfully created context "tuxssh"
``` 

The unix-domain contexts you have show up here in IntelliJ's [Docker connection settings](https://www.jetbrains.com/help/idea/settings-docker.html):
![[Pasted image 20230529150957.png]]


### Option 2: Enable TCP remote access on the Docker daemon 
To do this, [Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/).Beware this warning!: 
> Secure your connection
> 
> Before configuring Docker to accept connections from remote hosts it’s critically important that you understand the security implications of opening Docker to the network. If steps aren’t taken to secure the connection, it’s possible for remote non-root users to gain root access on the host. For more information on how to use TLS certificates to secure this connection, check [Protect the Docker daemon socket](https://docs.docker.com/engine/security/protect-access/).

### Configuring remote access with systemd unit file[](https://docs.docker.com/config/daemon/remote-access/#configuring-remote-access-with-systemd-unit-file)

1. Use the command `sudo systemctl edit docker.service` to open an override file for `docker.service` in a text editor.
    
2. Add or modify the following lines, substituting your own values. 
```
[Service]

# Add (unauthenticated!) TCP socker as per

# https://docs.docker.com/config/daemon/remote-access/#configuring-remote-access-with-systemd-unit-file

ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375 --containerd=/run/containerd/containerd.sock
```
    
- Save the file.
    
- Reload the `systemctl` configuration.
```
sudo systemctl daemon-reload
```
  
- Restart Docker.
```
sudo systemctl restart docker.service
```
    
- Verify that the change has gone through.
```
 sudo netstat -lntp | grep dockerd
```



#blog #vscode #docker #IntelliJ


