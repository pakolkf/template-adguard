# AdGuard Home Configuration

Configuración personalizada de AdGuard Home enfocada en:

- Bloqueo de publicidad y trackers
- Protección contra malware, phishing y scam
- DNS seguros y privados
- Compatibilidad con Smart TV y dispositivos IoT
- Balanceo entre múltiples upstream DNS
- Logging y estadísticas habilitadas

---

# Acceso Web

| Parámetro | Valor |
|---|---|
| URL | `http://IP_DEL_SERVIDOR` |
| Puerto | `80` |
| Usuario | `admin` |
| Contraseña | `adminadmin` |

---

# Características principales

## DNS seguros

Configurado con múltiples proveedores DNS:

- Quad9
- Cloudflare Security
- AdGuard DNS

### Protocolos utilizados

- DNS-over-HTTPS (DoH)
- DNS-over-TLS (DoT)

### Modo upstream

```yaml
upstream_mode: load_balance
```

Esto reparte las consultas DNS entre varios servidores para mejorar disponibilidad y rendimiento.

---

# Upstream DNS configurados

```yaml
upstream_dns:
  - https://dns10.quad9.net/dns-query
  - https://security.cloudflare-dns.com/dns-query
  - tls://security.cloudflare-dns.com
  - https://dns.quad9.net/dns-query
  - tls://dns.quad9.net
  - https://dns.adguard.com/dns-query
  - tls://dns.adguard.com
```

---

# Listas de bloqueo incluidas

## Publicidad y trackers

- AdGuard DNS Filter
- AdAway
- OISD Blocklist Big
- Steven Black's List
- Ealenn BlockList
- 1Hosts (Lite)

## Seguridad

- Phishing URL Blocklist
- URLHaus Malware List
- Phishing Army
- Scam Blocklist by DurableNapkin
- Anti-Malware List
- Badware Risks

## Smart TV y dispositivos IoT

- Perflyst and Dandelion Sprout Smart-TV Blocklist

## Protección adicional

- NoCoin Filter List
- Anti Push Notifications
- Gambling Blocklist

---

# Configuración destacada

## Cache DNS

```yaml
cache_enabled: true
cache_size: 4194304
```

## DNS plano habilitado

```yaml
serve_plain_dns: true
```

## DNSSEC

Actualmente deshabilitado:

```yaml
enable_dnssec: false
```

## Rate limit

```yaml
ratelimit: 50
```

## Query log

```yaml
querylog:
  enabled: true
  file_enabled: true
  size_memory: 1000
```

## Estadísticas

```yaml
statistics:
  enabled: true
```

---

# Docker Compose recomendado

```yaml
services:
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    restart: unless-stopped

    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      - "443:443/tcp"
      - "3000:3000/tcp"

    volumes:
      - ./work:/opt/adguardhome/work
      - ./conf:/opt/adguardhome/conf
```

---

# Configuración recomendada del router

Configura como DNS principal la IP del servidor donde corre AdGuard Home.

Ejemplo:

```text
192.168.1.10
```

---

# Seguridad

## Recomendaciones

- Cambiar la contraseña por defecto
- Restringir acceso web desde Internet
- Habilitar HTTPS
- Usar autenticación fuerte
- Mantener filtros actualizados

---

# Backup

La configuración principal se encuentra en:

```text
/opt/adguardhome/conf/AdGuardHome.yaml
```

---

# Verificación rápida

## Comprobar resolución DNS

```bash
nslookup google.com 192.168.1.10
```

## Comprobar bloqueo

```bash
nslookup doubleclick.net 192.168.1.10
```

---

# Recursos oficiales

- https://adguard.com/adguard-home/overview.html
- https://github.com/AdguardTeam/AdGuardHome
- https://github.com/AdguardTeam/AdGuardHome/wiki
