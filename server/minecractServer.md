# Minecraft Server

## Build Server
[Reference (Japanese)](https://qiita.com/sjchorcl/items/20f6741cc2090a1824c2)

### Notes
#### Ingress Rules
Add ingress rules on both TCP and UDP.([ref: Oracle Cloud Infrastructure Documentation](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/securityrules.htm))
- CIDR(Classless Inter-Domain Routing): Use IP address space efficiently by spliting (network and host) with any block.
```
Source CIDR: 0.0.0.0/0
```
The CIDR block where the traffic originates from. 0.0.0.0/0 to indicate all IP addresses.
```
Destination port: 25565
```
25565 is the initial server operating port of Minecraft server.

#### SSH Login
```sh
SSH -i Path-to-the-key opc@ipaddress
```
ops is the account name for oracle linux.
#### Firewall Rules
Ingress rules should align to OS firewall rules.

Open ports
``` sh
sudo firewall-cmd --permanent --zone=public --add-port=25565/tcp
sudo firewall-cmd --permanent --zone=public --add-port=25565/udp
sudo firewall-cmd --reload
```

#### Install Java
``` bash
curl -s "https://get.sdkman.io" | bash
```
curl is command to get shell script. Executing it with pipe.

[SDKMAN](https://sdkman.io/) is a tool for managing SDKs on UNIX based systems, especially used for managing Java(JDK, JVM, and so on).

#### Start Server
Java options
``` 
Xmx1024M: max memory size(1024M)
Xms1034M: min memory size(1024M)
```
[server.jar options(Japanese)](https://minecraft.fandom.com/ja/wiki/%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB/%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%AE%E3%82%BB%E3%83%83%E3%83%88%E3%82%A2%E3%83%83%E3%83%97#Minecraft.E3.81.AE.E8.A8.AD.E5.AE.9A)

#### Up server 24/7
Use screen to activate server 24/7, after closing terminal.

Use yum in Oracle Linux(its deb based).

``` sh
screen
```
Up the server after the command above.

``` sh
screen -li
```
List active screens.

``` sh
screen -r screen_name
```
Get in to the screen.

#### Update Server
1. Stop server.
2. Back up
    - banned-ips.json
    - banned-players.json
    - ops.json
    - server.propeties
    - world/
3. Wget new server.jar
4. Delete old server.jar
5. Up server
