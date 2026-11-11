# Stratum Firewall Official Website (`stratumfw.com`)

Multilingual (**English** & **Turkish**) Hugo source repository for the official **[Stratum Firewall (Stratum FW)](https://stratumfw.com/)** website.

* **Live Website (EN):** [https://stratumfw.com/](https://stratumfw.com/)
* **Live Website (TR):** [https://stratumfw.com/tr/](https://stratumfw.com/tr/)
* **Interactive Live Demo (FreeBSD 15.1 VM):** [https://demo.stratumfw.com/](https://demo.stratumfw.com/) (`demo` / `Stratum2026`)
* **Download Installers (`.iso` VGA & `.img` Serial):** [https://stratumfw.com/download/](https://stratumfw.com/download/)
* **Appliance Architecture & Issue Tracker:** [kaanozkan-81/stratum-panel](https://github.com/kaanozkan-81/stratum-panel)

## Repository Structure

| Directory / File | Description |
| :--- | :--- |
| `hugo.toml` | Multilingual (`en` & `tr`) Hugo configuration, menus, and SEO metadata |
| `content/en/` | English pages: Product, Solutions, Documentation (12 guides), Changelog (`v0.28.3`), FAQ, Download, Live Demo, and Blog |
| `content/tr/` | Turkish pages (`1:1` parity): Ürün, Çözümler, Dokümantasyon (12 rehber), Sürüm Notları (`v0.28.3`), SSS, İndir, Canlı Demo ve Blog |
| `layouts/` | Custom HTML5 templates, JSON-LD (`SoftwareApplication` & `WebApplication`) Schema, `hreflang`, and custom `robots.txt` |
| `static/` | CSS, brand SVGs, 3D product architecture visuals (`images/features/`), UI docs screenshots, and `.well-known/security.txt` |

## Building & Deploying

Requires **Hugo Extended** (`v0.154+`):

```bash
# Local development server
hugo server --buildFuture

# Production build & deploy to Nginx web root
hugo --minify --buildFuture
sudo cp -rf public/. /var/www/stratumfw-site/
sudo chown -R www-data:www-data /var/www/stratumfw-site
```

© 2026 Stratum Security · Kaan Özkan. All rights reserved.
