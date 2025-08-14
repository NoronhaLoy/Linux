This now covers everything from SSH keys → SSL/TLS → Kubernetes → Cloud → Automation → Troubleshooting in one place.
# Certificate Management Cheat Sheet (SRE / DevOps / Platform / Cloud)

This section covers everything about managing certificates in different environments.

---

## 1. Key Concepts

| Term | Description |
|------|-------------|
| **Certificate** | A digital file proving the identity of a server, service, or user. |
| **Private Key** | Secret cryptographic key used to prove identity — must be kept secure. |
| **Public Key** | Key that can be shared publicly — used for encryption/verification. |
| **CSR (Certificate Signing Request)** | A request to a Certificate Authority (CA) for a signed certificate. |
| **CA (Certificate Authority)** | An entity that issues digital certificates (e.g., DigiCert, Let’s Encrypt). |
| **Root CA** | Top-level trusted certificate — installed in systems by OS/browser vendors. |
| **Intermediate CA** | Delegated CA that issues certificates on behalf of a Root CA. |
| **Self-Signed Certificate** | Certificate signed by its own private key (not trusted by default). |
| **PKI (Public Key Infrastructure)** | Framework for managing public-key encryption and certificates. |

---

## 2. Common Certificate Types

| Type | Use Case |
|------|----------|
| **SSL/TLS Server Certificate** | Secures HTTPS websites or APIs. |
| **Client Certificate** | Used to authenticate a client to a server. |
| **Code Signing Certificate** | Verifies authenticity of software packages. |
| **Wildcard Certificate** | Covers all subdomains (`*.example.com`). |
| **SAN (Subject Alternative Name)** | Single cert for multiple domains. |
| **Self-Signed Certificate** | For internal/test use only. |
| **Kubernetes API Cert** | Secures the K8s API server and components. |

---

## 3. OpenSSL Commands

### Generate Private Key
```bash
openssl genrsa -out private.key 2048
```

### Generate CSR
```bash
openssl req -new -key private.key -out request.csr
```

### Generate Self-Signed Certificate
```bash
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
```

### View Certificate Details
```bash
openssl x509 -in cert.pem -text -noout
```

### Check CSR Details
```bash
openssl req -in request.csr -noout -text
```

### Verify Private Key Matches Certificate
```bash
openssl x509 -noout -modulus -in cert.pem | openssl md5
openssl rsa -noout -modulus -in key.pem | openssl md5
```

---

## 4. Let’s Encrypt / Certbot

**Install certbot (Debian/Ubuntu)**
```bash
sudo apt install certbot python3-certbot-nginx
```

**Generate & Install Certificate (Nginx)**
```bash
sudo certbot --nginx -d example.com -d www.example.com
```

**Renew Certificates**
```bash
sudo certbot renew --dry-run
```

---

## 5. Kubernetes Certificate Management

- Kubernetes uses certificates for:
  - API server
  - Kubelet authentication
  - etcd encryption
- Certificates are typically stored in `/etc/kubernetes/pki/`.

**View Kubernetes API Server Cert**
```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

**Renew All Kubernetes Certificates (kubeadm)**
```bash
sudo kubeadm cert renew all
```

**Check Certificate Expiry**
```bash
kubeadm certs check-expiration
```

---

## 6. Cloud Certificate Management

### AWS Certificate Manager (ACM)
- Upload/import certificate:
```bash
aws acm import-certificate \
  --certificate file://certificate.pem \
  --private-key file://privateKey.pem \
  --certificate-chain file://certificateChain.pem
```
- Request public certificate:
```bash
aws acm request-certificate \
  --domain-name example.com \
  --validation-method DNS
```

### GCP Certificate Manager
- Create managed certificate:
```bash
gcloud compute ssl-certificates create my-cert \
  --domains="example.com,www.example.com" \
  --global
```

### Azure Key Vault
- Import certificate:
```bash
az keyvault certificate import --vault-name MyVault \
  --name MyCert --file certificate.pfx
```

---

## 7. Certificate Automation

| Tool | Use Case |
|------|----------|
| **certbot** | Let’s Encrypt certificate issuance & renewal. |
| **lego** | Lightweight ACME client for automation. |
| **step-ca** | Small CA for internal PKI. |
| **CFSSL** | CloudFlare PKI toolkit for cert management. |
| **Vault PKI** | HashiCorp Vault for certificate issuance & rotation. |

---

## 8. Troubleshooting Certificates

### Test HTTPS Connection
```bash
openssl s_client -connect example.com:443
```

### Check Expiry Date
```bash
echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates
```

### Verify Certificate Chain
```bash
openssl verify -CAfile chain.pem cert.pem
```

### Curl with Custom Certificate
```bash
curl --cacert myCA.pem https://example.com
```

---

## 9. Best Practices
- Always **keep private keys secure** (restrict permissions to `600`).
- Automate renewal of short-lived certs.
- Use wildcard certs for multi-subdomain environments.
- For internal services, consider your own internal CA (Vault, step-ca).
- Always verify cert-chain integrity before deploying.
- Rotate keys periodically to limit exposure.

---
