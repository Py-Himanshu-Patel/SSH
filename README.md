# SSH
Secure Shell

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
