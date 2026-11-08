# Nginx Configuration

**V1 status:** Deployed on `vm-togetherly-v1`.

Nginx is configured to:

- Serve the React/Vite production build from `/opt/togetherly/togetherly-workforce/dist`.
- Use the SPA fallback to `index.html`.
- Cache static assets under `/assets/`.
- Terminate HTTPS using the Let's Encrypt certificate for `togetherly-v1-faizan.centralindia.cloudapp.azure.com`.
- Redirect HTTP traffic to HTTPS.
- Proxy `/api/tinyfish/*` requests to the local Node.js API at `http://127.0.0.1:3001`.
- Forward the Authorization header and standard forwarded request headers.

The API must remain bound to loopback. Do not expose port 3001 through the NSG. The active configuration is maintained on the VM at `/etc/nginx/sites-enabled/default`; inspect the live file before changing it. Certbot manages the TLS-related server directives, so avoid overwriting those sections manually.

Validation commands:

```bash
sudo nginx -t
sudo systemctl status nginx --no-pager
curl -I https://togetherly-v1-faizan.centralindia.cloudapp.azure.com/
```

Never commit private keys, certificate private material, secrets, or unsanitised configuration output.
