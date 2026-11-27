---
title: "Firewall > Download Control"
seo_title: "06.9 Firewall > Download Control & Disguised File Inspection — Stratum FW User Manual"
description: "Checking and blocking HTTP and HTTPS file downloads per internal network: Squid web proxy on port 3128, Inspection CA (.cer / .crt), true file-type detection from first bytes, catching disguised .exe files, Monitor and Block modes, Bypass and Always allowed lists, and the Downloads audit tab."
weight: 609
chapter_num: "06.9"
url: "/manual/firewall-download-control/"
aliases:
  - "/manual/06-09-firewall-download-control/"
translationKey: "manual-06-09-firewall-download-control"
---

**Firewall > Download Control** checks the files that devices in your chosen internal networks download over HTTP and HTTPS - recording what was downloaded, catching disguised programs (such as an `.exe` renamed `.pdf` or `.jpg`), and blocking unwanted file types before they land on a user's computer.

![Firewall > Download Control > Settings](/images/manual/dlctl-settings.png)

## How it works

1. **Explicit web proxy on port 3128:** Stratum FW runs **Squid** on the firewall's own address (`<interface IP>:3128`) in the internal networks you tick. Devices use it once configured - for example across a Windows domain with an Active Directory **Group Policy** (the panel lists the exact steps). Devices that are not configured to use the proxy are not checked.
2. **HTTPS inspection with Inspection CA:** To inspect HTTPS downloads, the proxy uses its own dedicated certificate authority under [**System > Certificates > Inspection CA**](/manual/system-certificates/#inspection-ca) - separate from the **Block Page CA**. Download it as `.cer` (Windows / Group Policy) or `.crt` (PEM) and install it on the devices that use the proxy.
3. **True file type from content, not just the name:** Decisions combine three signals - the **file name**, the **MIME type** the server claims, and the **first bytes** of the file itself:
   - **Plain-text content wins over a misleading name:** When a file's name looks like a program, archive or disk image (for example `response.bin` attachments sent as plain JSON by Google's web services) but its content is plainly text, the content wins and it is not treated as a program. `.bin` alone no longer means a program; a real program is still recognised by its first bytes.
   - **Compressed transfers (`gzip`, `deflate`):** A file sent `gzip`- or `deflate`-compressed is opened far enough to see its first bytes, so a compressed program is recognised as a program too.
   - **Web page assets ignored:** Parts of normal web pages (page scripts, inline pictures, compressed web pages) do not count as downloads.
4. **Fail-open safety:** If the file checker stops for any reason, downloads go through uninterrupted while endpoint antivirus software on the PCs keeps doing its own checks.

## File types and disguised programs

| Setting | What is recognised and checked |
| :--- | :--- |
| **Disguised programs** | Recognises executable files hidden under another extension - for example a Windows `.exe` named `invoice.pdf` or `photo.jpg` - from their content bytes. Can be blocked **whatever file types are chosen below**. |
| **Programs and installers** *(default)* | Windows, Linux and macOS programs (`.exe`, `.dll`, ELF, Mach-O), `MSI`, `MSIX`, `APK` and `JAR`. |
| **Scripts and shortcuts** *(default)* | PowerShell, batch, shell, VBScript, JavaScript downloads and shortcuts. |
| **Archives** | `ZIP`, `RAR`, `7z`, `CAB` and common archive formats. |
| **Disk images** | Optical and disk image containers (`.iso`, `.img`, `.dmg`, ...). |
| **Documents** | `PDF` and Microsoft Office files. |
| **Video and audio** | Downloaded video and audio files. |

## Monitor or Block

- **Monitor:** Records every download and marks what *would* have been blocked without blocking anything. **Start with Monitor for a few days** when rolling Download Control out to a network so you can verify business tools and add any needed exceptions first.
- **Block:** Replaces a blocked download with a page in the browser explaining that the download was blocked, using the same visual design as your [**Block page**](/manual/firewall-application-control/#block-page).

## Bypass list and Always allowed

| List | Behaviour | Typical entries |
| :--- | :--- | :--- |
| **Bypass list** | Connections to these sites are **never opened** by the proxy and their files are **not checked**. | Banks, government portals (`gov.tr`), operating system updates, endpoint security / antivirus updates, and applications that accept only their own pinned certificate. A **default bypass list** can be added with **one click**. |
| **Always allowed** | Connections and downloads from these sites **are checked and recorded** in the Downloads tab, **but never blocked**. | Trusted internal software servers, partner portals or vendor download hosts where IT wants audit visibility without blocking installers or scripts. |

## Downloads tab and privacy

![Firewall > Download Control > Downloads](/images/manual/dlctl-downloads.png)

The **Downloads** tab lists recorded HTTP and HTTPS downloads newest first:

- **Columns:** **Time**, **Device**, **Site**, **File**, **Type** (including disguised program flags), **Size** and **Result** (*Allowed*, *Always allowed*, *Would block* or *Blocked*).
- **Search and filters:** Filter by result, file type, network or period, or search by file name, domain or device address.
- **Retention:** Records are kept on the firewall for **1 to 365 days** (**30 days** by default).
- **Administrator-only & audited:** Only **Administrators** can open **Firewall > Download Control**, and every view of the download records is written to [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/).
