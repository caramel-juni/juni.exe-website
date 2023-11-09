# Configuring a Cisco AP

This assumes that you have an autonomous Cisco AP with no previous configuration. I'm using a 3702i AP.

The AP's current IP can be found through the DHCP server on your router.

## Connect to AP

Connect to the AP using telnet. Your AP might have a different IP

```bash
telnet 192.168.1.182
```

Login with Cisco/Cisco

You should get shell, though it's not unix shell.

```
ap>
```

From here, you can run commands, but they are quite different from usual bash/sh commands.

To elevate privileges, type `enable` and hit enter. It should prompt you for another login, where you can re-enter the default password again.

```
ap>enable
Password:
ap#
```

Note that the `#` or `>` characters can be thought of as bash shell's `$`, and anything before the character can be safely ignored.

## Network Connection

Changing the AP's network IP address can be done on the clientside. Note that settings apply instantly, so you may need to re-connect midway through.

I'll show configuring an AP to `192.168.1.9`

```ap>enable
Password: 
ap#configure terminal
ap(config)#interface bv1
ap(config-if)#ip address 192.168.1.9 255.255.255.0
==== this cuts off as it applies new ip. Need to open new telnet session on new ip.
ap(config-if)#exit
ap(config)#ip default-gateway 192.168.1.1
ap(config)#exit
ap#
```

 - `enable` to get privileges
 - `configure terminal` to configure things through terminal
 - `interface bv1` specifies the interface to configure
 - `ip address X Y` sets the interface ip address to X with subnet mask Y
    - At this point you may lose connection to the AP as it configures the interface to the static ip. Reconnect on the new IP as necessary
 - `exit` exits configuring the interface
 - `ip default gateway Z` sets Z as the default gateway


## SSID Setup

#### Create SSID

```
ap#configure terminal
ap(config)#dot11 ssid [SSID Name]
ap(config-ssid)#guest-mode
ap(config-ssid)#authentication open
ap(config-ssid)#authentication key-management wpa version 2
ap(config-ssid)#wpa-psk ascii [Private Network Key]
ap(config-ssid)#exit
ap(config)#
```

 - `configure terminal` to configure things through terminal
 - `dot11 ssid` specifies that we're configuring an 802.**11** ssid
 - `guest-mode` allows guests (client devices) to connect
 - `authentication open` allows guest devices to *try to* authenticate
 - `authentication key-management wpa version 2` set authentication key management to WPA2
 - `wpa-psk ascii [Private Network Key]` Set network key
 - `exit` exits config mode

#### Bind SSID to Wireless Radios

For 2.4 GHz

```
ap(config)#interface dot11Radio0
ap(config-if)#encryption mode ciphers aes-ccm
ap(config-if)#ssid [SSID Name]
ap(config-if)#channel least-congested
ap(config-if)#no shutdown
ap(config-if)#exit
```

And for 5 GHz

```
ap(config)#interface dot11Radio1
ap(config-if)#encryption mode ciphers aes-ccm
ap(config-if)#ssid [SSID Name]
ap(config-if)#channel least-congested
ap(config-if)#channel width 80
ap(config-if)#no shutdown
ap(config-if)#exit
ap(config)#exit
ap(config)#
```

 - `interface dot11Radio0` specify which interface to configure
 - `encryption mode ciphers aes-ccm` specifies the encryption mode
 - `ssid [SSID Name]` specifies the ssid to be bound
 - `channel least-congested` specifies the network channel. `least-congested` allows for dynamic channel selection.
 - `channel width 80` set the channel width to 80 MhHz (applicable to 5 GHz only)
 - `no shutdown` turn on the interface
 - `exit` exits config mode

I find it really interesting that Cisco uses `no shutdown` as the command to turn on an interface, but I guess it makes sense given that `shutdown` shuts down that interface


## Save Configuration

Please remember to do this step, otherwise the AP will reset to default when it is power cycled.

```
ap#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
[OK]
ap#
```

Copy the running-config to the startup-config.

When prompted with Destination filename (`Destination filename [startup-config]?`), simply press Enter



## Back Up Configuration

To back up these configuration changes, you can copy the running config to your local machine over tftp. 

Start a tftp server (I used [tftpd64](https://pjo2.github.io/tftpd64/)) and set the interface to the correct ip.

Then, on the AP, run the following, changing out the IP shown here for the IP of the machine where the tftp server is running.

```
ap#copy running-config tftp://192.168.1.111/destFileName
```


## Restore Configuration

Restoring a config is similar to backing it up, but this time it is reversed. Again, remember to change the IP and filename to the correct one for you.

```
ap#copy tftp://192.168.1.111/destFileName running-config 
```

