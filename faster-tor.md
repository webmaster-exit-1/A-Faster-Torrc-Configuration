```text
Create a tor and socks5 hashed password: 
```

```bash
tor --hash-password your-password-here 
```

```text
copy the hash (this number from the output example=8F:97579857886764747457337) and paste it where it belongs in the torrc file, save and close text editor. 
Run this file from the terminal: 
```

```text
To start the Tor network configuration file, run this command in the terminal.
```

```bash
tor RunAsDaemon 1 -f /etc/tor/torrc & 
``` 

```text
To stop tor and go back to normal network configuration; run this command in the terminal: 
```

```bash
killall tor
```

```text
A safer & faster Tor network configuration.
Tip: Use that hashed password when using "nyx" a terminal tool for tor; a command & control monitor/station. Not the hash, the password used to make the hashed password, the hash is just for this configuration file.
```

# This section is the torrc file that must be placed in /ect/tor/torrc

```text
VirtualAddrNetwork 10.192.0.0/10
AutomapHostsOnResolve 1
AutomapHostsSuffixes .exit,.onion
TransPort 127.0.0.1:9040 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
SocksPort 127.0.0.1:9050 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
ControlPort 9051
HashedControlPassword XX:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXx
DNSPort 127.0.0.1:9053
Sandbox 1
HardwareAccel 1
TestSocks 1
AllowNonRFC953Hostnames 0
WarnPlaintextPorts 23,109,110,143,80
ClientRejectInternalAddresses 1
NewCircuitPeriod 40
MaxCircuitDirtiness 600
MaxClientCircuitsPending 48
UseEntryGuards 1
EnforceDistinctSubnets 1
```
