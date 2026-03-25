N8 and Rickyg Updated Guide 2019

Carding Guide - Exploiting PayPal

Table Of Contents
-------
1| Abbreviations
2| Introduction
3| Cards To Use & BIN Numbers
4| Operation Security (OPSEC)
5| Acquiring Essential Details
6| Preparing Your System
7| Creating The Digital Identity
8| Using The Exploit
9| Finding Cardable Sites
 

1) Abbreviations
-------------------------------------------------------------------------------------------------------------------------------
RDP - Remote Desktop Protocol
VPN - Virtual Private Network
BIN - Bank Identification Number
VBV - Verified By Visa
MSC - MasterCard Secure Code
OPSEC - Operation Security
 
 
2) Introduction
-------------------------------------------------------------------------------------------------------------------------------
As a veteran carder, I'll express what I've learned over the years. You need to imitate the 
'Card Holder' enough to pass with 'points' that the processor uses to verify payments. 
You should think of it as a point scoring system that will decide whether you have enough 
'points' at the time to pass with whatever the processor was checking for. You gain points 
for every thing that matches the card holder. PayPal looks for 240+ things to authorize a 
payment. If you are able to imitate the card holder well enough you will have enough 'points' 
for the processor to put through the transaction. I will detail how to exploit this system.
 
         Discover what they look for with these resources
        PCI-DSS :: Payment Card Industry - Data Security Standards
        www.pcisecuritystandards.org/pdfs/PCI_DSS_Resource_Guide_(003).pdf
        What Processors Track and Check For
        www.threatmetrix.com/digital-identity-periodic-table
 

3) Cards To Use & BIN Numbers
-------------------------------------------------------------------------------------------------------------------------------
Every Card Can Be Used to exploit PayPal and access funds to a limit of $800 per transaction.
The reason it is capped at $800 is because higher amounts may cause the account to freeze
and require identification to release or spend the available funds. BINs And Card Types
make no difference to the succsses of a credit card transaction using the method I've
detailed below. "Card Types" can indicate a higher balance and "BIN" numbers may indicate
that the bank does not make use of VBV/MSC. Considering PayPal does not utilise VBV/MSC
and we can only cash $800 per transaction, the "BIN" numbers and "Card Types" are not
important and I do not recommend purchasing cards based on those details. The card providers
I've listed sell card details for much more affordable prices that suit this exploit.

4) OPERATIONAL SECURITY (OPSEC)
-------------------------------------------------------------------------------------------------------------------------------
The value of keeping your identity hidden is core to the operation and is considered
the most important practice that comes before anything else. Make sure you set your
connection up to the exact details provided below as this method has been proven to work
and uses international law to keep your business untracable hiding your identity.
This practice is known as "OPSEC".

#1 - Use your current version of Tor to download and install the free version of
       VMware Workstation for windows or if you're using mac VMWare Fusion.
 
        VMWare Workstation For Windows
        www.vmware.com/au/products/workstation-pro/workstation-pro-evaluation.html
        VMWare Fusion For Mac Users
        www.vmware.com/au/products/fusion/fusion-evaluation.html
 
#2 - Download the latest version of Ubuntu as an ISO file.
 
        The Latest Ubuntu Version (ISO FILE)
        www.ubuntu.com/download/desktop
 
#3 - Mount the Ubuntu ISO file to the VMware player and start the virtual machine.
 
        How To Install Ubuntu VMWare Player (Windows)
        www.youtube.com/watch?v=AlOcHa_KB0k
 
        How To Install Ubuntu VMWare Fusion (MAC)
        www.youtube.com/watch?v=b7JyIf8bmL8
 
#4 - Use the provided firefox version in Ubuntu to download the latest version of Tor
       Browser.
     
        Tor Download
        www.torproject.org
 
#5 - Download & Install NordVPN which is the VPN service with servers based in Panama.
       In Panama requests made to their servers do not get logged as is not required by law.
       This provides perfect conditions for a VPN service to operate and hide your identity.
       They also provide servers that connect to Tor as an extension to the connection.
 
        Purchase a NordVPN Subscription
        www.nordvpn.com
 
      Your Connection Should Now Look Like This:
      Your OS -> VMWare (Ubuntu) -> NordVPN (Tor Extension)

