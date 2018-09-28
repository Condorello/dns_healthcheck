# routeros_dns_healthcheck
Mikrotik RouterOS script to check DNS service and update it if required

This script is used on my Mikrotik devices in combination with Pi-Hole to block ADS. Since I have dynamic IP and my Pi-Hole is in cloud behind firewall, I need to make sure that DNS service is available on the event my IP has changed and firewall rules in cloud didn't adapt to that change yet.

One of the issues which drived me to create this script is Mikrotik RouterOS's build it function "ip cloud", which is used for Dynamic DNS. In the event my IP changed, "ip cloud" will try to update it, but since DNS service is not available - will fail. As result, firewall in cloud will not see that change and will not update rule set.

To solve this, in the event DNS is not reachable, script will set temporary CloudFare DNS 1.1.1.1 as primary DNS. This will allow "ip cloud" function to perform updates. Once firewall will observe IP address change and update itself, script will verify if original DNS is available again and will revert back to original configuration.

Please note: in my setup I'm using single DNS configuration on Mikrotik side. This script may not work if you have two DNS servers configured and may require update for variable $ConfiguredDNS.

Also, please fill variables and configure SMTP if you want to receive email message on event, otherwise comment lines where you specify your email address as variable and lines where email is generated and sent.

Schedule this script exectuion as you see fit. In my case, I'm using 300 seconds delay between script execution.

For more info about Mikrotik RouterOS's scripting please check official Wiki https://wiki.mikrotik.com/wiki/Manual:Scripting
