---
name: analyze_project
description: Detect language, framework, and port in one pass by inspecting project files. Use as the first step before generating a Dockerfile.
---

# Skill: analyze_project

## Goal
Detect language, framework, and exposed port from project files in a single pass.

## Instructions

### Language detection (check in order)
- `package.json` → language = node
- `requirements.txt` or `pyproject.toml` → language = python
- `go.mod` → language = go
- `pom.xml` or `build.gradle` → language = java
- `Cargo.toml` → language = rust
- `Gemfile` → language = ruby
- Otherwise → language = generic

### Framework detection
- **node**: read `package.json` dependencies/devDependencies for: express, fastify, next, nest, koa, hapi
- **python**: read `requirements.txt` or `pyproject.toml` for: django, flask, fastapi, tornado, aiohttp
- **go**: read `go.mod` for: gin, echo, fiber, chi
- **java**: read `pom.xml` or `build.gradle` for: spring-boot, quarkus, micronaut
- **rust**: read `Cargo.toml` for: actix-web, axum, rocket, warp
- **ruby**: read `Gemfile` for: rails, sinatra, hanami
- Otherwise → framework = generic

### Port detection (in priority order)
1. Check `.env.example` or `.env.sample` for `PORT=<number>`
2. Check `package.json` `scripts.start` for `PORT=<number>` or `--port <number>`
3. Use framework defaults if no explicit port found:
   - django / flask / fastapi / aiohttp → 8000
   - rails / sinatra → 3000
   - next → 3000
   - express / fastify / koa / nest / hapi → 3000
   - gin / echo / fiber / chi → 8080
   - actix-web / axum / rocket / warp → 8080
   - spring-boot → 8080
   - quarkus / micronaut → 8080
4. Default → 3000

## Output
language, framework, port
