# Security policy

Anpheros handles medical data, so we take reports about security issues seriously and are grateful for them.

## Reporting a vulnerability

Please **do not open a public issue** for a security problem. Write to **contact@anpheros.com** with the subject
line starting with `Security:` and include:

- what is affected (the Anpheros Platform API, an SDK, one of the websites) and where;
- how to reproduce it, step by step;
- what an attacker could do with it, as far as you know;
- the `Anpheros-Request-Id` header of any request involved, if you have it.

Never include real patient data, access tokens or API keys in a report. Use the sandbox and synthetic patients
for any testing, and do not access, modify or delete data that is not yours.

We will confirm that we received your report, keep you informed while we investigate, and tell you when the issue
is fixed. We are happy to credit you once the fix is published, if you want.

## Scope

- Anpheros Platform API (`platform.anpheros.com`, `developers.anpheros.com`)
- The official SDKs in [anpheros-sdk](https://github.com/anpheros/anpheros-sdk)
- The websites under `anpheros.com`

## Supported versions of the SDKs

Security fixes are released in the latest published version of each SDK (`anpheros_sdk` on pub.dev and
`@anpheros/sdk` on npm). Please upgrade to the latest version before reporting an SDK issue.
