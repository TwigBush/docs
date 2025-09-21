---
seo:
  title: TwigBush - GNAP Authorization Server for Modern Access Control
  description: TwigBush is an early GNAP Authorization Server written in Go. We’re building an open source community to advance secure, key-bound, just-in-time access control for humans and AI agents.

---

::u-page-hero
#title
TwigBush GNAP Authorization Server

#description
Open source implementation of the Grant Negotiation and Authorization Protocol (RFC 9635) written in Go.

Enable modern, key-bound, just-in-time access control for humans and AI agents.

#links
  :::u-button
  ---
  color: neutral
  size: xl
  to: /en/getting-started/introduction
  trailing-icon: i-lucide-arrow-right
  ---
  Get Started
  :::

  :::u-button
  ---
  color: neutral
  icon: simple-icons-github
  size: xl
  to: https://github.com/TwigBush/TwigBush
  variant: outline
  ---
  View on GitHub
  :::
::

::u-page-section
#title
Open Source GNAP Server

#features
  :::u-page-feature
  ---
  icon: i-lucide-shield-check
  ---
  #title
  [Authorization Server]{.text-primary}

  #description
  Full GNAP AS implementation with `/grant`, `/continue`, `/introspect`, and JWKS endpoints. Configurable TTL, audience constraints, and step-up authentication flows.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-key
  ---
  #title
  [Proof-of-Possession Tokens]{.text-primary}

  #description
  Support for Detached JWS, HTTP Message Signatures, DPoP, and mTLS. Short-lived, key-bound access tokens prevent replay attacks and enhance security.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-git-branch
  ---
  #title
  [Policy Engine Integration]{.text-primary}

  #description
  Built-in adapter for OpenFGA and other policy engines. Implement Zanzibar-style relationship graphs for fine-grained access control.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-server
  ---
  #title
  [Resource Server Toolkit]{.text-primary}

  #description
  Example RS implementation and importable client libraries for token introspection and JWKS fetching. Quickly secure your APIs with GNAP.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-lock
  ---
  #title
  [Security First Design]{.text-primary}

  #description
  Comprehensive audit logging, key rotation support, token revocation, and step-up authentication flows. Built for production environments.
  :::

  :::u-page-feature
  ---
  icon: i-lucide-bot
  ---
  #title
  [AI Agent Ready]{.text-primary}

  #description
  Enable secure access control for AI agents and automation. Support for machine-to-machine authentication with cryptographic key binding.
  :::
::
