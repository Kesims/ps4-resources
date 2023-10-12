# IPv4 Cheat Sheet
## Subnet Cheat Sheet

| CIDR | Subnet Mask     | Wildcard Mask   | # of IP Addresses | # of Usable IP Addresses |
|------|-----------------|-----------------|-------------------|--------------------------|
| /32  | 255.255.255.255 | 0.0.0.0         | 1                 | 1                        |
| /31  | 255.255.255.254 | 0.0.0.1         | 2                 | 2*                       |
| /30  | 255.255.255.252 | 0.0.0.3         | 4                 | 2                        |
| /29  | 255.255.255.248 | 0.0.0.7         | 8                 | 6                        |
| /28  | 255.255.255.240 | 0.0.0.15        | 16                | 14                       |
| /27  | 255.255.255.224 | 0.0.0.31        | 32                | 30                       |
| /26  | 255.255.255.192 | 0.0.0.63        | 64                | 62                       |
| /25  | 255.255.255.128 | 0.0.0.127       | 128               | 126                      |
| /24  | 255.255.255.0   | 0.0.0.255       | 256               | 254                      |
| /23  | 255.255.254.0   | 0.0.1.255       | 512               | 510                      |
| /22  | 255.255.252.0   | 0.0.3.255       | 1,024             | 1,022                    |
| /21  | 255.255.248.0   | 0.0.7.255       | 2,048             | 2,046                    |
| /20  | 255.255.240.0   | 0.0.15.255      | 4,096             | 4,094                    |
| /19  | 255.255.224.0   | 0.0.31.255      | 8,192             | 8,190                    |
| /18  | 255.255.192.0   | 0.0.63.255      | 16,384            | 16,382                   |
| /17  | 255.255.128.0   | 0.0.127.255     | 32,768            | 32,766                   |
| /16  | 255.255.0.0     | 0.0.255.255     | 65,536            | 65,534                   |
| /15  | 255.254.0.0     | 0.1.255.255     | 131,072           | 131,070                  |
| /14  | 255.252.0.0     | 0.3.255.255     | 262,144           | 262,142                  |
| /13  | 255.248.0.0     | 0.7.255.255     | 524,288           | 524,286                  |
| /12  | 255.240.0.0     | 0.15.255.255    | 1,048,576         | 1,048,574                |
| /11  | 255.224.0.0     | 0.31.255.255    | 2,097,152         | 2,097,150                |
| /10  | 255.192.0.0     | 0.63.255.255    | 4,194,304         | 4,194,302                |
| /9   | 255.128.0.0     | 0.127.255.255   | 8,388,608         | 8,388,606                |
| /8   | 255.0.0.0       | 0.255.255.255   | 16,777,216        | 16,777,214               |
| /7   | 254.0.0.0       | 1.255.255.255   | 33,554,432        | 33,554,430               |
| /6   | 252.0.0.0       | 3.255.255.255   | 67,108,864        | 67,108,862               |
| /5   | 248.0.0.0       | 7.255.255.255   | 134,217,728       | 134,217,726              |
| /4   | 240.0.0.0       | 15.255.255.255  | 268,435,456       | 268,435,454              |
| /3   | 224.0.0.0       | 31.255.255.255  | 536,870,912       | 536,870,910              |
| /2   | 192.0.0.0       | 63.255.255.255  | 1,073,741,824     | 1,073,741,822            |
| /1   | 128.0.0.0       | 127.255.255.255 | 2,147,483,648     | 2,147,483,646            |
| /0   | 0.0.0.0         | 255.255.255.255 | 4,294,967,296     | 4,294,967,294            |

* /31 is a special case detailed in RFC 3021 where networks with this type of subnet mask can assign two IP addresses as a point-to-point link.

Wildcard is just the inverse of the subnet mask.

## Binary representation of IPv4
IPv4 addresses like 192.168.0.1 are really just decimal representations of four binary blocks. Every IPv4 address is 32 bits long, each block is 8 bits, and represents numbers from 0-255. These blocks are known as octets.

We can convert 172.16.16.51 to

```
10101100.00010000.00010000.00110011
```

To convert an IP address between its decimal and binary forms, you can use this chart:

```
128   64   32   16   8    4    2    1
x     x    x    x    x    x    x    x
```

