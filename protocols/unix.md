# `unix`

This protocol encodes a UNIX-domain socket path to a resource. In the string
representation, the path is encoded in a way consistent with a single URI Path
segment per [RFC 3986 Section 3.3](https://datatracker.ietf.org/doc/html/rfc3986#autoid-23).

Specifically following the grammar of a single `segment-nz`. In the binary
representation, no encoding is needed as the value is length prefixed.

When comparing multiaddrs, implementations should compare their binary
representation to avoid ambiguities over which characters were escaped.

## Examples

The following is a table of examples converting some common UNIX paths to their
Multiaddr string form.

| Unix Path                   | Multiaddr string form                   |
| --------------------------- | --------------------------------------- |
| socket                      | `/unix/socket`                          |
| /run/unbound.ctl            | `/unix/%2Frun%2Funbound.ctl`            |
| /run/user/1000/gnupg/S.g... | `/unix/%2Frun%2Fuser%2F1000%2Fgnupg...` |
| real space                  | `/unix/fake%20space`                    |
| real/slash                  | `/unix/real%2Fslash`                    |
| fake%20space                | `/unix/fake%2520space`                  |
| fake%2Fslash                | `/unix/fake%252Fslash`                  |

## Usage

`/unix` would typically be found at the start of a multiaddr, however it may
appear anywhere, for example in the case where we route through some sort of
proxy server or SSH tunnel.

# `unix-abstract`

This protocol encodes a Linux UNIX-domain abstract socket address,
which are distinguished by their first byte being 0.
It is encoded the same way as `unix`;
the marker byte is not part of the path.

## Examples

In the following table, the address column follows the userspace convention
of 0 bytes in the address being rendered as an `@` for abstract addresses
for display only.

| Rendered Address                             | multiaddr string form                                              |
| -------------------------------------------- | ------------------------------------------------------------------ |
| @f87a1c847a4ecaf3/bus/systemd/bus-api-system | `/unix-abstract/f87a1c847a4ecaf3%2Fbus%2Fsystemd%2Fbus-api-system` |
| @/run/fsid.sock@@@@@@@@@@@@@@@@@@@@@@@@@@... | `/unix-abstract/%2Frun%2Ffsid.sock%00%00%00%00%00%00%00%00%00...`  |

# `stream`, `seqpacket`, `dgram`

These correspond to the *type* of UNIX-domain socket:
`SOCK_STREAM`, `SOCK_SEQPACKET`, `SOCK_DGRAM`.

Previous versions of this specification did not contain these types;
for compatibility, their absence should not indicate an error,
if a default makes sense when decoding.
