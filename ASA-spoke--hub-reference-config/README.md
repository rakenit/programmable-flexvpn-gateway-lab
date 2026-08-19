# ASA Spoke Reference Snippets

This folder contains sanitized IOS XE snippets for ASA-style spoke onboarding
with IKEv2, dynamic VTI, local AAA authorization lists, and BGP route exchange.

These examples are retained as reference material from the proof-of-concept lab
asset. They are not deployment-ready configs and include placeholders for all
secrets and environment-specific values.

## Role In The Proof Of Concept

The snippets show how firewall-oriented spokes can fit beside router spokes in
the programmable VPN gateway model. They provide a comparison point for IKEv2,
dynamic VTI, AAA authorization, and BGP attachment behavior.

## Review Flow

Start with the hub-facing IKEv2 and VTI parameters, then compare the local AAA
authorization lists and BGP neighbor settings against the router-spoke examples
in `../LabConfig/`.

## Sanitization And Replacement Notes

The snippets remove reusable secrets and environment-specific values. Before
private lab use, replace all PSKs, peer identities, local usernames, interface
names, BGP passwords, route targets, and topology-specific addressing.
