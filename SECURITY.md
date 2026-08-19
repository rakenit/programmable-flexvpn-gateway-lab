# Security Policy

## Scope

This repository contains sanitized network lab and reference configuration
examples. It is intended for education and private lab adaptation.

## Reporting Issues

If you find a credential, private key, customer identifier, proprietary artifact,
live endpoint, packet capture, or other sensitive material in this repository,
open a private security report through the repository host or contact the
maintainers through the project owner channel.

Do not disclose suspected secrets publicly until the maintainers have had a chance to rotate, remove, or verify the material.

## Public Reference Handling

- Treat every configuration file as an example template.
- Replace every placeholder before using the configs in a private lab.
- Do not reuse secrets from slides, screenshots, tickets, chat logs, or prior lab exports.
- Validate cryptographic settings against current organizational standards before production use.
- Do not publish packet captures, device logs, generated keys, `.env` files, or vendor/customer artifacts in this repo.

## Supported Use

The examples are provided as-is for learning and private lab review. They are not a supported production baseline.
