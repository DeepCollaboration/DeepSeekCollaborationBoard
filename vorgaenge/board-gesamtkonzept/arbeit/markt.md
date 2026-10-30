---
etage: Plan
status: erledigt
offene_fragen: 0
stand: 2026-10-30
---

# Markt-Eintrag und Paketname

**Befund:** Das Plugin heißt im Profil und in `package.json`
`@local/dsh-board`. Der Store ist die Liste **awesome-dsh-plugin**; ihr Katalog
(`dsh-plugin-catalog`, Format 1) wird aus je einer YAML-Datei je Plugin
erzeugt, die auf ein GitHub-Repository zeigt. `dsh plugin add` installiert von
dort.

**Folge:** Für den Store ist **kein npm-Paket und kein öffentlicher Paketname**
nötig. Der Eintrag heißt `owner/repo`
(`DeepCollaboration/DeepSeekCollaborationBoard-Plugin`); der interne Name
`@local/dsh-board` bleibt, wie er ist, und wird im Store nicht angezeigt.

**Nur falls später ein npm-Paket dazukommt**, braucht es einen öffentlichen
Namen; dann steht `dsh-collaborationboard` zur Wahl (Änderung an
`package.json`, `cordis.patch.yml` und im Profil). Bis dahin: keine Umbenennung.

**Der Weg in den Store** steht mit Eintrag, Voraussetzungen und den fehlenden
Schritten in `docs/store.md`. Offen ist nur, was er tun muss: committen und
pushen, Thema `dsh-plugin` setzen, PR an die Liste.
