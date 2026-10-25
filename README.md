# AI Carrier Operations Case Study

Applied AI deployment for carrier operations integrating conversational AI, identity verification, business rules, external data and a legacy transportation management system.

## The problem

The objective was not to build a voice demo.

The goal was to create an end to end operational workflow capable of:

- Identifying and authenticating a carrier
- Verifying carrier information
- Retrieving external operational data
- Applying deterministic business and negotiation rules
- Managing an AI driven conversation
- Writing a booking into a legacy TMS
- Handling failures and ambiguous system states safely

## High level architecture

**Voice AI Agent**  
↓  
**Identity Verification**  
↓  
**OTP Authentication**  
↓  
**Carrier / FMCSA Data**  
↓  
**Business Rules**  
↓  
**Negotiation Controls**  
↓  
**Integration Layer**  
↓  
**Legacy Raw TCP TMS**

The integration layer also connects to monitoring and regression testing to validate system behavior across the workflow.

## Key engineering decisions

### 1. Separate probabilistic AI from deterministic operations

The conversational layer can be flexible.

Authentication, pricing rules, negotiation limits and booking actions cannot.

Critical decisions were therefore moved into deterministic controls rather than relying on the language model to make operational decisions.

### 2. OTP verification redesign

The original verification flow introduced unnecessary friction through voice recognition and an external delivery path.

Instead of continuing to patch the initial design, the workflow was redesigned around native email delivery, deterministic email normalization and fail closed OTP verification.

This reduced the number of fragile components in the authentication path and created a more reliable end to end verification experience.

### 3. Legacy TMS integration

The transportation management system communicated through raw TCP and could return:

- Partial responses
- Concatenated responses
- Malformed responses
- Ambiguous states

Read operations could sometimes be retried safely.

Booking writes required different handling because a failed response did not necessarily mean the write itself had failed.

Blind retries could therefore create duplicate operational actions.

The integration distinguished between safe retry behavior and potentially duplicative writes.

### 4. Controlled negotiation

The AI agent was not allowed to invent pricing or negotiate without limits.

Negotiation behavior was constrained by deterministic business rules and explicit operating boundaries.

This separated conversational flexibility from operational authority.

### 5. Regression testing

The workflow was validated through 32 regression tests covering core authentication, integration and operational behavior.

The objective was to make sure that changes to one part of the system did not silently break another part of the workflow.

## What I owned

I designed the deployment architecture and integration approach across:

- Authentication and OTP verification
- Business rules
- Negotiation controls
- External data integration
- Legacy TMS connectivity
- Failure handling
- Monitoring
- Regression testing
- End to end validation

## Core principle

> AI can be probabilistic. Critical operational actions cannot be ambiguous.

## What this project demonstrates

This project was designed as an Applied AI deployment problem rather than a standalone model experiment.

The core challenge was connecting probabilistic AI behavior to deterministic operational systems while maintaining reliability, traceability and control.

It required combining AI orchestration, integration design, legacy system connectivity, operational rules, failure handling and testing into one end to end workflow.

## Public case study

Read the full deployment story here:

https://www.linkedin.com/pulse/from-operations-ai-deployment-building-end-to-end-morales-begu%C3%A9-mba-np5he

## Repository scope

This repository is a public technical case study.

Production source code, proprietary platform components, credentials, private endpoints and sensitive configuration are intentionally excluded.

The purpose of this repository is to document the architecture, engineering decisions and deployment approach behind the system.

## Builder

**Fernando Begué**

Applied AI  
AI Deployment  
Forward Deployed Engineering  
Deployment Strategy

LinkedIn:

https://www.linkedin.com/in/fernando-begue
