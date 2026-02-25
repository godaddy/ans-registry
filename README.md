# Agent Name Service (ANS) Registry

A production-ready registry system for secure AI agent discovery and identity verification. The ANS Registry enables autonomous agents to find and trust each other across organizational boundaries without requiring bilateral agreements.

## Status

The registry is operational with REST APIs for agent discovery and search.

> **Repository Intent**: This repository follows a design-first approach with OpenAPI specification alignment. The architecture and API contracts are defined in the design documentation, ensuring implementation consistency and enabling API-first development practices.

## API Reference

The ANS Registry REST API is documented using the OpenAPI (Swagger) specification:
* 📄 [View OpenAPI Spec - Human Readable](https://developer.godaddy.com/doc/endpoint/ans)
* 📄 [OpenAPI Spec - AI/Machine Readable](https://developer.godaddy.com/swagger/swagger_ans.json)

## Overview

The ANS Registry provides cryptographic identity and trust infrastructure for AI agents. Every agent identity is anchored to a verifiable Fully Qualified Domain Name (FQDN), creating a permanent, discoverable address that remains stable while agent software versions evolve.

## Core Design Principles

1. **FQDN-Anchored Identity**: Agent identities are cryptographically bound to globally resolvable domain names, providing a verifiable trust anchor.
2. **Dual Certificate Model**:
   - **Public Server Certificate**: Long-lasting certificate for the stable FQDN (time-based lifecycle)
   - **Private Identity Certificate**: Event-driven certificate for specific versioned agents (immutability-enforced)
3. **Strict Immutability**: Any code or metadata change requires a new version and registration, ensuring accountability per software version.
4. **Transparency Log**: Immutable, append-only ledger using Merkle trees, providing cryptographic proof of all registration history.
5. **Decentralized Discovery**: Registration Authority publishes lifecycle events to pub/sub; third-party services build competitive discovery indexes.

## Key Features

- **PKI-Based Trust**: Certificate Authority and Registration Authority issue X.509 certificates for agent authentication
- **Version-Centric Lifecycle**: Event-driven registration where each software version gets its own immutable identity
- **Protocol-Agnostic**: Supports A2A, MCP, and ACP protocols through a unified registration model
- **Domain Control Validation**: ACME DNS-01 challenge verifies agent ownership before registration
- **Cryptographic Verification**: Merkle inclusion proofs enable independent verification of agent registrations

## Architecture

The system consists of three main components:

- **Registration Authority (RA)**: Orchestrates validation, certificate issuance, and log sealing
- **Transparency Log (TL)**: Immutable, cryptographically verifiable ledger of all agent lifecycle events
- **Key Management System (KMS)**: Centralized root of trust for signing Merkle tree roots

## Documentation

- **[DESIGN.md](DESIGN.md)**: Complete architecture and design documentation

## Design Goals

The ANS Registry addresses the O(n²) scaling problem of bilateral agent agreements by providing:

- **Universal Discovery**: Standard method for agents to discover each other across protocols
- **Automated Trust**: Cryptographic identity verification without manual configuration
- **Auditability**: Complete, verifiable history of all agent registrations and lifecycle events
- **Ecosystem Enablement**: Foundation for competitive marketplaces and discovery services
