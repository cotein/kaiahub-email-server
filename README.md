# KAIAHUB Mail Server

This project sets up a mail server using the `analogic/poste.io` Docker image.

## Services

### kaiahub_mail_server

The `kaiahub_mail_server` service uses the `analogic/poste.io` Docker image to provide a full-featured mail server. It includes support for SMTP, IMAP, and POP3 protocols, as well as a web-based administration interface.

#### Environment Variables

- `TZ`: Sets the timezone for the container.
- `h`: The hostname for the mail server.
- `HTTP_PORT`: The port for HTTP access to the web interface.
- `HTTPS_PORT`: El puerto para acceso HTTPS (internamente 4443).
- `HTTPS`: Se establece en `OFF` ya que el SSL lo maneja Nginx Proxy Manager.
- `DISABLE_CLAMAV`: Desactiva el antivirus ClamAV (ahorra RAM).
- `DISABLE_RSPAMD`: Desactiva Rspamd (ahorra RAM).

#### Volumes

- `/mnt/mail:/data`: Monta el directorio del host `/mnt/mail` en `/data` del contenedor para persistencia.

#### Redes

- `npm_network`: Red externa compartida con **Nginx Proxy Manager**.

#### Puertos Estándar

- SMTP: `25`
- SMTPS: `465` (SSL/TLS)
- MSA/Submission: `587` (STARTTLS)
- IMAP: `143` (STARTTLS) / `993` (SSL/TLS)
- POP3: `110` (STARTTLS) / `995` (SSL/TLS)
- Sieve: `4190`
- Web UI (HTTP): `8080`
- Web UI (HTTPS): `4443`

### Configuración de Clientes (SMTP/IMAP)

| Servicio | Puerto | Seguridad |
| :--- | :--- | :--- |
| **SMTP (Envío)** | `587` | STARTTLS |
| **SMTP (Envío Alt)** | `465` | SSL/TLS |
| **IMAP (Recibo)** | `993` | SSL/TLS |
| **POP3 (Recibo)** | `995` | SSL/TLS |

## Usage

To start the mail server, run the following command:

```sh
docker-compose up -d
```

## Configuración en Nginx Proxy Manager

1. Crea un nuevo **Proxy Host**.
2. **Domain Names**: `mail.kaiahub.ar`
3. **Scheme**: `http`
4. **Forward Hostname / IP**: `kaiahub_email_server`
5. **Forward Port**: `8080`
6. En la pestaña **SSL**, solicita un nuevo certificado Let's Encrypt.