# Nginx Configuration

**V1 status:** Deployed and verified on `vm-togetherly-v1`. The VM was user-confirmed deallocated on 9 November 2026, so Nginx is not currently expected to be running.

During V1, Nginx was configured to:

- Serve the React/Vite production build from `/opt/togetherly/togetherly-workforce/dist`.
- Use the SPA fallback to `index.html`.
- Cache static assets under `/assets/`.
- Terminate HTTPS using the Let's Encrypt certificate for `togetherly-v1-faizan.centralindia.cloudapp.azure.com`.
- Redirect HTTP traffic to HTTPS.
- Proxy `/api/tinyfish/*` requests to the local Node.js API at `http://127.0.0.1:3001`.
- Forward the Authorization header and standard forwarded request headers.

The API was bound to loopback. Port 3001 must not be exposed through the NSG. The active configuration was maintained on the VM at `/etc/nginx/sites-enabled/default`. Certbot managed the TLS-related server directives.

If the VM is intentionally restarted for maintenance, inspect the live configuration before changing it. Validate with:

```bash
sudo nginx -t
sudo systemctl status nginx --no-pager
curl -I https://togetherly-v1-faizan.centralindia.cloudapp.azure.com/
```

Never commit private keys, certificate private material, secrets, or unsanitised configuration output.
