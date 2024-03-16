<!-- markdownlint-disable -->
<h1>A Faster/Safer TOR Configuration</h1><br>
  <h3><b>NOTE:</b> Not to be confused with <b>tor-browser</b>, that's a different package and not what we are doing here.</h3><br>

# 

 Create a password for **tor** and save the password's **hash** in the **torrc** file.

  ```bash
  tor --hash-password "Write your password here!"
  ```

 Optional:
  - Create a directory for user configs
  
  ```bash
  sudo mkdir -p /etc/tor/torrc.d
  ```
 To start tor with user config

  ```bash
  tor RunAsDaemon 1 -f /etc/tor/torrc.d/better_torrc
  ```

 To stop tor and return to the normal network configuration.

  ```bash
  sudo killall tor
  ```

 The `better_torrc` File.

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
  HashedControlPassword **(created from the `tor --hash-password` command)**
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

<u><b>Tip 1</b></u>:<br>
  - When using <b><u>nyx</b></u> (A cli command & control monitor/station for your tor connections).<br>
    - Use the <b>password</b> you used to make the <b>hash</b> in the `better_torrc` file to sign in.<br> 
      - The <b>hash</b> created from the <b>password</b> is just for the `better_torrc` configuration file.<br>

<u><b>Tip 2</b></u>:<br> 
  - It is also a good idea to create a `torrc.d` user config folder rather than overwriting the default config `torrc`.<br>

<u><b>Tip 3</b></u>:<br>
  - Add as many socks5 isolated port addresses as needed following the same syntax as in the commented line in the code block above.<br>
    - For instance. Perhapse you want to run four specific programs and want each of them running there internet connection via tor.<br>
      - You would make four isolated socks5 addresses with different ports available for them in the `better_torrc` file, like this.<br>

  ```
  SocksPort 127.0.0.1:9050 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9060 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9070 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  SocksPort 127.0.0.1:9080 IsolateClientAddr IsolateSOCKSAuth IsolateClientProtocol IsolateDestPort IsolateDestAddr
  ```

  - And now each of those four programs would use one of each of these four new socks5 addresses and port numbers provided by your `better_torrc`.

