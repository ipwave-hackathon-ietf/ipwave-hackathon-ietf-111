# ipwave-hackathon-ietf-111
IETF 111 IPWAVE WG Hackathon
The source code here is based on the implementation of a open source code here:
https://www.pdbuchan.com/rawsock/rawsock.html

A demo in YouTube is here: https://youtu.be/OTYtLOBX1CI

# Prerequisite
* This source code can be run locally by swtiching the interface name and local IPv6 address to "lo" and "::1", respectively.
* This source code can also be run on top of two laptops with AR94xx WiFi module using ath9k driver
  * OCB-enabled linux kernel in Ubuntu 18.04 (https://github.com/ipwave-hackathon-ietf/ipwave-hackathon-ietf-106)
  * When using two OCB-enabled laptops, the interface name and IPv6 address shall be properly set.

# Compiling
```shell
$ gcc rawsocket_na.c -o rawsocket_na
$ gcc rawsocket_na_rcv.c -o rawsocket_na_rcv
```

# Running Step
* Make sure that the interface name and IPv6 address are properly set in the source code. Here we set the interface to "lo" and target ipv6 address to "::1".
```c
// sender side
// Interface to send packet through.
strcpy(interface, "lo");
// Destination IPv6 address either:
// 1) unicast address of node which sent solicitation, or if the
// solicitation came from the unspecified address (::), use the
// 2) IPv6 "all nodes" link-local multicast address (ff02::1).
// You need to fill this out.
strcpy (target, "::1");
```
```c
// receiver side
// Interface to receive packet on.
strcpy (interface, "lo");
```
* Open one terminal, using sudo to run ```rawsocket_na_rcv```.
```shell
$ sudo ./rawsocket_na_rcv
```
* Open another terminal, using sudo to run ```rawsocket_na```.
```shell
$ sudo ./rawsocket_na
```

# Main Code Explained
* VMI Option
```c
  // Put a dummy GPS data into options buffer.
  options[0] = 2;           // Option Type - "target link layer address" (Section 4.6 of RFC 4861), need to be redefined
  options[1] = optlen / 8;  // Option Length - units of 8 octets (RFC 4861), need to be redefined

  // Westin Josun Busan: longitude 35.155988, latitude 129.154134
  char longitude[14] = "35.1559880000";
  char latitude[14] =  "129.154134000";
  char delimiter[2]=",";

  for (i = 0; i < sizeof(longitude); i++){
    options[i + 2] = (uint8_t)longitude[i];
  }
  options[2 + sizeof(longitude)] = (uint8_t)delimiter[0];

  for (i = 0; i < sizeof(latitude); i++){
    options[i + 2 + sizeof(longitude) + 1] = (uint8_t)latitude[i];
  } 

  // Report advertising node's mobility to stdout.
  printf("Advertising car's VMI for interface %s is ", interface);
  for (i = 0; i < 30; i++){
    printf("%c", options[i + 2]);
  }
  printf("\n");
```
