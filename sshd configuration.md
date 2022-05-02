
**SSHD CONFIGURATION FOR CENTOS 7**

OpenSSH : Password Authentication

 First we take remote server via ssh using below command
 ```bash
 ssh -l remoteservername ip address
 ```
 
 ![image](https://user-images.githubusercontent.com/98270930/166197608-0c13c5e5-e2c8-40bb-8e5f-163a10108600.png)

 ![image](https://user-images.githubusercontent.com/98270930/166194186-ac507332-fb59-4a99-89b1-4bd49ea93723.png)


# Prohibit root login
 
 vim /etc/ssh/sshd_config

 line 38 uncomment and change 

 PermitRootLogin no

 ![image](https://user-images.githubusercontent.com/98270930/166194889-07646ee5-5969-4a0c-a4fe-754beb26f312.png)
 
# Enable user limit

 vim /etc/ssh/sshd_config
 
 add the below parameter in sshd_config file
 
 AllowUsers username
 
 save and exit
 
 ![image](https://user-images.githubusercontent.com/98270930/166196937-89742a93-2444-4770-a346-e41dedb10608.png)

 
![image](https://user-images.githubusercontent.com/98270930/166196753-c944ba88-bb83-4613-ab5b-fec2663cd099.png)

# Enable group limit

 vim /etc/ssh/sshd_config

 add the userslist group in sshd_config file
 
  ![image](https://user-images.githubusercontent.com/98270930/166199157-75fe11ae-7edb-466b-9ed3-d98d1715b29c.png)
  
  
# OpenSSH : SSH File Transfer (CentOS Client)

  using scp command
  
  file transfer between local host to remote (cent) host
  
  ![image](https://user-images.githubusercontent.com/98270930/166200848-1bad9478-2e88-4d7c-b830-3241bed9aab6.png)
    
  file transfer between remote to local
  
  ![image](https://user-images.githubusercontent.com/98270930/166201607-13570f04-580a-4603-bba6-2d434be09218.png)

  
  Using sftp command
  
  ![image](https://user-images.githubusercontent.com/98270930/166209075-93383c9b-9b26-4915-8d28-8c65df733271.png)


# OpenSSH : SSH Key-Pair Authentication

  first create the ssh keys then copy the  public key into your remote host
  
  ssh-keygen

![image](https://user-images.githubusercontent.com/98270930/166215747-924e5ab8-0315-4680-91bc-acae1458c261.png)

![image](https://user-images.githubusercontent.com/98270930/166214743-8b439073-2b74-4e85-802e-a10a09ee1065.png)
  
 ![image](https://user-images.githubusercontent.com/98270930/166216378-d0a0db4e-2fb8-40ce-8b66-d1d9a25b9b07.png)

    
