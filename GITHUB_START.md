# AP2 TCG bei GitHub veröffentlichen

## 1. Repository vorbereiten

Das ZIP-Paket vollständig entpacken. Der Ordner `AP2-TCG-GitHub` enthält alle nötigen Dateien, insbesondere den versteckten Ordner `.github/` mit den automatischen Workflows.

Auf GitHub über **New repository** ein neues Repository, zum Beispiel `AP2-TCG`, erstellen. Bei der Erstellung **kein zusätzliches README, keine .gitignore und keine Lizenz** anlegen, da diese Dateien bereits enthalten sind.

Der Quellcode ist noch in Entwicklung. Vor einer öffentlichen Veröffentlichung Lizenz- und Bildrechte prüfen. Für die erste Erprobung ist ein privates Repository sinnvoll; die Nutzbarkeit von GitHub Pages für private Repositories hängt vom GitHub-Plan und den jeweiligen Einstellungen ab.

## 2. Dateien mit Git hochladen

Windows: PowerShell oder Git Bash im entpackten Ordner öffnen. Git muss installiert und der Zugriff auf GitHub eingerichtet sein.

```bash
git init
git add .
git commit -m "AP2 TCG v1.3 – GitHub-Erstimport"
git branch -M main
git remote add origin https://github.com/DEIN-BENUTZERNAME/AP2-TCG.git
git push -u origin main
```

`DEIN-BENUTZERNAME` durch den eigenen GitHub-Namen ersetzen. Wenn der Repositoryname anders lautet, auch die URL anpassen.

**Wichtig:** ZIP-Datei nicht unverändert als einzelne Datei ins GitHub-Repository hochladen. Der gesamte entpackte Inhalt muss mit seiner Ordnerstruktur übertragen werden.

## 3. Automatische Tests

GitHub → Repository → **Actions**. Der Workflow **Qualitätssicherung** sollte nach dem ersten Push starten. Er führt aus:

- 39 bestehende Regeltests einschließlich simulierter Partien.
- Formale Prüfung des Fragenkatalogs.
- Neuaufbau von Offline-, Browser- und Android-Dateien.
- Prüfung auf fehlende und voneinander abweichende Quelldateien.

Die Offline-HTML steht danach zusätzlich unter dem erfolgreichen Action-Lauf als Download-Artefakt bereit.

## 4. Web-App über GitHub Pages veröffentlichen

Unter **Settings → Pages** die Quelle **GitHub Actions** auswählen. Der Workflow **GitHub Pages veröffentlichen** erstellt und überträgt die Web-App aus `web/` nach einem Push auf `main`.

Die spätere Adresse folgt in der Regel dem Schema:

`https://DEIN-BENUTZERNAME.github.io/AP2-TCG/`

Der genaue Link erscheint nach erfolgreichem Deployment unter **Settings → Pages** und im Workflow-Lauf. Die Web-App unterstützt den Offline-Cache nach dem ersten erfolgreichen Laden über HTTPS.

## 5. Android-Debug-APK bauen

GitHub → **Actions → Android Debug-APK bauen → Run workflow**.

Nach einem erfolgreichen Lauf unter **Artifacts** das Paket `AP2-TCG-Android-Debug-APK` herunterladen, entpacken und die darin enthaltene `app-debug.apk` auf das Android-Smartphone übertragen.

Das ist eine **Debug-APK**, keine signierte Release-Version für einen App-Store. Der Build erfordert Zugriff des GitHub-Runners auf Google Maven, Gradle und das Android SDK. Der tatsächliche CI-Build wurde bei der Erstellung dieses Repositories noch nicht auf GitHub ausgeführt.

## 6. Spätere Änderungen

Vor jedem Commit lokal ausführen:

```bash
npm run check
```

Danach:

```bash
git add .
git commit -m "Kurze Beschreibung der Änderung"
git push
```

Bei Änderungen in `engine.js`, `app.js` oder der Fragenbank erzeugt `npm run build` aktualisierte Kopien für Browser, Android und die Offline-Datei. **Nicht** nur die Kopien unter `web/` ändern.

## Fehlerbehebung

- **Workflow rot:** Den fehlgeschlagenen Schritt im Actions-Protokoll öffnen. Häufige Ursachen: fehlende Datei, fehlerhafte Frage, Android-SDK-Buildproblem.
- **Keine Webseite:** Unter Settings → Pages die Quelle GitHub Actions auswählen und den Pages-Workflow kontrollieren.
- **Keine APK:** Sicherstellen, dass der Android-Workflow gestartet wurde und der Job erfolgreich abgeschlossen ist.
- **Download nur als ZIP:** GitHub Actions stellt Artefakte in ZIP-Archiven bereit. Vor dem Installieren der APK entpacken.
- **Alte Web-App:** Seite erneut laden; bei installierter PWA den Service Worker aktualisieren beziehungsweise Website-Daten überprüfen. Dabei können lokal gespeicherte Spielstände verloren gehen.
