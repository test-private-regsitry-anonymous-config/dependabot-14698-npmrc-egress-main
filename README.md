# Dependabot issue 14698 reproduction

Minimal reproduction for [github/dependabot-updates#14698](https://github.com/github/dependabot-updates/issues/14698).

The repository configures an anonymous, non-default npm registry only in
`.npmrc`. The registry is intentionally absent from `.github/dependabot.yml`.
With proxy egress enforcement enabled, a Dependabot version update should fail
before registry access because `r.cnpmjs.org` is absent from the
job's credentials metadata and egress allowlist.

The public anonymous mirror stands in for the customer's anonymous private
Nexus registry while exercising the same ecosystem-config discovery path.

Expected proxy output includes:

```text
egress not allowlisted r.cnpmjs.org
403 https://r.cnpmjs.org/nodemailer
```

The supported workaround is to declare the registry in `dependabot.yml`. That
workaround is intentionally not applied here because it prevents the failure.
