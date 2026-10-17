# Module 05 · Networking Fundamentals 🌐

> **Phase 1 — Core Foundations** · *Estimated time: ~1 week*
> **Prerequisites:** Module 04. (Your `requests`/`curl` work already brushed up against this.)
> **The vibe:** We learn networking by **following one real request** through every layer — and by learning to **debug** it, because half of all ops incidents are network problems in disguise.

---

## 🎬 The mission: follow a request

> You type `https://github.com` and press Enter. In **under a second**, a dozen networking concepts fire in sequence: a DNS lookup, a TCP handshake, a TLS handshake, an HTTP request, maybe a load balancer and reverse proxy in the middle. By the end of this module you'll know **every step** — and how to fix it when it breaks.

There's an old joke in operations: *"It's always DNS."* It's a joke because it's *true* — so much of what looks like "the app is down" is really a networking problem. Knowing this layer is what lets you debug confidently instead of guessing. It's also the foundation that makes **AWS VPCs (Module 07)** and **Kubernetes networking (Module 11)** suddenly make sense.

Here's our whole journey at a glance — we'll unpack each piece:
```
github.com  ──①DNS──►  140.82.x.x  ──②TCP──►  ──③TLS──►  ──④HTTP──►  [LB → reverse proxy → app]
            name→IP    the address   handshake  encrypt    request      ⑤traffic directors
```

---

## 1. The map: OSI & TCP/IP models 🗺️

Networking is built in **layers** — each one does its job and hands off to the next. Why care? Because troubleshooting is really *"which layer is broken?"*

**The OSI model (7 layers)** — the classic reference:

| # | Layer | What it does | Example |
|---|---|---|---|
| 7 | Application | What apps speak | HTTP, DNS, SSH |
| 6 | Presentation | Formatting/encryption | TLS |
| 5 | Session | Connections between apps | — |
| 4 | **Transport** | Reliable delivery, ports | **TCP, UDP** |
| 3 | **Network** | Addressing & routing | **IP** |
| 2 | Data Link | Local links | Ethernet, MAC |
| 1 | Physical | Cables, signals | fiber, wifi |

*Mnemonic (top→bottom): "All People Seem To Need Data Processing."*

**The TCP/IP model (4 layers)** is the practical, real-world version: **Link → Internet (IP) → Transport (TCP/UDP) → Application (HTTP/DNS)**.

> 🧠 **What matters for DevOps:** you mostly live at **Layer 3 (IP)**, **Layer 4 (TCP/UDP)**, and **Layer 7 (HTTP)**. The phrase **"L4 vs L7"** comes up *constantly* with load balancers — and now you know it points right back at this model.

---

## 2. IP addresses, subnets & CIDR 🏷️ (the addressing system)

Every device on a network has an **IP address**.
- **IPv4** — 32-bit, four numbers: `192.168.1.10`. (~4.3B addresses — we ran out, hence…)
- **IPv6** — 128-bit, huge: `2400:cb00:2048:1::c629:d7a2`. (Increasingly common; just recognize it.)

