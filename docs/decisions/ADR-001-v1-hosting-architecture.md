# ADR-001: Original V1 Hosting Proposal

**Status:** Superseded by the implemented VM + Nginx + Node.js Express design  
**Date:** 2026-11-08

## Context

The initial V1 design considered moving the TinyFish API handlers from the Togetherly Workforce application into an Azure-hosted serverless API while serving the frontend from an Azure Linux VM.

## Original proposal

The proposal was to:

- Serve the built frontend from an Azure Linux VM running Nginx.
- Reverse-proxy `/api/tinyfish/*` requests to an Azure Function App.
- Adapt the API handlers to the Azure Functions Node.js programming model.
- Keep Supabase and TinyFish as external HTTPS services.
- Keep `TINYFISH_API_KEY` server-side.

## What was implemented instead

Azure Functions and its storage account were not deployed. The working V1 API was deployed as a separate Node.js Express service on the same VM, bound to `127.0.0.1:3001` and managed by `tinyfish-api.service` under systemd. Nginx reverse-proxies `/api/tinyfish/*` to that local service.

This record is retained to explain why the final implementation differs from the original proposal. The [V1 architecture](../architecture/v1-architecture.md) documents the deployed design.
