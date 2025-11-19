# Enhanced Agent Name Service (ANS) / Registration Authority (RA)

ANS/RA defines the security architecture for AI agent identity on the internet, using cryptographic primitives to address trust problems in machine-to-machine commerce. The registry enables autonomous agents to find and trust each other across organizational boundaries without requiring bilateral agreements.

> **Repository Intent**: This repository follows a design-first approach with OpenAPI specification alignment. The architecture and API contracts are defined in the design documentation, ensuring implementation consistency and enabling API-first development practices.

## The Problem

HTTPS certificates work for websites but fail for autonomous agents. Web certificates last 90 days based on domain ownership. Agent code changes daily. ANS creates event-driven certificates tied to specific software versions.

## Core Design Principles

1. **FQDN-Anchored Identity**: Agent identities are cryptographically bound to globally resolvable domain names, providing a verifiable trust anchor.
2. **Dual Certificate Model**:
   - **Public Server Certificate**: Long-lasting certificate for the stable FQDN (time-based lifecycle)
   - **Private Identity Certificate**: Event-driven certificate for specific versioned agents (immutability-enforced)
3. **Strict Immutability**: Any code or metadata change requires a new version and registration, ensuring accountability per software version.
4. **Transparency Log**: Immutable, append-only ledger using Merkle trees, providing cryptographic proof of all registration history.
5. **Decentralized Discovery**: Registration Authority publishes lifecycle events to pub/sub; third-party services build competitive discovery indexes.

## Architecture

The system consists of three main components:

- **Registration Authority (RA)**: Orchestrates validation, certificate issuance, and log sealing
- **Transparency Log (TL)**: Immutable, cryptographically verifiable ledger of all agent lifecycle events
- **Key Management System (KMS)**: Centralized root of trust for signing Merkle tree roots

The cryptographic architecture uses four key elements: Private Identity Certificates (version-specific mTLS credentials), Transparency Log (Merkle tree with cryptographic proofs), Dual Certificates (separate transport vs identity), and JWS Signatures (detached signatures for transaction authorization).

ANS operates at Layer 1 (identity verification). External services provide Layer 2 (compliance auditing) and Layer 3 (reputation monitoring). The ANS Integrity Monitor validates DNS records against the Transparency Log continuously using DNSSEC and JCS (JSON Canonicalization Scheme) for deterministic verification.

## Key Features

- **PKI-Based Trust**: Certificate Authority and Registration Authority issue X.509 certificates for agent authentication
- **Version-Centric Lifecycle**: Event-driven registration where each software version gets its own immutable identity
- **Protocol-Agnostic**: Supports A2A, MCP, and ACP protocols through a unified registration model
- **Domain Control Validation**: ACME DNS-01 challenge verifies agent ownership before registration
- **Cryptographic Verification**: Merkle inclusion proofs enable independent verification of agent registrations

## API Documentation

### Live Endpoints

**ANS Registry**: `https://ra.int.godaddy.com` (Alpha)
**Transparency Log**: `https://transparency.ans.godaddy.com` (Production)
**Project Site**: `https://www.agentnameregistry.org/` (Planned)

### Core Registry API operations

`POST /v1/agents/register` - Submit registration with Certificate Signing Requests
`POST /v1/agents/{id}/revoke` - Revoke certificates
`GET /v1/agents/{protocol}/{ansName}` - Retrieve agent details
`GET /registration/{protocol}/{ansName}` - Get transparency log proof

[OpenAPI specification](https://developer.godaddy.com/doc/endpoint/ans-registry)

## Status

**Working**: Registration, dual certificates, transparency log
**In Development**: Pub/Sub event system (Q1 2026)
**Planned**: OAuth, external API access, partner SDKs

## Design Goals

The ANS Registry addresses the O(n²) scaling problem of bilateral agent agreements by providing:

- **Universal Discovery**: Standard method for agents to discover each other across protocols
- **Automated Trust**: Cryptographic identity verification without manual configuration
- **Auditability**: Complete, verifiable history of all agent registrations and lifecycle events
- **Ecosystem Enablement**: Foundation for competitive marketplaces and discovery services

## Documentation

- **[DESIGN.md](docs/DESIGN.md)**: Complete architecture and design documentation.

## Contributing and Adoption

ANS/RA is an open standard for the agentic web ecosystem. See [CONTRIBUTING.md](CONTRIBUTING.md) for more information.