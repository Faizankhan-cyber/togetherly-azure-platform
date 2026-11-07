# ADR-001: V1 Hosting Architecture

**Status:** Proposed, not approved for deployment  
**Date:** 2026-11-08

## Context

Togetherly Workforce uses a React/Vite frontend and Vercel-style serverless TinyFish API handlers. It does not provide a conventional Express API server. The platform needs an Azure hosting design that preserves the existing authentication and authorization behaviour.

## Proposed decision

- Serve the built frontend from an Azure Linux VM running Nginx.
- Reverse-proxy the required `/api/tinyfish/*` paths from Nginx to an Azure Function App over HTTPS.
- Adapt the existing API handlers to the Azure Functions Node.js v4 programming model.
- Keep Supabase and TinyFish as external HTTPS services.
- Keep `TINYFISH_API_KEY` server-side.

## Consequences and validation required

This design is a proposal, not a deployment decision. Before approval, validate Functions runtime compatibility, hosting plan, request duration, storage dependency, regional availability, DNS/TLS, networking, security, and total cost. Preserve session validation, employee/role checks, URL validation, HTTP methods, status codes, and response semantics.

See [V1 architecture](../architecture/v1-architecture.md). No Azure resources are authorised by this ADR.
