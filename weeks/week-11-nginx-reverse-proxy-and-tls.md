[← Week 10](week-10-deploy-a-blazor-app-with-kestrel-and-systemd.md) | [Overview](../README.md) | [Week 12 →](week-12-break-it-on-purpose.md)

# Week 11 - nginx reverse proxy and TLS

*Phase 3: Run a real server (Weeks 9-12)*

- [ ] **Done**

**Goal:** Put the app behind nginx with HTTPS.

**Exercises:**

- [ ] Install nginx; write a server block that proxies to Kestrel; test with nginx -t.
- [ ] Configure forwarded headers in ASP.NET Core and WebSocket/SignalR support for Blazor Server.
- [ ] Get a certificate with certbot (Let's Encrypt), check auto-renewal, redirect HTTP to HTTPS.
- [ ] Add basic security headers and gzip; look at the access and error logs.

**Deliverable:** Your app live on a domain with valid TLS and an A grade on an online TLS test.

**My notes:** [notes/week-11-nginx-reverse-proxy-and-tls.md](../notes/week-11-nginx-reverse-proxy-and-tls.md)

---

[← Week 10](week-10-deploy-a-blazor-app-with-kestrel-and-systemd.md) | [Overview](../README.md) | [Week 12 →](week-12-break-it-on-purpose.md)
