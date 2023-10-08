# Cisco switch Access :

There are three techniques to connect to a switch: telnet, web, or serial port (console connection). The method to use for the initial configuration, troubleshooting, or reconfiguration is the console mode connection.

## **Connection with Serial:**

This is about connecting a serial cable (blue) from the console port of the switch to one
of the "COM" ports on a PC. We will assume that we are connected
to COM1.

Don't hesitate to use one or more line breaks to "wake up" the console.

1. Open a "Terminal Emulator" (Start Menu - Accessories - Communication).
2. Configure it to 8N1 (8 bits, Null, parity bit 1).
3. Choose the corresponding COM port (usually COM1). The connection is direct and rather simple (the default settings should be correct).

<p align="center">
  <img src="https://github.com/YassinMk/Basic-Switch-Config/assets/122708120/5ed338a6-4021-4ba9-8502-f92847b54ce3" alt="imageTelnet">
</p>


    

## 

## Connection with T**elnet, web:**

La connexion par telnet et interface web est aisée une fois que la configuration de base a
été menée à bien, et que vous avez donné un mot de passe au mode privilégié.

## Connection Secure with  SSH **:**

Secure Shell (SSH) is a network protocol used to establish secure and encrypted communication between two devices over a potentially unsecured network. In the context of networking and switches, SSH is often used to securely connect to and configure network switches, routers, and other network devices.

<p align="center">
    <img src="https://github.com/YassinMk/Basic-Switch-Config/assets/122708120/9c800617-7f3f-4548-a7a3-6d68dcb04fac" alt="image">
<p/>

### Concept :

1. **Authentication**: SSH requires proper authentication to establish a connection. Typically, you'll need a username and password to log in. More secure methods, like SSH keys, can also be used for authentication.
2. **Encryption**: SSH encrypts the data exchanged between the client (your computer) and the switch, making it extremely difficult for unauthorized parties to intercept and decipher the information.
3. **Secure Communication**: Once connected, you can securely configure and manage the switch remotely. This is especially important for network administrators who need to access and manage network devices from a distance.
4. **Configuration**: You can use SSH to access the command-line interface (CLI) of the switch. From there, you can configure various settings, such as VLANs, port settings, security policies, and more.
5. **Security Best Practices**: When using SSH with switches, it's essential to follow security best practices. These may include using strong, unique passwords or employing SSH key pairs for authentication, limiting SSH access to trusted IP addresses, and regularly updating switch firmware for security patches.

# The basic configuration of a switch

## I-Le setup:

To configure a switch (initial configuration or complete reconfiguration), you first need to enter privileged mode:

```bash
>enable
Switch#
```

At the very beginning, the privileged mode will not have a password, it's up to you to configure a new one for the future.
—>Then enter the command:

```bash
configure terminal / conf t
```

The system switches to global configuration mode.
—>To change the switch's name, type "hostname" in configure mode.

```bash
# hostname NOM_SWITCH
```

to create a a user in switch you can do it by this  instruction in bash  you have 0-14 levels of authorization for users you can give it  0 (he does not have any access) -15(admin):

```bash
>conf t
#username admin privilege 15 secret 123
#do sh run (show la configuration en coure)
#username netkat privilege password 345
```

The encryption is done through the "encrypt" command for the password. If you are not interested, use "password".

- Privileged password:

The password is by default set to unencrypted mode, and will therefore appear
every time the "show run" command is executed. To fix this, encrypt the password
for the privileged mode:

```bash
>enable
 #conf t 
 # enable secret MOTDEPASSE
```

## II- Configuring Telnet Access:

1. To begin with, you will require a Rollover cable, a patch cable, and a serial cable.
2. Physical connection
3. Network topology 
4. Discuss Different modes (privilege exec mode)
5. Set hostname:
    
    ```bash
    Swictch>en
    #hostname SW-1
    ```
    

6-Set IP address for VLAN :

```bash
#int vlan 1
#add ip address 192.168.1.1 255.255.255.0
#no sh
#show interface brief
```

7-Set Password VTY(virtual …)  and enable password :

```bash
#line vty 0 15
#password blah
#login
#enable secret 0 enablepw
```

8- Set service password-encryption 

```bash
#service password encry
```

9-Save the configuration to enable easy setup in the future.

```bash
#wr
#show start-configuration  
```

### III-Configuration SSH Access :

1. **Connect to the Switch Console**: You will need console access to the switch for initial configuration. This can be done through a physical console cable or through a remote management tool like Telnet or a web-based interface, if available.
2. **Access Configuration Mode**: Log in to the switch's command-line interface (CLI) and enter privileged exec mode. This is typically done with a command like **`enable`** or **`enable password`** and providing the correct password.
3. **Generate SSH Keys**: SSH uses cryptographic keys for secure authentication. If keys are not already generated on the switch, you'll need to create them. Use the following command to generate RSA keys:
    
    ```bash
    switch# crypto key generate rsa
    ```
    
    to more understand i suggest to watch this video  in youtube :
    
   <p align="center">
      <a href="https://youtu.be/ePGJMXj8dqc">
        <img src="https://github.com/YassinMk/Basic-Switch-Config/assets/122708120/e10abfb1-5363-4e1a-ac30-a648ed7906ba" alt="image">
      </a>
    </p>

    
    ## VI-Configure of Vlan in Switch Cisco :
    
    Configuring a Cisco switch typically involves using the Command Line Interface (CLI) to enter commands. Below, I'll provide you with a series of CLI commands to perform the actions mentioned in the video tutorial:
    
    1. Change the hostname of the switch:
    
    ```bash
    shellCopy code
    enable
    configure terminal
    hostname YourNewHostName
    
    ```
    
    1. Set the enable mode password:
    
    ```bash
    shellCopy code
    enable
    configure terminal
    enable secret YourEnablePassword
    
    ```
    
    1. Assign interfaces to VLANs:
    
    ```bash
    shellCopy code
    enable
    configure terminal
    vlan 10
    name Sales
    exit
    vlan 20
    name HR
    exit
    vlan 30
    name WiFi
    exit
    
    ```
    
    1. Assign IP addresses to VLAN interfaces and enable inter-VLAN routing:
    
    ```bash
    shellCopy code
    enable
    configure terminal
    interface range FastEthernet0/1 - 10
    switchport mode access
    switchport access vlan 10
    exit
    interface range FastEthernet0/11 - 20
    switchport mode access
    switchport access vlan 20
    exit
    interface range FastEthernet0/21 - 30
    switchport mode access
    switchport access vlan 30
    exit
    
    ```
    
    1. Assign interfaces to VLANs:
    
    ```bash
    shellCopy code
    enable
    configure terminal
    interface vlan 10
    ip address 10.10.10.1 255.255.255.0
    exit
    interface vlan 20
    ip address 10.10.20.1 255.255.255.0
    exit
    interface vlan 30
    ip address 10.10.30.1 255.255.255.0
    exit
    ip routing
    
    ```
    
    These commands are a simplified example, and you may need to adjust them based on your specific Cisco switch model and existing configuration. Make sure to replace "YourNewHostName" and "YourEnablePassword" with your desired values, and adapt VLAN numbers and IP addresses as needed.
    
    Please be cautious when configuring network devices, especially in a production environment, as misconfigurations can lead to network issues.
    
