# Convert a pem file into a rsa private key

When you build a server in AWS one of the last steps is to either acknowledge that you have access to an existing pem file, or to create a new one to use when authenticating to your ec2 server.

If you want to convert that file into an rsa key that you can use in an ssh config file, use the openssl command string.

```
openssl rsa -in somefile.pem -out id_rsa
```

Note: you don’t have to call the output file id_rsa, you will want to make sure that you don’t overwrite an existing id_rsa file.

Copy the id_rsa file to your .ssh directory and make sure to change permissions on the id_rsa key to read only for just your user.

chmod 400 ~/.ssh/id_rsa

---

## Example

openssl rsa -in rancher-test.pem -out id_rsa

STDOUT: writing RSA key


ls
...
id_rsa  
rancher-course.pem



ls -la
...
-rw-rw-r--.  1 jcbrent jcbrent 1679 Jul 14 12:34 id_rsa
-rw-------.  1 jcbrent jcbrent 1698 Jul 13 09:09 rancher-test.pem


chmod 400 id_rsa



ls -la
...
-r--------.  1 jcbrent jcbrent 1679 Jul 14 12:34 id_rsa
-rw-------.  1 jcbrent jcbrent 1698 Jul 13 09:09 rancher-test.pem

---

# Errors

Could not open a connection to your authentication agent.

https://appuals.com/fix-not-open-connection-authentication-agent/

On the other hand, if you get the “Could not open a connection to your authentication agent” error again, the agent needs full reassignment. If you’re working with the regular shell, then just run ssh-agent /bin/sh and then ssh-add ~/.ssh/id_rsa, once again making sure to replace the name of the key. You should have the prompt at this point. Those using pure bash who don’t mind what some in the Linux community refer to as “bashisms” in their ssh client can merely use ssh-agent bash and then use the ssh-add command. Most people will find that both root and regular users have bash in their path and don’t need anything else.


 ssh-agent /bin/sh 
 
ssh-add ~/.ssh/id_rsa

I.e. 
ssh-agent /bin/sh
sh-4.2$ ssh-add ~/.ssh/id-rsa
/home/ec2-user/.ssh/id-rsa: No such file or directory
sh-4.2$ ssh-add ~/.ssh/id_rsa
Identity added: /home/ec2-user/.ssh/id_rsa (/home/ec2-user/.ssh/id_rsa)
sh-4.2$

[ec2-user@ip-172-31-85-223 ~]$ ssh-add -l
The agent has no identities.

#### Worked
    [ec2-user@ip-172-31-85-223 ~]$  eval `ssh-agent`
    Agent pid 4970
    [ec2-user@ip-172-31-85-223 ~]$ ssh-add 
    Identity added: /home/ec2-user/.ssh/id_rsa (/home/ec2-user/.ssh/id_rsa)
    [ec2-user@ip-172-31-85-223 ~]$ ssh-add -l
    2048 SHA256:C14oya08Vw+5+tuauI00wCI/3kWTcPrjRsXiVi6szcY /home/ec2-user/.ssh/id_rsa (RSA)
    [ec2-user@ip-172-31-85-223 ~]$ 

#### Worked
eval "$(ssh-agent)"

ssh-add
  #### alternative

        exec ssh-agent bash
      [ec2-user@ip-172-31-85-223 ~]$ ssh-agent 
      SSH_AUTH_SOCK=/tmp/ssh-1ghdR855Meev/agent.5033; export SSH_AUTH_SOCK;
      SSH_AGENT_PID=5034; export SSH_AGENT_PID;
      echo Agent pid 5034;
      [ec2-user@ip-172-31-85-223 ~]$ ssh-add -l
      The agent has no identities.
      [ec2-user@ip-172-31-85-223 ~]$ ssh-add
      Identity added: /home/ec2-user/.ssh/id_rsa (/home/ec2-user/.ssh/id_rsa)
      [ec2-user@ip-172-31-85-223 ~]$ ssh-add -l
      2048 SHA256:C14oya08Vw+5+tuauI00wCI/3kWTcPrjRsXiVi6szcY /home/ec2-user/.ssh/id_rsa (RSA)







      INFO[0000] Initiating Kubernetes cluster                
      INFO[0000] [dialer] Setup tunnel for host [3.85.233.126] 
      WARN[0129] Failed to set up SSH tunneling for host [3.85.233.126]: Can't retrieve Docker Info: error during connect: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/info: Failed to dial ssh using address [3.85.233.126:22]: dial tcp 3.85.233.126:22: connect: connection timed out 
      WARN[0129] Removing host [3.85.233.126] from node lists 
      WARN[0129] [state] can't fetch legacy cluster state from Kubernetes 
      INFO[0129] [certificates] Generating CA kubernetes certificates 
      INFO[0129] [certificates] Generating Kubernetes API server aggregation layer requestheader client CA certificates 
      INFO[0130] [certificates] Generating Kube Proxy certificates 
      INFO[0130] [certificates] Generating Node certificate   
      INFO[0130] [certificates] Generating admin certificates and kubeconfig 
      INFO[0130] [certificates] Generating Kubernetes API server proxy client certificates 
      INFO[0131] [certificates] Generating Kubernetes API server certificates 
      INFO[0131] [certificates] Generating Service account token key 
      INFO[0131] [certificates] Generating Kube Controller certificates 
      INFO[0131] [certificates] Generating Kube Scheduler certificates 
      INFO[0132] Successfully Deployed state file at [./cluster.rkestate] 
      INFO[0132] Building Kubernetes cluster                  
      FATA[0132] Cluster must have at least one etcd plane host: please specify one or more etcd in cluster config









