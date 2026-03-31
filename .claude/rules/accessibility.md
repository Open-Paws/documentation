---
paths:
  - "**/ui/**"
  - "**/frontend/**"
  - "**/i18n/**"
  - "**/l10n/**"
---
# Accessibility Rules for Animal Advocacy Projects

Advocacy networks span borders, languages, economic conditions, and infrastructure environments. An activist coordinating a rescue in a rural area with intermittent connectivity has fundamentally different needs than a campaign organizer at a well-resourced urban nonprofit. Accessibility in advocacy software is not about compliance with standards — it is about ensuring the movement's tools work for everyone the movement serves.

## Internationalization from Day One

Design every user-facing component with internationalization from the start — never retrofit it. Advocacy networks operate across linguistic boundaries: a coalition might include Spanish-speaking organizers in the Americas, Mandarin-speaking activists in Asia, and English-speaking legal teams in Europe. Externalize all user-facing strings from the beginning. Support right-to-left text layouts. Handle pluralization rules that differ across languages. Date, time, currency, and number formatting must respect locale. Adding i18n after the fact requires touching every component — the cost grows exponentially with codebase size.

## Low-Bandwidth Optimization

Many activists operate on mobile data in regions with expensive or throttled connections. Optimize aggressively: compress all assets, lazy-load non-critical content, minimize payload sizes, implement efficient data synchronization that transfers only deltas. Set performance budgets and test against them on throttled connections. A tool that requires broadband to function excludes the activists who need it most.

## Offline-First Architecture

Design for disconnected operation as the default, not as an exception. Activists in areas with unreliable connectivity — rural investigation sites, countries with internet shutdowns, disaster response scenarios — need tools that work without a network connection. Local-first data storage with background sync when connectivity is available. Conflict resolution for data modified offline by multiple users. Queue operations during disconnection and replay them on reconnect. The application must be fully functional for core workflows without any network access.

## Low-Literacy Design Patterns

Not all advocacy participants are fluent readers. Rescue coordinators, sanctuary workers, and community organizers come from diverse educational backgrounds. Design for comprehension: use icons alongside text labels, provide visual workflows instead of text-heavy instructions, support voice input and audio output where possible, use progressive disclosure to avoid overwhelming users with information density. Test interfaces with users who have limited formal literacy.

## Mesh Networking Compatibility

In environments where centralized internet infrastructure is unavailable, compromised, or surveilled, mesh networking enables direct device-to-device communication. Design data synchronization protocols that can operate over mesh networks with high latency, low bandwidth, and intermittent peer availability. This is not a theoretical concern — activists operating in regions with government internet shutdowns depend on mesh-capable tools.

## Graceful Degradation

Every feature must have a degraded mode that functions under constrained conditions. If the encryption library fails to load, the application must refuse to transmit sensitive data rather than transmitting it in plaintext. If the media processing pipeline is unavailable, investigation footage must be stored safely for later processing rather than discarded. If the network connection is lost, the user must see clear status indicators, not silent failures. Degrade capability, never safety.

## Device Seizure Preparation — Application State

When connectivity is lost suddenly — device confiscated, signal jammed, power cut — the application must not leave sensitive data exposed. No temporary files with decrypted investigation content. No in-memory caches that persist to swap files. No crash dumps containing witness identities. No recovery modes that display previously viewed sensitive content without re-authentication. Design the application so that power loss at any moment leaves zero recoverable sensitive state on disk.

## Multi-Language Activist Networks Across Borders

Coalition tools must support simultaneous use in multiple languages within the same deployment. A shared coordination platform where each user sees the interface in their language, but shared content (campaign plans, action alerts, investigation summaries) can be viewed in translated or original form. Support both machine translation for real-time use and human-reviewed translation for legally sensitive content.
