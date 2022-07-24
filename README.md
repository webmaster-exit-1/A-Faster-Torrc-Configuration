<!-- markdownlint-disable MD033 -->
<h1>A Faster/Safer TOR Configuration</h1><br>

  <h6>Not to be confused with tor-browser, that's a different package and not what we are doing here.</h6><br>

    * Create a tor socks5 hashed password. Run this command in the terminal:<br>

```bash
  tor --hash-password "Write your password here!"
```

    * Copy the hash (this number from the output of the command above example=8F:97579857886764747457337) and paste it where it belongs in the torrc file (the part below that's all X's), save and close text editor.<br>

    * To start the Tor network with the new configuration file, run this after copying it to /etc/tor/torrc:<br>

```bash
  sudo -u tor tor RunAsDaemon 1 -f /etc/tor/torrc &
```

    * To stop tor and go back to normal network configuration; run this command in the terminal:<br>

```bash
  killall tor
```

```h
  VirtualAddrNetwork 10.192.0.0/10
  AutomapHostsOnResolve 1
  AutomapHostsSuffixes .exit,.onion
  TransPort 127.0.0.1:9040 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9050 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  # ------------------------------------------------------------------------------------------------------------------
  # Create as many socks ports as you need following the same syntax:   (Example following port 9050 above)
  # ------------------------------------------------------------------------------------------------------------------
  # SocksPort 127.0.0.1:9085 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  # ------------------------------------------------------------------------------------------------------------------
  ControlPort 9051
  HashedControlPassword XX:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  DNSPort 9053
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

  <u><b>Tip 1</b></u>: Use that hashed password when using "nyx" a terminal tool for tor; a command & control monitor/station. Not the hash, the password used to make the hash. The hash is just for this configuration file.<br>

  <u><b>Tip 2</b></u>: It is also a good idea to rename the default torrc with the suffix .bak to keep as your backup configuration file instead of overwriting it.
