# Synthetic 504 fixture — Design stub

This folder is reserved for a **synthetic** Nginx Ingress HTTP 504 incident bundle. No genuine production logs or secrets are included.

Suggested evidence:
- Ingress access log with `status`, `upstream_status`, `request_time`, `upstream_connect_time`, `upstream_header_time` and `upstream_response_time`.
- EndpointSlice observation around the incident window.
- Pod readiness transition history (when available).
- Expected diagnosis and evidence coverage label.

The fixture will be implemented and validated during the first development milestone.
