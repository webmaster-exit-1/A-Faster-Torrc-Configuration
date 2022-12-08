<!-- markdownlint-disable -->
<h1>A Faster/Safer TOR Configuration</h1><br>

  <h5>Not to be confused with tor-browser, that's a different package and not what we are doing here.</h5><br>

 Create a password for **tor** and save the password's **hash** in the **torrc** file.

  ```bash
  tor --hash-password "Write your password here!"
  ```

To start the Tor network in system-wide mode (all connections run through tor).
To start the Tor network on a per port number basis, leave out: **RunAsDaemon 1**.
Also make sure to comment out any ports you don't need. Ports that wont be used.

  ```bash
  sudo -S -u tor tor RunAsDaemon 1 -f /etc/tor/torrc & #By adding "&" this will run in the background.
  ```

 To stop tor and return to the normal network configuration.

  ```bash
  sudo killall tor
  ```

 The Torrc File.

  ```conf
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

  <u><b>**Tip 1**</b></u>: When using <b><u>nyx</b></u> (A cli command & control monitor/station for your tor connections).
  Use the <b>password</b> you used to make the <b>hash</b> in the <b>torrc</b> file to sign in.
  The <b>hash</b> created from the <b>password</b> is just for the <b>torrc</b> configuration file.<br>

  <u><b>**Tip 2**</b></u>: It is also a good idea to rename the default <b>/etc/tor/torrc</b> with the suffix <b>.bak</b> to keep as your backup configuration file instead of overwriting it. ex.: <b>/etc/tor/torrc.bak</b>

  <u><b>**Tip 3**</b></u>: Add as many socks5 ports as needed following the same syntax as in the <b>torrc</b> file.
  For instance. Perhapse you want to run four specific programs and want each of them running there internet connection via tor.
  You would make four socks5 ports available for them in the <b>torrc</b> file, like this.<br>

  ```
  SocksPort 127.0.0.1:9050 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9060 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9070 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9080 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  ```

 And now each of those four programs would use one of each of these four port numbers provided by your `torrc` file: **9050**, **9060**, **9070**, & **9080**.

