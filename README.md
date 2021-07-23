# ipwave-hackathon-ietf-111
IETF 111 IPWAVE WG Hackathon

# Prerequisite
* Two laptops with AR94xx WiFi module using ath9k driver
* OCB-enabled linux kernel in Ubuntu 18.04 (https://github.com/ipwave-hackathon-ietf/ipwave-hackathon-ietf-106)
* This source code can also be run locally by swtiching the interface name and local IPv6 address to "lo" and "::1", respectively 

# Compiling
```shell
$ gcc rawsocket_na.c -o rawsocket_na
$ gcc rawsocket_na_rcv.c -o rawsocket_na_rcv
```

# Running Step
* Make sure that the interface name and IPv6 address is properly set in the source code. 
```c
// Interface to send packet through.
strcpy(interface, "lo");
// Destination IPv6 address either:
// 1) unicast address of node which sent solicitation, or if the
// solicitation came from the unspecified address (::), use the
// 2) IPv6 "all nodes" link-local multicast address (ff02::1).
// You need to fill this out.
strcpy (target, "::1");
```
* Open one terminal, using sudo to run rawsocket_na_rcv.
```shell
$ sudo ./rawsocket_na_rcv
```
* Open another terminal, using sudo to run rawsocket_na.
```shell
$ sudo ./rawsocket_na
```
