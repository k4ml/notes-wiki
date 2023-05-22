---
description: >-
  This is very opinionated best practices and if you're experienced Django
  developers, there are some that you might not agree with.
---

# Missing Notes for Professional Developers

## Correct way to generate migrations

* Start with new empty database.
* git checkout main
* run the initial migrations
* git checkout the-branch
* delete existing migrations if you already created before - each branch should only have one migrations
* create new migrations from your models changes

## How to test migrations?

## Always use intermediate table for M2M relationship

