# werkzeuge-oeffentlich

Gemeinsame Workflows, die auch **öffentliche** Repos des Kontos einbinden müssen. GitHub lässt ein
öffentliches Repo keinen Workflow aus einem privaten Repo aufrufen; private Repos dürfen dagegen
öffentliche Workflows nutzen. Alles andere bleibt im privaten Repo `werkzeuge` daneben.

| Ort | Inhalt |
|---|---|
| `.github/workflows/cockpit-melden.yml` | meldet Cockpit live einen neuen Stand eines Pull Requests |

Einbinden, in jedem Repo als `.github/workflows/melden.yml`:

```yaml
jobs:
  melden:
    permissions: { contents: read, id-token: write }
    uses: cloud-rust/werkzeuge-oeffentlich/.github/workflows/cockpit-melden.yml@main
```