For the first octet, 172, we can convert it to binary like this:

```
128   64   32   16   8    4    2    1
1     0    1    0    1    1    0    0
```

128 + 32 + 8 + 4 = 172

## Subnetting
Subnetting is the process of dividing a large IP network into smaller, more manageable sub-networks. It allows for efficient use of IP addresses and better network organization.

In subnetting, an IP address is divided into two parts: the network portion and the host portion. The network portion identifies the specific subnet, while the host portion distinguishes individual devices within that subnet.

### Subnet Mask
Subnet mask is a 32-bit number, that determines, which part of IP address is network and which is host. It is usually represented in decimal form, but can also be written in binary form, octal form or hexadecimal form.

Here is an example of subnet mask:

| Type        | Decimal       | Binary                              |
|-------------|---------------|-------------------------------------|
| Subnet mask | 255.255.255.0 | 11111111.11111111.11111111.00000000 |

This means, that all the positions where there is binary 1 belong to network part of IP address, and all the positions where there is binary 0 belong to host part of IP address.

This is usually done with an IP address, so let's take a look at the same subnet mask with an IP address:

| Type        | Decimal       | Binary                              |
|-------------|---------------|-------------------------------------|
| IP address  | 192.168.0.101 | 11000000.10101000.00000000.01100101 |
| Subnet mask | 255.255.255.0 | 11111111.11111111.11111111.00000000 |

This means, that for this particular subnet, we can change the last octet of the IP address to anything between 0 and 255, and it will still be in the same subnet. On the other hand, the network part has to stay the same, otherwise it will be in a different subnet.

### CIDR Notation
CIDR notation is a way to represent a subnet mask in a shorter form. It is a single number, that represents number of `1` in the mask. For the example above, the CIDR notation would be `/24`, because there are 24x `1` in the subnet mask.
You can check out the table on the top of this page to see the CIDR notation for each subnet mask.


### How to Calculate Subnets
Generally the assignment is to take a given IP address and subnet mask and figure out the network ID, broadcast address, and valid IP address range for that subnet.

Let's take a look at `132.51.12.43 /27` example:

The steps are following:
1) Look at the subnet mask and figure out the **size of the subnet**. You can do this using a general formula: `2^(32 - subnet mask)`. For example, if the subnet mask is /27, then the formula would be `2^(32 - 27) = 2^5 = 32`. This means that the subnet has 32 IP addresses.
2) Calculate the number of **usable IP adresses** in the subnet. This can be done using the formula `subnet_size - 2`. In case of the previous example, the number of usable IP addresses would be `32 - 2 = 30`.
3) Calculate the **network ID**. All subnets have to go in order in the IP address space, so the network ID is always the first IP address in the subnet. For example, if the IP address is `132.51.12.43` and the subnet mask is `/27`, then the network ID would be `132.51.12.32`, as it is clearly in the second available subnet. (There is a /27 subnet of x.x.x.0 - x.x.x.31, then x.x.x.32 - x.x.x.63, etc.)
4) Calculate the **broadcast address**. The broadcast address is always the last IP address in the subnet. For example, if the IP address is `132.51.12.43` and the subnet mask is `/27`, then the broadcast address would be `132.51.12.63`.

## IP Classes
| Class | Range                       | Network ID              | Host ID | Default Subnet Mask            | Networks  | Usable Addresses per Network |
|-------|-----------------------------|-------------------------|---------|--------------------------------|-----------|------------------------------|
| A     | 1.0.0.0 - 127.255.255.255   | 8 bits                  | 24 bits | 255.0.0.0 (or /8 in CIDR)      | 128       | 16,777,214                   |
| B     | 128.0.0.0 - 191.255.255.255 | 16 bits                 | 16 bits | 255.255.0.0 (or /16 in CIDR)   | 16,384    | 65,534                       |
| C     | 192.0.0.0 - 223.255.255.255 | 24 bits                 | 8 bits  | 255.255.255.0 (or /24 in CIDR) | 2,097,152 | 254                          |
| D     | 224.0.0.0 - 239.255.255.255 | Reserved for multicasts | -       | -                              | -         | -                            |
| E     | 240.0.0.0 and above         | Experimental            | -       | -                              | -         | -                            |
