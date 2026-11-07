# Architecture

This directory contains the current architecture diagram, deployment topology, trust boundaries, and version-by-version architecture changes.

## V1 target

The intended request path is Internet → Azure Public IP → NSG → Ubuntu VM running Nginx. Nginx serves the static Togetherly frontend and reverse-proxies the required /api/tinyfish routes to a local Node.js service. That service communicates with Supabase over HTTPS. The exact Node.js compatibility and runtime model must be verified by inspecting the existing Togetherly implementation before deployment.

Architecture diagrams must reflect the deployed and tested state. Clearly label planned components that are not yet implemented.