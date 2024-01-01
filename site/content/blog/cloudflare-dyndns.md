+++
title = "FRITZ!Box DynDNS with Cloudflare"
date = "2020-02-12"
tags = [
    "cloudflare",
]
+++

## Step1: Create a Cloudflare custom API token

Create a [Cloudflare API token](https://dash.cloudflare.com/profile/api-tokens) with **read permissions** for the scope `Zone.Zone` and **edit permissions** for the scope `Zone.DNS`.

![Cloudflare Custom API Token](/images/cloudflare-dyndns1.png)

## Step 2: Set up DynDNS on your FRITZ!Box

| FRITZ!Box Setting | Value                                                                                             | Description                                                                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Update URL        | `https://dyndns.nicoo.org/?token=<pass>&record=www&zone=example.com&ipv4=<ipaddr>&ipv6=<ip6addr>` | Replace the URL parameter `record` and `zone` with your domain name. If required you can omit either the `ipv4` or `ipv6` URL parameter. |
| Domain Name       | www.example.com                                                                                   | The FQDN from the URL parameter `record` and `zone`.                                                                                     |
| Username          | admin                                                                                             | You can choose whatever value you want.                                                                                                  |
| Password          | ●●●●●●                                                                                            | The API token you’ve created earlier.                                                                                                    |

The code behind `https://dyndns.nicoo.org` is open source and available on [GitHub](https://github.com/L480/cloudflare-dyndns)!
