# Setting up SSL certificate using Let's Encrypt for Nextcloud instance

Navigating to https://nextcloud.fossterer.com/settings/admin/overview showed me a warning that I'm not using HTTPS.
So I set out to https://letsencrypt.org/getting-started/ to pick the https://certbot.eff.org/lets-encrypt/ubuntubionic-apache path.

My DNS provider is *not* by default supported and hence I skipped the dns-plugin step.
The setup still went smooth and by the end, everything was good: https://www.ssllabs.com/ssltest/analyze.html?d=nextcloud.fossterer.com