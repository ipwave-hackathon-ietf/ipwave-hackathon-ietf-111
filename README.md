# ipwave-hackathon-ietf-111
IETF 111 IPWAVE WG Hackathon

# Prerequisite
* Two laptops with AR94xx WiFi module using ath9k driver
* OCB-enabled linux kernel in Ubuntu 18.04 (https://github.com/ipwave-hackathon-ietf/ipwave-hackathon-ietf-106)
* This source code can also be run locally by swtiching the interface name and local IPv6 address to "lo" and "::1", respectively 

# Compiling
```shell
$ gcc rawsocket_na.c -o rawsocket_na
```