#6 - Download and install the Remmina RDP client. This will enable you to connect to RDPs.
       
        Remmina RDP Client
        www.remmina.org/how-to-install-remmina
 
        Your Connection Should Now Look Like This:
        Your OS -> VMWare (Ubuntu) -> NordVPN (Tor Extension) -> Remmina RDP Client
 
 
5) Acquiring Essential Details
-------------------------------------------------------------------------------------------------------------------------------

#1 - You will need to decide which location you will use during the exploit.
 
#2 - Purchase credit card credentials in the location you chose
         Suggested Vendors
         - cclean
         - miomentor
         - friskmint
         - savastano
         - darkbusiness
 
#3 - Purchase the details of a Socks5 connection that is within 20 miles of the card holders
address
         Suggested Vendors
         - slipperypete
         - Goldoratt
 
#4 - Purchase a (Windows 10) RDP connection which has adminsitrative privelidges.
         Suggested Vendors
         - g0ldenboy
         - gwolf
         - TOPMONEYMAKER
 
#5 - Purchase a Hotmail account that is atleast a few months old. You can find these in
email lists being sold on the market.
         Suggested Vendors
          - Stackz420
 
#6 - Purchase a verified PayPal account that does not have balance.
         Suggested Vendors
         - topkittzz
         - ubuntu009

You should now have all the details you need to attempt the exploit.
         - Target Location
         - Credit Card Details (Location Specific)
         - Windows 10 RDP Connection
         - Socks5 Connection within 20 miles of the Card Holder's address
         - Aged Hotmail account
         - Verfied PayPal account with NO balance
 

6) Preparing Your System
-------------------------------------------------------------------------------------------------------------------------------
NOTE:
You may have heard about using software such as fraudfox and others alike to create new
browser fingerprints and spoof the device ID. The fraud detection system we are exploiting
specifically looks for signs of such use and requires a manual set-up and genuine details.
 
#1 - Using Remmina and making sure you're using NordVPN with a Tor extension, connect to the
       RDP connection using the details you were provided.
 
#2 - Create an admin account and use the hotmail email address. Name the account with the
       first name of the card holder and change the time-zone to match the card holders location.
 
#3 - Open "Internet Explorer" and the "Settings" menu. Click "Internet Options" and navigate
       to the "Connections" tab. Click on "LAN settings" and under the "Proxy Server Options"
       check the box to "Use a proxy server" then click the "Advanced" button that becomes
       available. Now in the "Proxy Settings" window, in the empty field next to "Socks" enter
       the details of your Socks5 connection and click "OK".
 
#4 - Using Internet Explorer, download and install "Google Chrome" and set it as your default
       browser.
 
#5 - To make sure the card details are correct and has a balance, we will make a purchase
       using a payment processor that is easy to card (Authorize.net). Access www.guy4game.com
       and purchase a listed item. If it goes through, you have a valid card with a balance.
       If it does not, purchase another card and update the systems clock to match the location.
 

7) Creating The Digital Identity
-------------------------------------------------------------------------------------------------------------------------------

#1 - You now need to create a "Digital Identity" with ThreatMetrix. PayPal uses the fraud
       detection system "ThreatMetrix" and 240+ key identifying factors to authorize a credit
       card transaction. By making a small transaction on another site that utilises
       ThreatMetrix it will create a "Digital Profile" we can use later to exploit PayPal.
 
#2 - Using Google Chrome go to Netflix.com and purchase a subscription using the credit
       card details and register using the details of the card holder.
 

8) Using The Exploit
-------------------------------------------------------------------------------------------------------------------------------

#1 - Navigate to www.bigcommerce.com and pay for a subscription using your credit card
       details. Set up an online store with a listing for $800 USD and link your verified
       PayPal account to process transactions.
#2 - Purchase the listing from your online store and you should now have an $800 balance
       on your PayPal account that you can use on localbitcoins.com to purchase bitcoin.
 

9) Finding Cardable Sites
-------------------------------------------------------------------------------------------------------------------------------