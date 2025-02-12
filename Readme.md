# Azure function based Let's Encrypt automation

Automatically issue Let's Encrypt SSL certificates for all your custom domain names in Azure.

![.github/workflows/azure-functionapp.yml](https://github.com/MarcStan/lets-encrypt-azure/workflows/.github/workflows/azure-functionapp.yml/badge.svg)

# Motivation

Existing solutions ([Let's Encrypt Site Extension](https://github.com/sjkp/letsencrypt-siteextension), [Let's Encrypt Webapp Renewer](https://github.com/ohadschn/letsencrypt-webapp-renewer)) work well but are target at Azure App Services only.

This solution also enables Azure CDN based domains to use Let's Encrypt certificates (Azure CDN is needed if you want a custom domain name for your static website hosted via azure blob storage).

If you want to know how to setup an Azure CDN based website backed by Blob Storage, [read my blog post](https://marcstan.net/blog/2019/07/12/Static-websites-via-Azure-Storage-and-CDN/).

# Details

The function runs on a daily schedule and automatically renews all certificates that are close to expiring (based on a configurable threshold). In such a case the function will issue a new certificate for the app service/CDN and automatically configure it.

# Features

* automated Let's Encrypt certificate renewal for
    * Azure App Service*
    * Azure CDN
* securely store certificates in keyvaults
* cheap to run (< 0.10$/month)

\*Managed App Service Certificates have been provided for free by Microsoft for a while now and as of [March 17th 2021 also support Apex domains](https://azure.microsoft.com/updates/public-preview-app-service-managed-certificates-now-supports-apex-domains/). This means an Azure native solution exists that automatically rotates app service certificates. I recommend you use it instead of this function for app services.

# Error handling

The function runs every day. In case of an error it will simply retry the next day (Let's encrypt also recommends running the renewal daily). If you would like to be informed of any errors you can set up an azure alert to monitor exceptions in the application insights instance (e.g. exception > 0) and have an email/notification delivered to you.

In the worst case (complete failure of the function for a long time) Let's Encrypt will also send out emails to the domain owners days before the actual expiry.

# Setup

See [Setup](./docs/Setup.md) .

# Changelog

Changelog is [here](Changelog.md).