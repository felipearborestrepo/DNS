# 🌐 DNS (Domain Name System) - Explained Simply

DNS is what lets us use names like `tryhackme.com` instead of hard-to-remember IP addresses like `93.184.215.14`. It works silently in the background every time you open a website, join a game server, or send an email.

---

## ☕ Real-World Analogy
Think of DNS like your phone's contact list:
- You search for "Mom," and it dials her number.
- You search for "google.com," and DNS finds the right IP address to connect to.

---

## 🔍 What is DNS?
**DNS (Domain Name System)** is a global system that:
- Translates **domain names** into **IP addresses**
- Makes it easier for people to navigate the internet
- Works automatically without users needing to understand IPs

DNS operates on **Application Layer (Layer 7)** of the OSI Model.
- **UDP Port 53** is used by default
- **TCP Port 53** is used as backup (especially when responses are large)

---

## 🧱 Structure of a Domain Name

![Screenshot 2025-06-23 105905](https://github.com/user-attachments/assets/7d90a26e-8fa2-41ad-923d-46f84ee5d19f)


| Term                 | Description |
|----------------------|-------------|
| **TLD (Top-Level Domain)** | The last part of the domain like `.com`, `.org`, `.uk`.<br>Examples: `.com` (generic), `.ca` (Canada), `.edu` (education) |
| **Second-Level Domain** | The main domain name (e.g., `tryhackme` in `tryhackme.com`) |
| **Subdomain** | A section before the main domain. Example: `admin.tryhackme.com` |

🔹 You can use multiple subdomains like: `vpn.internal.network.example.com`
🔹 Total domain length must be **253 characters or less**.

---

## 📂 Common DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Maps a domain to an **IPv4 address** | `example.com → 93.184.215.14` |
| **AAAA** | Maps to an **IPv6 address** | `example.com → 2606:...` |
| **CNAME** | Alias for another domain | `store.tryhackme.com → shops.shopify.com` |
| **MX** | Mail Exchange, used for email routing | `alt1.aspmx.l.google.com` (with priority) |
| **TXT** | Stores plain text for security, verification, SPF, etc. | `v=spf1 include:_spf.google.com ~all` |

---

## 🧭 How DNS Resolution Works (Step-by-Step)

When you type a domain like `www.tryhackme.com` into your browser, DNS works through these steps to find the correct IP:

![Screenshot 2025-06-23 111501](https://github.com/user-attachments/assets/bf6769f6-2b7a-4224-8e32-1fa4174cd2fa)

### 🔢 Step 1: Local Cache Check
Your computer checks if it recently looked up the domain.
- ✅ Found → Use cached IP.
- ❌ Not found → Ask a DNS server.

### 🔁 Step 2: Ask the Recursive DNS Server
- Usually provided by your **ISP**, or you can use public ones (Google, Cloudflare).
- Checks its own cache for the answer.
- If found → Returns IP.
- If not → Continues the search.

### 🌍 Step 3: Contact Root DNS Servers
- These are the **starting point** of DNS lookups.
- They don't know the final IP address, but they know which **TLD server** (.com, .org, etc.) to ask.
- For `www.tryhackme.com`, the root server replies: 
> "Ask the `.com` TLD server."

### 🏷️ Step 4: Ask the TLD Server (e.g., .com)
- The TLD server knows which **nameserver** holds the records for `tryhackme.com`.
- Example reply: 
> "Go to `kip.ns.cloudflare.com` or `uma.ns.cloudflare.com`."

### 🧾 Step 5: Ask the Authoritative DNS Server
- The **authoritative nameserver** has the actual DNS records.
- It responds with the correct **IP address**, MX record, etc.

### 🔁 Return and Cache
- The recursive DNS server caches the answer using the **TTL** (Time To Live) value.
- Then it sends the IP back to **your computer** so the site can load.


## 🧪 DNS in Action (Query & Response)

You can use the command line to see DNS in action:

### 🔍 `nslookup` Example:
```bash
nslookup www.example.com
```
Expected output:

Server:         127.0.0.53
Address:        127.0.0.53#53

```bash
Non-authoritative answer:
Name:   www.example.com
Address: 93.184.215.14
Name:   www.example.com
Address: 2606:2800:21f:...etc
```
This shows:

- An IPv4 address (A record)

- An IPv6 address (AAAA record)

- The DNS server here is your local system (127.0.0.53 is localhost).
