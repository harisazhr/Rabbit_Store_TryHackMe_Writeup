# Rabbit_Store_TryHackMe_Writeup

## Objective

Complete a full penetration test of the Rabbit Store machine by exploiting web application vulnerabilities and RabbitMQ misconfigurations to achieve root access. This challenge involves chaining multiple exploits including mass assignment, SSRF, SSTI, and RabbitMQ exploitation.

**Platform:** TryHackMe  
**Room:** Rabbit Store  
**Difficulty:** Medium  
**Type:** Boot-to-Root CTF  
**Status:** Root Access Obtained ✅

## Skills Learned

- **Mass Assignment Exploitation** - Manipulating registration parameters to bypass subscription activation
- **Server-Side Request Forgery (SSRF)** - Accessing internal API documentation endpoints
- **Server-Side Template Injection (SSTI)** - Exploiting Jinja2 templating engine for RCE
- **RabbitMQ Enumeration** - Leveraging Erlang cookie for service authentication
- **Password Hash Analysis** - Converting and extracting SHA-256 hashes from RabbitMQ password format
- **API Security Testing** - Identifying and exploiting hidden API endpoints
- **Virtual Host Discovery** - Enumerating subdomains and internal services
- **Linux Privilege Escalation** - Exploiting misconfigured file permissions

## Tools Used

- **Nmap** - Network scanning and service enumeration
- **Burp Suite** - Web application testing and request manipulation
- **cURL** - API interaction and testing
- **Netcat** - Reverse shell listener
- **rabbitmqctl** - RabbitMQ management and enumeration
- **linpeas.sh** - Linux privilege escalation enumeration
- **JWT.io** - JWT token analysis and decoding
- **base64/xxd** - Hash conversion and encoding manipulation

## Attack Chain Summary

```
1. Reconnaissance
   └─> Nmap scan reveals ports 22, 80, 4369, 25672
   └─> Virtual host discovery: cloudsite.thm, storage.cloudsite.thm

2. Initial Access
   └─> Mass Assignment vulnerability in registration
   └─> Bypass subscription activation
   └─> SSRF to access internal API documentation
   └─> SSTI in /api/fetch_messages_from_chatbot endpoint
   └─> RCE as rabbitmq user

3. Privilege Escalation
   └─> World-readable /var/lib/rabbitmq/.erlang.cookie
   └─> RabbitMQ enumeration with Erlang cookie
   └─> Extract root password hash from RabbitMQ definitions
   └─> Convert base64 hash to SHA-256
   └─> Root access obtained ✅
```

## Flags Captured

| Flag Type | Location |
|-----------|----------|
| User Flag | `/home/azrael/user.txt` |
| Root Flag | `/root/root.txt` |

## Full Writeup

For detailed step-by-step exploitation process, screenshots, and technical analysis, please refer to the complete writeup:

📄 **[Rabbit_Store_TryHackMe_Writeup.pdf](./Rabbit_Store_TryHackMe_Writeup.pdf)**

The writeup includes:
- Comprehensive reconnaissance and enumeration results
- Detailed exploitation methodology with screenshots
- Complete privilege escalation process
- RabbitMQ password hash extraction technique
- Key security takeaways and defense recommendations

## Key Vulnerabilities Exploited

| Vulnerability | Severity | Impact |
|---------------|----------|--------|
| Mass Assignment | HIGH | Unauthorized subscription activation |
| SSRF | HIGH | Access to internal API documentation |
| SSTI (Jinja2) | CRITICAL | Remote Code Execution |
| World-readable Erlang Cookie | HIGH | RabbitMQ authentication bypass |
| Weak Password Storage | MEDIUM | Password hash extraction |

## Security Lessons

- 🔐 **Whitelist Parameters** - Always validate and whitelist allowed parameters in registration/update endpoints
- 🛡️ **Secure Internal APIs** - Implement proper authentication and network segmentation for internal services
- 🚫 **Input Sanitization** - Never embed user input directly in template rendering
- 🔒 **File Permissions** - Erlang cookies and sensitive files should have restrictive permissions (600)
- 🔑 **Defense in Depth** - Multiple security layers are essential to prevent full system compromise

---

**Disclaimer:** This writeup is for educational purposes only. Always obtain proper authorization before performing security testing.

🐇 Happy Hacking!
