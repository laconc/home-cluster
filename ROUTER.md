## Router Config

In here, we're going over the configuration needed for our Ubiquiti router.

1. SSH into the router
2. Install and configure `frr`
    ```bash
    curl -s https://deb.frrouting.org/frr/keys.asc | apt-key add -
    echo deb https://deb.frrouting.org/frr bullseye frr-stable | tee -a /etc/apt/sources.list.d/frr.list
    apt update
    apt install frr frr-pythontools -y

    cat > /etc/frr/frr.conf << EOF
    ! -*- bgp -*-
    !
    frr defaults traditional
    log file stdout
    !
    router bgp 65510
     bgp ebgp-requires-policy
     bgp router-id 10.0.0.1
     maximum-paths 1
     !
     ! Peer group for DNS
     neighbor DNS peer-group
     neighbor DNS remote-as 64512
     neighbor DNS activate
     neighbor DNS soft-reconfiguration inbound
     neighbor DNS timers 15 45
     neighbor DNS timers connect 15
     !
     ! Neighbors for DNS
     neighbor 10.4.0.201 peer-group DNS
     neighbor 10.4.0.202 peer-group DNS
     neighbor 10.4.0.203 peer-group DNS

     address-family ipv4 unicast
      redistribute connected
      !
      neighbor DNS activate
      neighbor DNS route-map ALLOW-ALL in
      neighbor DNS route-map ALLOW-ALL out
      neighbor DNS next-hop-self
     exit-address-family
     !
    route-map ALLOW-ALL permit 10
    !
    line vty
    !
    EOF

    chown frr:frr /etc/frr/frr.conf
    ```
3. Enable `bgpd` in `/etc/frr/daemons`