**Public vs private IPs.** Private ranges (RFC 1918) are reused inside every home/office/cloud network and **never routed on the public internet**:
```
10.0.0.0/8        (10.x.x.x)        ← huge; AWS VPCs love this
172.16.0.0/12     (172.16–31.x.x)
192.168.0.0/16    (192.168.x.x)     ← your home router
```
- **NAT (Network Address Translation)** lets many private devices share one public IP (it's why all your home gadgets reach the internet through one address).
- `127.0.0.1` = **localhost** (this machine). `0.0.0.0` = "all interfaces / everywhere."

**CIDR notation** — the `/number` you'll configure all over AWS and Kubernetes. The number = how many bits are the *network* part; the rest are for *hosts*:

| CIDR | # addresses | Typical use |
|---|---|---|
| `/32` | 1 | a single host (e.g. "allow just my IP") |
| `/28` | 16 | a tiny subnet |
| `/24` | 256 (254 usable) | a standard subnet |
| `/16` | 65,536 | a whole VPC |
| `/8` | 16,777,216 | enormous |

> 🧠 **Why DevOps cares:** you'll carve a VPC like `10.0.0.0/16` into subnets (`10.0.1.0/24` for public, `10.0.2.0/24` for private) for dev/staging/prod. CIDR math is a real interview/whiteboard topic. The tool `ipcalc` checks your work.

---

## 3. Ports & protocols: TCP vs UDP 🚪 (the doorways)

An IP address gets you to the *machine*; a **port** (0–65535) gets you to the right *application* on it. The combo `IP:port` is a **socket**.

**Well-known ports to memorize** (interviewers ask):

| Port | Service | | Port | Service |
|---|---|---|---|---|
| 22 | SSH | | 443 | HTTPS |
| 53 | DNS | | 3306 | MySQL |
| 80 | HTTP | | 5432 | PostgreSQL |
| 25/587 | SMTP (mail) | | 6379 | Redis |

**TCP vs UDP** — the two transport protocols:

| | **TCP** | **UDP** |
|---|---|---|
| Style | Connection-oriented, reliable, ordered | Connectionless, fast, "fire and forget" |
| Guarantees delivery? | Yes (retransmits) | No |
| Used for | Web, SSH, databases, **most things** | DNS, video/voice streaming, gaming |

**The TCP 3-way handshake** (a classic interview question) — how a connection opens:
```
Client ──SYN──►  Server      "let's talk?"
Client ◄SYN-ACK─ Server      "sure, you ready?"
Client ──ACK──►  Server      "yep!" → connection established
```

---

## 4. DNS — the internet's phonebook 📞 (your most common incident)

Computers route by IP, but humans use names. **DNS** translates `github.com` → `140.82.x.x`.

**The resolution journey** (step ① of our request) — when nothing is cached:
```
1. Browser cache?  →  2. OS cache + /etc/hosts?  →  3. Recursive resolver (your ISP or 8.8.8.8)
                                                        │
   4. Root server (.) ──► 5. TLD server (.com) ──► 6. Authoritative server (github.com)
                                                        │
                                              returns the IP ──► cached with a TTL
```
- The answer is **cached** for a duration set by its **TTL** (time-to-live). *This is why DNS changes "take time to propagate"* — old answers linger in caches until TTL expires.

**DNS record types you'll work with:**

| Record | Maps… | Example use |
|---|---|---|
| **A** | name → IPv4 | `app.com → 1.2.3.4` |
| **AAAA** | name → IPv6 | IPv6 address |
| **CNAME** | name → another name (alias) | `www.app.com → app.com` |
| **MX** | domain → mail server | email routing |
| **TXT** | arbitrary text | domain verification, SPF |
| **NS** | domain → its nameservers | delegation |

> 🔥 **"It's always DNS" in practice:** a service "goes down" but the server is fine — it's a wrong/expired record, a TTL still serving the old IP, or a typo in a CNAME. The `/etc/hosts` file lets you *override* DNS locally to test (e.g. point `myapp.com` at `127.0.0.1`) — a handy trick we'll use in the lab.

---

## 5. HTTP & HTTPS 🌍 (how the web actually talks)

Step ④: your browser sends an **HTTP request**, the server sends a **response**. It's a simple request/response model.

**Methods:** `GET` (read), `POST` (create), `PUT`/`PATCH` (update), `DELETE` (remove).

**Status codes — know these families cold** (you'll read them in logs all day):

| Family | Meaning | Key ones |
|---|---|---|
| **2xx** | ✅ Success | 200 OK, 201 Created |
| **3xx** | ↪️ Redirect | 301 moved, 302 found, 304 not modified |
| **4xx** | 🙋 *Client* error | 400 bad request, 401 unauthorized, 403 forbidden, 404 not found, 429 too many requests |
| **5xx** | 🔥 *Server* error | 500 internal error, 502 bad gateway, 503 unavailable, 504 gateway timeout |

> 🧠 **Interview gold — the 5xx trio:** **502 Bad Gateway** = your proxy/LB got a broken reply from the app behind it. **503 Service Unavailable** = the app is down or overloaded. **504 Gateway Timeout** = the app took too long to respond. Being able to distinguish these instantly is a real "this person has run production" signal.

**Headers** carry metadata: `Host`, `Content-Type`, `Authorization: Bearer <token>`, `User-Agent`. (You set these in `curl` and `requests`.) HTTP is **stateless** — each request stands alone; cookies/tokens carry identity between them. Modern versions: **HTTP/2** (multiplexing) and **HTTP/3** (over UDP/QUIC) are faster, but the concepts are the same.

---

## 6. TLS / SSL & HTTPS 🔒 (the lock)

**HTTPS = HTTP + TLS encryption** (step ③). TLS gives you three things:
1. **Encryption** — eavesdroppers can't read the traffic.
2. **Integrity** — it wasn't tampered with in transit.
3. **Authentication** — you're really talking to `github.com`, proven by its **certificate**.

- **Certificates & Certificate Authorities (CAs):** a trusted CA signs a site's certificate; your browser/OS trusts the CA, so it trusts the cert — a **chain of trust**.
- **Let's Encrypt** issues **free** certs and automates renewal — it's everywhere now.
- The **TLS handshake** (simplified): client hello → server sends its cert → keys are exchanged → an encrypted channel is established. *(You can watch this happen with `curl -v`.)*

> ⚠️ **A classic real-world outage:** an **expired TLS certificate.** Sites suddenly throw scary browser warnings and APIs reject connections. DevOps teams **monitor cert expiry** and automate renewal precisely to avoid this. Also note: TLS is often **terminated at the load balancer** (decrypted there), so traffic behind it can be plain HTTP inside a trusted network.

---

## 7. Load balancers, reverse proxies & API gateways 🚦 (the traffic directors)

Step ⑤ — in real systems, your request rarely hits the app server directly. Things sit in front:

- **Reverse proxy** (e.g. **Nginx**): sits *in front of your servers*, receives requests and forwards them on. It can also do **TLS termination**, caching, and compression. *(Contrast: a **forward proxy** sits in front of **clients**, e.g. a corporate web filter.)*

- **Load balancer (LB):** spreads incoming traffic across **multiple backend servers** for scale and high availability (if one server dies, traffic routes to the others). Two flavors — straight from the OSI model:
  - **L4 load balancer** — works at the transport layer (IP/port). Fast, protocol-agnostic. *(AWS: **NLB**.)*
  - **L7 load balancer** — works at the application layer (HTTP-aware). Can route by **path or hostname** (e.g. `/api` → one service, `/shop` → another). *(AWS: **ALB**.)*
  - Common algorithms: **round-robin**, **least connections**.

- **API gateway:** the front door for APIs/microservices — handles **authentication, rate limiting, and routing** to the right backend service.

A typical chain: `client → load balancer → reverse proxy → app servers → database`.

> 🧠 **Why DevOps cares deeply:** this is *how you scale and stay available*. "How would you handle 10× traffic?" → add servers behind a load balancer. Knowing **L4 vs L7** and where **TLS terminates** are bread-and-butter design questions.

---

## 8. Firewalls & security groups 🛡️ (the bouncers)

A **firewall** allows or denies traffic based on rules (source IP, port, protocol).
- **Linux:** `ufw` (simple) or `iptables`/`nftables` (powerful).
- **AWS (preview, Module 07):** **Security Groups** (stateful, attached to instances) and **Network ACLs** (stateless, at the subnet level).

> 🧠 **The principle that matters: least privilege.** Open only the ports you actually need. The textbook setup: allow **443 (HTTPS) from anywhere**, but **22 (SSH) only from your own IP** — never SSH open to the whole world. Misconfigured security rules are one of the most common (and dangerous) cloud mistakes.

---

## 9. The DevOps networking toolkit 🧰 (hands-on — run these now)

These are the commands you'll *actually* use to debug. Try each (mostly work on Linux/WSL/macOS; `ss` is Linux — use `netstat`/`lsof` elsewhere):
```bash
ping github.com                 # is the host reachable? (Layer 3 / ICMP)
traceroute github.com           # what path do packets take? (macOS: traceroute, Win: tracert)

dig github.com                  # full DNS lookup
dig +short github.com           # just the IP(s)
dig github.com MX               # mail records
dig @8.8.8.8 github.com         # ask a specific resolver (Google's)
nslookup github.com             # older DNS tool, similar idea

curl -v https://github.com      # 🔥 verbose: watch DNS→TCP→TLS→HTTP happen live
curl -I https://github.com      # headers only (HEAD request) — see status + server
curl -s -o /dev/null -w "%{http_code}\n" https://github.com   # print JUST the status code
curl -L https://bit.ly/xyz      # follow redirects (3xx)

ss -tulpn                       # what's LISTENING on this machine? (ports + PIDs)
nc -zv github.com 443           # is port 443 open/reachable? (netcat)
ip addr                         # this machine's IP addresses
ip route                        # the routing table (where does traffic go?)
```

> 🔥 **The "I can't reach the service" playbook — debug up the layers:**
> 1. `ping` it → is the host even up? (L3)
> 2. `nc -zv host port` → is the *port* open? (L4 — firewall/security group?)
> 3. `curl -v` → does HTTP respond, and with what status? (L7)
> 4. `dig` → is DNS resolving to the *right* IP? (the usual culprit 😉)
>
> Walking these in order turns "it's broken, idk" into a precise diagnosis. **This is exactly how senior engineers think on-call.**

---

## 🔁 Bringing it home: the full request, annotated

Now the opening trace makes complete sense:
1. **DNS** — `github.com` resolves to an IP (browser/OS cache → recursive resolver → root → TLD → authoritative).
2. **TCP** — a 3-way handshake (SYN / SYN-ACK / ACK) opens a connection to that IP on **port 443**.
3. **TLS** — handshake verifies GitHub's certificate and encrypts the channel.
4. **HTTP** — your browser sends `GET /`; the server replies `200 OK` with the page.
5. **Traffic directors** — along the way a **load balancer** (L7/ALB) and **reverse proxy** routed you to a healthy app server, possibly terminating TLS on the way.

That's the internet, demystified. 🎉

---

## 🎥 Reference videos

1. **Networking Concepts Every DevOps Engineer Must Know**
   <https://www.youtube.com/watch?v=w0SQGCt-6Ro>
   *Current (2026), DevOps-targeted — covers exactly this module's scope. Start here.*

2. **DNS Explained in 100 Seconds** — *Fireship*
   <https://www.youtube.com/watch?v=UVR9lhUGAyU>
   *Fast and fun — the perfect quick mental model for "it's always DNS."*

3. **Cloud & DevOps Networking Fundamentals — Crash Course**
   <https://www.youtube.com/watch?v=Lw8QFsYCexc>
   *A deeper walkthrough tying networking to cloud/DevOps contexts.*

**Authoritative written references** (excellent, free): Cloudflare Learning Center — <https://www.cloudflare.com/learning/dns/what-is-dns/> (also has superb pages on TLS, load balancing, and CDNs).

---

## 🛠️ Mini-project: "Network Detective" 🔍

Investigate a real domain end-to-end and write up your findings in `module-05/findings.md`:

1. **Trace a domain:** run `dig +short`, `dig <domain> MX`, and note the **A record** and its **TTL**. Then `traceroute` it and count the hops.
2. **Watch a request unfold:** run `curl -v https://github.com` and identify in the output where **DNS**, **TCP**, **TLS**, and **HTTP** each happen. Paste 2–3 interesting lines and label them.
3. **Status code safari:** use the one-line `curl ... -w "%{http_code}"` trick to capture the status of a normal page, then hit something that 404s and something that redirects (`-L` to follow). Record what you saw.
4. **Local listening ports:** run `ss -tulpn` (or `netstat`/`lsof`), list 3 listening ports, and identify what service each is.
5. **Port probe:** use `nc -zv` to test whether port `443` is open on a public site, and whether port `9999` is (it shouldn't be).
6. **DNS override trick:** add a line to `/etc/hosts` mapping a fake name (e.g. `mytest.local`) to `127.0.0.1`, `ping mytest.local` to prove it resolves locally, then remove the line. (Explain *why* this worked.)
7. **CIDR math:** answer in your notes — how many usable hosts in `10.0.0.0/24`? In `/16`? Which of `10.x`, `172.16–31.x`, `192.168.x` are private ranges?

> ✅ **Done = you can read a request's whole journey and debug connectivity across the layers.** That's a skill that makes you genuinely useful in your first week on-call.

---

## ✅ Self-check (interview-style)

1. *"What happens when you type a URL and press Enter?"* → DNS resolves the name → TCP handshake to the IP:443 → TLS handshake → HTTP request → response (often via a load balancer/reverse proxy). *(A famous interview question — now you own it.)*
2. *"TCP vs UDP?"* → TCP = reliable, ordered, connection-based (web, SSH, DBs); UDP = fast, connectionless, no guarantees (DNS, streaming).
3. *"Explain the TCP 3-way handshake."* → SYN → SYN-ACK → ACK establishes the connection.
4. *"Difference between an A record and a CNAME?"* → A maps a name to an IP; CNAME maps a name to another name (alias).
5. *"502 vs 503 vs 504?"* → 502 bad gateway (broken upstream reply), 503 unavailable (app down/overloaded), 504 gateway timeout (upstream too slow).
6. *"L4 vs L7 load balancer?"* → L4 routes by IP/port (fast, protocol-agnostic); L7 is HTTP-aware and can route by path/host.
7. *"Name the private IP ranges."* → 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16.
8. *"A service is unreachable — how do you debug it?"* → work up the layers: ping (host up?), nc (port open/firewall?), curl (HTTP/status?), dig (DNS correct?).
9. *"What does a `/24` give you?"* → 256 addresses (254 usable hosts).

---

## 📌 Key takeaways (TL;DR)

- Networking is **layered**; troubleshooting = "which layer broke?" DevOps lives at **L3 (IP), L4 (TCP/UDP), L7 (HTTP)**.
- **IP + port = socket.** Know the private ranges and **CIDR** (you'll subnet VPCs with it).
- **TCP** = reliable (3-way handshake); **UDP** = fast/connectionless.
- **DNS** maps names→IPs via resolver→root→TLD→authoritative, cached by **TTL**. *It's always DNS.*
- **HTTP status families** (2/3/4/5xx); the **502/503/504** distinction signals real experience. **HTTPS = HTTP + TLS** (watch out for expired certs).
- **Reverse proxy** fronts servers; **load balancers** scale them (**L4 vs L7**); **API gateways** front APIs.
- **Firewalls/security groups** = least privilege (443 open, 22 locked to your IP).
- Master the toolkit: **`dig`, `curl -v`, `ss`, `nc`, `ping`, `traceroute`, `ip`** — and the up-the-layers debug flow.

---

## ➡️ What's next: Module 06 — Git & GitHub

That wraps the **foundations toolkit** (Linux, Bash, Python, Networking). Module 06 covers **Git & GitHub** — version control done properly: branching, merges vs rebases, pull requests, and the collaboration workflows every team uses. It's the last foundation before **Phase 2**, where all this networking gets *real* as you build actual infrastructure in **AWS** (VPCs, subnets, security groups — you'll recognize every term now).

*Do the "Network Detective" investigation (it's genuinely satisfying to watch `curl -v` reveal the whole handshake), then say **"next"** and I'll build Module 06.*
