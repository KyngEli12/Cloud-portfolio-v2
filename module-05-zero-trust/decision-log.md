# Module 05 — Zero Trust

## What We Built

### Component 1: Zero Trust Microservice Mesh (VPC Lattice)
- Created VPC Lattice service network `eli-zero-trust-mesh` with AWS IAM auth
- Created Lattice service `eli-zero-trust-svc` with AWS IAM auth
- Associated service with service network
- Verified in Console: service network enforces identity-based access between services

### Component 2: Verified Access for Internal Tooling
- Created Verified Access instance `eli-verified-access`
- Created trust provider `eli-trust-provider` (User trust, IAM Identity Center)
- Verified in Console: trust provider linked to instance

## Key Concepts
- **VPC Lattice**: service-to-service networking with built-in auth — no implicit trust inside the VPC
- **Auth policies**: only requests with verified IAM identity are allowed through
- **Verified Access**: VPN-less access to internal apps — checks identity and device trust on every request
- **Zero Trust principle**: never trust, always verify — even traffic inside your own network

## Status: Complete
