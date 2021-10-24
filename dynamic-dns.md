# Configure Dynamic DNS
This tutorial explains how to configure dynamic dns. This can be usefull if your ISP only gives you a dynamic IP address (and not a static one) but you still want to access your edgerouter (e.g. connect via VPN).
Prerequisite for this tutoiral is a valid domain that you own (e.g. example.com) and an free account at [cloudflare](https://www.cloudflare.com/).

Note you have to create an *A Record* in your cloudflare dashboard beforehand. E. g. `router.example.com A 1.2.3.4`.

Please replace 
- router.example.com -> with your subdomain
- info@example.com -> with your cloudflare e-mail address
- your-cloudflare-api-key -> with you cloudflare global API Key. For help look [here](https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys) (note this is not your cloudflare login password)
- example.com -> with your domain

```
configure
set service dns dynamic interface pppoe0 service custom-cloudflare host-name router.example.com
set service dns dynamic interface pppoe0 service custom-cloudflare login info@example.com
set service dns dynamic interface pppoe0 service custom-cloudflare password <your-cloudflare-api-key>
set service dns dynamic interface pppoe0 service custom-cloudflare protocol cloudflare
set service dns dynamic interface pppoe0 service custom-cloudflare options zone=example.com
commit
save

```

## Verify the configuration
With `show dns dynamic status` you can list the status of the dynamic dns confguration.
When you `ping router.example.com` the IP address of your router should appear. 


Source: https://help.ui.com/hc/en-us/articles/204976324-EdgeRouter-Custom-Dynamic-DNS
