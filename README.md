# SSH - Secure Shell

## Generate SSH Key on Client side
- Install Open SSH to generate RSA pub and pri key pair  
  ```
  sudo apt install openssh-client
  ssh -V
  ssh-keygen
  ```
- Generate Key Pairs  
    ```
    ssh-keygen -t rsa
    ```
- Hit Enter for all and go to dir  
    ```
    ~/.ssh
    ```
- Find below files
    ```bash
    id_rsa  # contains the private key.
    id_rsa.pub # contains the public key.
    ```
- See Private Key
    ```
    ----BEGIN OPENSSH PRIVATE KEY-----
    aRPhZP5/9VB7rMW8PRNoz5CjS4T9eAr1/ZBQQLGwFQavSfYVJpchUtoE6WDaM8jVMF0+qc
    VAGEBT0FCBVoHUPD26/fedMBjcOTaGHAAAAHmhpbWFuc2h1LnBhdGVsQGNsYXJpc2lnaHRzLmNvbQECAwQ=
    -----END OPENSSH PRIVATE KEY-----
    ```
- See Public Key
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAqDdXc42vWhdKZlV4SKFBnEA5McU6cFsvW1k9v0Cx/Ur+0ewTamJoHXfsCAOrEZSMUOkxuwTZsQbQw== himanshu.patel@clarisights.com
    ```
- See `known_host` file. this contains the host to which we connect, any new host will be added to this file and if any existing host facing some problem while connecting then we can remove it from this file first.
    ```
    |1|BHIFZI4UN2Oofu23naAQKttFZ3c=|R8cSNIDAP+w0nv1I2MSps39G+6Y= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
    |1|MbAPRd/yF/dG6nkD/ywM9xNaf3s=|o/prZJ/dAFCp5mjdOYHRRljAnC0= ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
    ```
- Update the `config` file to give an alias
    ```
    ssh root@10.193.32.55
    ```
    This can be updated to 
    ```
    ssh dev
    ```
    If we update the `config` file as
    ```
    $ cat config 
    Host dev
		HostName 10.193.32.55
		User 	root
		ForwardAgent yes    
    ```
    This will only work if we have `PermitRootLogin` and `PasswordAuthentication` as `yes` in `/etc/ssh/sshd_config` file in server we are trying to SSH into.
- In case we are connecting to a BitBucket or similar third party tool, then provide them with our public key.
    ```bash
    ssh -T git@bitbucket.org
    # if this works then SSH is successful
    ```
    Also update the `config` file as below
    ```
    Host bitbucket.org
        AddKeysToAgent yes
        IdentityFile ~/.ssh/{ssh-key-name}
    ```

## Secure Remote Command Execution
```bash
$ ssh root@10.193.32.55 'echo $HOSTNAME'
# OR
$ ssh dev 'echo $HOSTNAME'

clarisights-dev
```

## Secure File Transfer
```bash
$ scp dump.js dev:/root/dev/
# OR
$ scp dump.js root@10.193.32.55:/root/dev/

dump.js                 100%   66   151.3KB/s   00:00    
```
When transmitted by scp, the file is automatically encrypted as it leaves client machine and decrypted as it arrives on server machine.

## SSH Agent
1. In advance (and only once), place special files called public key files into your
remote computer accounts. These enable your SSH clients (ssh, scp) to access
your remote accounts.
2. On your local machine, invoke the ssh-agent program, which runs in the
background.
3. Choose the key (or keys) you will need during your login session.
4. Load the keys into the agent with the ssh-add program. This requires knowl-
edge of each key’s secret passphrase.
5. You have an ssh-agent program running on your local machine, holding your secret keys in memory. You’re now done. You have password-less access to all your remote accounts that contain your public key files.

## Port Forwarding
SSH can increase the security of other TCP/IP-based applications such as telnet, ftp, and the X Window System. A technique called `port forwarding` or `tunneling` reroutes a TCP/IP connection to pass through an SSH connection, transparently encrypting it end-to-end. Port forwarding can also pass such applications through network firewalls that otherwise prevent their use.
```
$ ssh -L 3002:localhost:119 news.yoyodyne.com
```
This says “ssh, please establish a secure connection from TCP port 3002 on my local machine to TCP port 119, the news port, on news.yoyodyne.com.”

Configure your news-reading program to connect to port 3002 on your local machine. The secure tunnel created by ssh automatically communicates with the news server on news.yoyodyne.com, and the news traffic passing through the tunnel is protected by encryption.

## Firewalls
A firewall is a hardware device or software program that prevents certain data from entering or exiting a network. For example, a firewall placed between a web site and the Internet might permit only HTTP and HTTPS traffic to reach the site. As another example, a firewall can reject all TCP/IP packets unless they originate from a designated set of network addresses.