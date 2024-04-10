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

  # Additional Settings for Speed and Security

  AvoidDiskWrites 1
  FascistFirewall 1
  Socks5DestroyLocalAddresses 1
  Socks5DestroyLocalPorts 1
  Socks5DestroyLocalDomain 1
  DataDirectory /var/lib/tor
  ```

#

This configuration includes additional settings to optimize speed and security:

#

- AvoidDiskWrites: Enables the avoid disk writes feature, which reduces disk I/O and improves performance.
- FascistFirewall: Enables the fascist firewall feature, which blocks all outgoing connections except for those explicitly allowed.
-  - Socks5DestroyLocalAddresses,       
   - Socks5DestroyLocalPorts, 
   - Socks5DestroyLocalDomain: 
     - Destroys local information in SOCKS5 requests, which improves anonymity.
- DataDirectory: Specifies the directory where Tor will store its data files, which can help to prevent unauthorized access.

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


## Below is an explanation of each configuration option.

1. VirtualAddrNetwork: Sets the range of IP addresses used for virtual circuits. The range 10.192.0.0/10 is used.

2. AutomapHostsOnResolve: Enables automatic mapping of .onion and .exit hostnames to IPv6 addresses.

3. AutomapHostsSuffixes: Specifies the suffixes that trigger the automatic mapping of hostnames.

4. TransPort: Configures the port used for incoming connections and applies isolation policies.

5. SocksPort: Configures the port used for outgoing SOCKS connections and applies isolation policies.

6. ControlPort: Enables the control port for Tor's command-line interface.

7. HashedControlPassword: The hashed password for the control port. This should be generated using the `tor --hash-password` command.

8. DNSPort: Configures the port used for DNS queries.

9. Sandbox: Enables the sandboxing feature to prevent Tor from accessing external resources.

10. HardwareAccel: Enables hardware acceleration for improved performance.

11. TestSocks: Enables testing of SOCKS connections.

12. AllowNonRFC953Hostnames: Allows non-RFC953 compliant hostnames.

13. WarnPlaintextPorts: Warns about plaintext ports that could be used for traffic analysis.

14. ClientRejectInternalAddresses: Rejects connections to internal addresses.

15. NewCircuitPeriod: Sets the time period for creating new circuits.

16. MaxCircuitDirtiness: Sets the maximum time a circuit can be unused before being closed.

17. MaxClientCircuitsPending: Sets the maximum number of pending circuits allowed per client.

18. UseEntryGuards: Enables the use of entry guards to protect against routing attacks.

19. EnforceDistinctSubnets: Enforces the use of distinct subnets for each circuit.

To add more socks ports, follow the example provided in the comments.

