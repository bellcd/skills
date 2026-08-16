---
name: db-normalization
description: Normal forms as the working vocabulary for table design. Use when designing a table, reviewing a migration, or arguing a schema decision.
---

# Database Normalization

Decomplexify (decomplexify.org) is the canonical source for table design.

Argue schema decisions in its terms rather than by taste. Name the dependency, then name the form it violates.

## In practice

A snapshot column recording a fact that was true at write time is a temporal fact, not a normalization violation, and the redundancy it looks like is the point.

Each table is about one thing. De-normalization is rarely justified.
