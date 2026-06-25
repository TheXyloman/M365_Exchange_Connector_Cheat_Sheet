# Exchange M365 Connector Scan-to-Email Cheat Sheet

A standalone visual runbook for engineers setting up scan-to-email through Microsoft 365 SMTP relay.

The guide walks engineers through creating an Exchange Online inbound connector authenticated by a static public IP address, finding the tenant MX endpoint, updating SPF, configuring common printer brands, testing SMTP connectivity, and troubleshooting common failures.

It also includes a fillable engineer setup record at the top of the page. Values entered there automatically update the examples and command snippets below, and the generated ticket notes can be copied to the clipboard.

## Open the Guide

Open [`index.html`](./index.html) directly in a browser.

No build step, package install, web server, or external assets are required. The page is self-contained and includes embedded CSS and JavaScript.

## What It Covers

- Confirming when Microsoft 365 SMTP relay is the right method for scan-to-email
- Finding the sending origin's static public IP address
- Finding the Microsoft 365 MX endpoint for the sender domain
- Creating an Exchange Admin Center connector
- Adding the sending IP address to the domain SPF record
- Configuring printer/MFP SMTP settings
- Vendor notes for HP, Lexmark, Canon, and Brother devices
- Testing with PuTTY on Windows
- Testing with PowerShell, macOS Terminal, `dig`, `nc`, and OpenSSL
- DNS, gateway, date/time, TLS, and firmware checks
- Firewall and ISP considerations for outbound SMTP on TCP port 25
- Troubleshooting delivery, SPF, connector, and device issues
- Fillable engineer setup record for ticket notes and change records
- One-click copy of generated setup notes to the clipboard

## Recommended Setup Path

1. Gather the customer domain, printer details, public IP, and MX endpoint.
2. Confirm the public IP is static and exclusive to the customer.
3. Create the Exchange Online inbound connector.
4. Update the existing SPF TXT record with the sending IP.
5. Configure the printer to send to the MX endpoint on port 25.
6. Test from the network and from the device.
7. Use message trace, firewall logs, DNS checks, and the troubleshooting matrix if delivery fails.

## Key SMTP Relay Values

| Setting | Value |
| --- | --- |
| SMTP server / smart host | Customer MX endpoint, for example `contoso-com.mail.protection.outlook.com` |
| Port | `25` |
| TLS / STARTTLS | Enable where supported |
| SMTP authentication | Disabled / none |
| Sender address | Address in a verified accepted domain, for example `scanner@contoso.com` |

## Important Notes

- Do not create multiple SPF TXT records. Update the existing record that begins with `v=spf1`.
- Do not use dynamic or shared public IP addresses for IP-authenticated SMTP relay.
- Do not use `smtp.office365.com` for this connector-based relay method. That hostname is used for SMTP AUTH scenarios.
- Direct Send can send to internal Microsoft 365 recipients only. Use SMTP relay when scan-to-email must send externally.
- If the customer's public IP changes, update both the Exchange connector and SPF record.

## GitHub Pages

This repo can be published with GitHub Pages:

1. Push the repository to GitHub.
2. Open repository **Settings**.
3. Go to **Pages**.
4. Set the source to the default branch and root folder.
5. Save, then open the published Pages URL.

Because the guide is a single `index.html`, GitHub Pages can serve it without any build configuration.

## Source Reference

This guide is based on Microsoft guidance for multifunction devices and applications that send email through Microsoft 365:

<https://learn.microsoft.com/en-us/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365>

Always check current Microsoft documentation and customer change-control requirements before making production mail-flow or DNS changes.

## Repository Contents

```text
.
├── index.html
└── README.md
```

## License

No license has been selected yet. Add a `LICENSE` file before publishing if you want to define reuse terms.
