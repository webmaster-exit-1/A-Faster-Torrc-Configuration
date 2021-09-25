# A faster tor configuration

First thing is to have tor installed via your distro's package manager; whether it is apt for debian and its derivatives or pacman for arch and its derivatives.
Not to be confused with tor-browser, that's a different package and not what we are doing here.

* Create a tor socks5 hashed password, run this command in the terminal:

```bash
tor --hash-password [hint]("your password here")
```

Copy the hash (this number from the output example=8F:97579857886764747457337) and paste it where it belongs in the torrc file (the part below that's all X's), save and close text editor.

To start the Tor network configuration file, run this command in the terminal:

```bash
tor RunAsDaemon 1 -f /etc/tor/torrc &
```

To stop tor and go back to normal network configuration; run this command in the terminal:

```bash
killall tor
```

A safer & faster Tor network configuration.
Tip: Use that hashed password when using "nyx" a terminal tool for tor; a command & control monitor/station. Not the hash, the password used to make the hashed password, the hash is just for this configuration file.

## This section is the torrc file that must be placed in /ect/tor/torrc

### It is also a good idea to rename the default torrc with the suffix .bak to keep as your backup configuration file instead of overwriting it

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
