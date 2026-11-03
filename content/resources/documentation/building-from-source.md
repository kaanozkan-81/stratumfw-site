---
title: "Building from Source"
lead: "How to deploy the Stratum FW management panel and configuration engine on a FreeBSD 14 system."
---

Stratum Gateway is currently in **Developer Preview**. A pre-built FreeBSD ISO installer is not yet available. However, developers can deploy the panel and configuration engine directly onto an existing FreeBSD 14 system.

## Prerequisites

Before you begin, ensure you have:

- A machine running **FreeBSD 14.1** or later
- Root (or `sudo`) access
- At least 2 GB of RAM and 10 GB of storage
- A basic understanding of the `pf` firewall

## Step 1: Install System Dependencies

Stratum requires Python 3.12+ and a few system tools. Log in as root and install them using `pkg`:

~~bash
pkg update
pkg install -y python312 py312-pip py312-sqlite3 git
~~

Ensure that `pf` is enabled in `/etc/rc.conf`:

~~bash
sysrc pf_enable="YES"
service pf start
~~

## Step 2: Clone the Repository

Clone the source code from our public GitHub repository:

~~bash
git clone https://github.com/kaanozkan-81/stratumfw-site.git
cd stratumfw-site
~~

*(Note: The core panel source code is currently housed in a private repository and will be merged into the public repo upon the v1.0.0 release. This guide outlines the upcoming architecture).*

## Step 3: Set Up the Python Environment

We use a standard Python virtual environment to isolate dependencies:

~~bash
python3.12 -m venv /opt/stratum/venv
source /opt/stratum/venv/bin/activate
pip install --upgrade pip
pip install fastapi uvicorn jinja2 itsdangerous psutil passlib bcrypt
~~

## Step 4: Generate Initial Configuration

Stratum relies on a declarative JSON file. Initialize the default configuration:

~~bash
mkdir -p /usr/local/etc/stratum
# This will be handled by a setup script in the future
echo '{"system": {"hostname": "stratum"}}' > /usr/local/etc/stratum/config.json
~~

## Step 5: Run the Panel

Start the FastAPI application using Uvicorn:

~~bash
cd /opt/stratum/app
uvicorn main:app --host 0.0.0.0 --port 8000
~~

You can now access the developer panel by navigating to `http://<your-ip>:8000`.

## Next Steps

Once the panel is running, you can explore the API documentation or start configuring the firewall. The **Safe-Apply** mechanism ensures that you won't lock yourself out during configuration.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Explore the API</h3>
    <p>Learn how to automate Stratum using the REST API.</p>
    <a href="/product/architecture/" class="card-link">Read Architecture →</a>
  </div>
  <div class="card">
    <h3>Join the Beta</h3>
    <p>Interested in testing? Contact us for early access.</p>
    <a href="/company/contact/" class="card-link">Contact Us →</a>
  </div>
</div>
