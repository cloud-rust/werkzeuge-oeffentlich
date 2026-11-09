# werkzeuge-oeffentlich

Gemeinsame Workflows, die auch **öffentliche** Repos des Kontos einbinden müssen. GitHub lässt ein
öffentliches Repo keinen Workflow aus einem privaten Repo aufrufen; private Repos dürfen dagegen
öffentliche Workflows nutzen. Alles andere bleibt im privaten Repo `werkzeuge` daneben.

| Ort | Inhalt |
|---|---|
| `.github/workflows/cockpit-melden.yml` | meldet Cockpit live einen neuen Stand eines Pull Requests |
| `.github/workflows/cockpit-ereignis.yml` | meldet Cockpit live, wie ein Lauf ausging (Ausrollen, Kandidat, Release) |

Einbinden, in jedem Repo als `.github/workflows/melden.yml`:

```yaml
jobs:
  melden:
    permissions: { contents: read, id-token: write }
    uses: cloud-rust/werkzeuge-oeffentlich/.github/workflows/cockpit-melden.yml@main
```

Ein Lauf meldet sein Ergebnis ins Event-Center als letzter Job:

```yaml
  melden:
    needs: [ausrollen]
    if: always()
    permissions: { contents: read, id-token: write }
    uses: cloud-rust/werkzeuge-oeffentlich/.github/workflows/cockpit-ereignis.yml@main
    with:
      quelle: ausrollen
      ergebnis: ${{ contains(needs.*.result, 'failure') && 'failure' || needs.ausrollen.result }}
```
