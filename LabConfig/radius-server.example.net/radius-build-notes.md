# FreeRADIUS Build Notes

These notes preserve the RADIUS policy-server workflow used by the original
proof of concept. They are provided as public reference guidance, not as a
hardened production procedure.

## Reference Lab Baseline

- FreeRADIUS 3.2.3
- Ubuntu Server 22.04.3
- Cisco IOS XE hubs using RADIUS for IKEv2 PSK lookup, user authorization, group authorization, and accounting

## Setup Flow

1. Update the server and install FreeRADIUS 3.2.3 using the Network RADIUS packages for Ubuntu Jammy.
2. Verify the service and version:

   ```sh
   freeradius -v
   ```

3. Replace the default sample client secret with `<REPLACE_WITH_RADIUS_SHARED_SECRET>`.
4. Add `hub-01` and `hub-02` as RADIUS clients in `clients.conf`. See `rad-01.clients.conf`.
5. Replace the default sample users with the IKEv2 identity and group policy entries in `rad-01.users`.
6. Restart the service, or stop it and run debug mode while testing:

   ```sh
   freeradius -X
   ```

## RADIUS Request Pattern

Each hub can send three RADIUS authorization requests while building an IKEv2 tunnel:

1. PSK lookup: the username is the remote IKEv2 ID and the password is the value
   configured in `keyring aaa <password>`.
2. User-specific attributes: the username is the remote IKEv2 ID unless
   explicitly configured otherwise.
3. Group-specific attributes: the username and password are manually configured
   with the `aaa authorization group psk list ... password ...` command.

The first two requests commonly match the same username and `NAS-Identifier`.
The group request returns shared attributes for all clients associated with the
IKEv2 profile.

Accounting start messages are sent when a tunnel comes up, stop messages are
sent when it goes down, and updates are sent when state changes. FreeRADIUS
accounting records are typically written under `/var/log/freeradius/radacct/`.

## Dynamic Authorization Notes

The hub examples include `aaa server radius dynamic-author` so a controller or
operator workflow can use RADIUS dynamic authorization as part of the proof of concept.
Use this distinction when presenting the lab:

- Refresh supported tunnel policy in the backing policy source without dropping
  the active tunnel when the platform can apply that change in place.
- Send a RADIUS CoA or Disconnect workflow when a tunnel must be intentionally
  reset, disconnected, reauthorized, or rebuilt so updated policy is fully
  applied.

Exact CoA and Disconnect behavior is platform- and release-specific, so validate
the workflow against the IOS XE version used for the private lab.

## References

- https://networkradius.com/packages/#fr32-ubuntu-jammy
- https://networkradius.com/doc/current/
- https://wiki.freeradius.org/guide/Basic-configuration-HOWTO
- https://wiki.freeradius.org/config/Configuration-files
- https://wiki.freeradius.org/protocol/Disconnect-Messages
- https://www.freeradius.org/documentation/freeradius-server/4.0.0/howto/protocols/radius/coa_examples.html
- https://www.cisco.com/c/en/us/support/docs/security-vpn/remote-authentication-dial-user-service-radius/116291-configure-freeradius-00.html
