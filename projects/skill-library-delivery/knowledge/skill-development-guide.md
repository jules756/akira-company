# Skill Development Guide

This document defines how we build reusable agent capabilities at Akira.

## Philosophy

We build **custom agent capabilities** only.

- No external workflow tools
- Every skill is a purpose-built agent capability
- Capabilities must be reusable across clients with per-client configuration
- Capabilities follow a consistent architecture and quality standard
- We use a standardized SKILL.md format and progressive disclosure pattern for documentation

Our goal is to create a compounding library of high-quality, maintainable agent capabilities that give us a real competitive advantage in hospitality.

## What is a Skill?

A capability is a self-contained agent capability that:
- Solves one clear problem for hospitality businesses
- Can be configured per client
- Has well-defined inputs and outputs
- Follows our quality standards
- Uses our standardized SKILL.md format and progressive disclosure pattern

## The Skill Building Process

### 1. Capability Request
A need is identified by Product Manager, Customer Success, or a client.

### 2. Capability Definition
Clearly define:
- The exact problem the skill solves
- Input formats and sources
- Expected outputs and behavior
- Edge cases and failure modes
- Configuration parameters needed per client

### 3. Architecture & Design
- Design the agent architecture following our established patterns
- Define the prompt strategy
- Plan integration points
- Design error handling and escalation paths

### 4. Implementation
- Create a `SKILL.md` following our standardized format
- Implement the skill logic
- Add proper configuration, error handling, and logging
- Ensure the skill is fully configurable per client

### 5. Testing & Verification
- Test core functionality
- Test edge cases and failure modes
- Verify with real hospitality scenarios
- Ensure quality meets our standards

### 6. Documentation
- Complete `SKILL.md` with clear usage instructions
- Document configuration options
- Note known limitations
- Provide deployment and support notes

### 7. Deployment Process
- Code review
- QA verification
- Beta deployment to pilot client(s)
- Monitoring and feedback collection
- Promotion to production when stable

## Quality Standards

Every capability must:
- Follow our standardized SKILL.md format
- Use progressive disclosure to minimize token usage
- Be fully configurable per client
- Have clear error handling and escalation paths
- Be maintainable and well-documented
- Contain no hardcoded client-specific values

---

This file serves as the **methodology and standards** for building capabilities at Akira.

The actual inventory of built skills is maintained in a separate document.