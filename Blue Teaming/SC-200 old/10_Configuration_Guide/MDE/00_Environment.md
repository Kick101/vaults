>You must be a *Security Administrator* for the tenant. 

**On the Set-up preferences page, you can set the:**

- **Data storage location** - Determine where you want to be primarily hosted: US, EU, or UK. You can't change the location after this is set up, and Microsoft won't transfer the data from the specified geolocation.
- **Data retention** - The default is *six months*.
- **Enable preview features** - The default is on, can be changed later.

**To access the Microsoft Defender portal settings for Endpoints:**

1. Go to ([https://security.microsoft.com](https://security.microsoft.com/))
2. Select **Settings**.
3. Select **Endpoints**.

## Proxy Network configuration

- The **(MDE) sensor** relies on **WinHTTP** to report data and communicate with the cloud service.
- It runs under the *LocalSystem account* in the system context.
- The WinHTTP configuration setting is independent of the Windows Internet (WinINet) internet browsing proxy settings and can only discover a proxy server by using the following discovery methods:
    - **Transparent proxy**
    - **Web Proxy Autodiscovery Protocol (WPAD)**

If a Transparent proxy or WPAD has been implemented in the network topology, there's no need for special configuration settings.