# cmux — Référence de configuration

> Copie locale de la documentation officielle, pour référence rapide.
> **Source :** https://cmux.com/fr/docs/configuration (récupérée le 2026-06-23)
> En cas de doute, la page officielle fait foi.

cmux s'appuie sur le moteur de terminal **Ghostty**. La configuration se répartit en deux fichiers :
- **Ghostty config** → rendu du terminal (police, fond, couleurs, thème…)
- **cmux.json** → app cmux (raccourcis, actions, mises en page, sidebar…)

---

## Emplacements des fichiers

### Ghostty (rendu du terminal)
cmux cherche, dans l'ordre :
1. `~/.config/ghostty/config`
2. `~/Library/Application Support/com.mitchellh.ghostty/config`

Créer le fichier s'il n'existe pas :
```bash
mkdir -p ~/.config/ghostty
touch ~/.config/ghostty/config
```

Exemple :
```
font-family = SF Mono
font-size = 13
sidebar-font-size = 14
surface-tab-bar-font-size = 11
theme = One Dark
scrollback-limit = 50000000
split-divider-color = #3e4451
working-directory = ~/code
```

### cmux.json (app cmux)
1. `~/.config/cmux/cmux.json` (global)
2. `.cmux/cmux.json` (projet — actions et commandes au niveau projet)

- Accepte le **JSON avec commentaires et virgules finales** (JSONC).
- **Précédence :** `cmux.json` global > fenêtre Settings > fichiers legacy (fallback).
- **Schéma :** `schemaVersion` = `1` — https://raw.githubusercontent.com/manaflow-ai/cmux/main/web/data/cmux.schema.json

### Recharger la config
Après modification : **`Cmd+Shift+,`** ou la commande **`cmux reload-config`** (pas besoin de redémarrer l'app).

---

## Exemple de cmux.json

```json
{
  "$schema": "https://raw.githubusercontent.com/manaflow-ai/cmux/main/web/data/cmux.schema.json",
  "schemaVersion": 1,

  // "app": {
  //   "appearance": "dark",
  //   "menuBarOnly": false,
  //   "newWorkspacePlacement": "afterCurrent",
  //   "windowTitleTemplate": "[cmux:{windowToken}] {activeWorkspace}",
  //   "confirmQuit": "always",
  //   "openSupportedFilesInCmux": true,
  //   "workspaceInheritWorkingDirectory": true,
  //   "iMessageMode": true
  // },

  // "terminal": {
  //   "showScrollBar": false,
  //   "copyOnSelect": true,
  //   "autoResumeAgentSessions": true,
  //   "agentHibernation": { "enabled": false, "idleSeconds": 5, "maxLiveTerminals": 12 }
  // },

  // "sidebarAppearance": {
  //   "matchTerminalBackground": true
  // }
}
```

---

## Référence des paramètres `cmux.json`

### Métadonnées
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `$schema` | string | URL du schéma | URL optionnelle pour validation |
| `schemaVersion` | integer | 1 | Version du schéma |

### `app` (préférences générales)
| Paramètre | Type | Défaut | Valeurs / Description |
|---|---|---|---|
| `app.language` | string | system | system, en, ar, bs, zh-Hans, zh-Hant, da, de, es, fr, it, ja, ko, nb, pl, pt-BR, ru, th, tr |
| `app.appearance` | string | system | system, light, dark |
| `app.appIcon` | string | automatic | automatic, light, dark |
| `app.windowTitleTemplate` | string | "" | Modèle de titre ({windowId}, {windowToken}, {activeWorkspace}, {activeDirectory}, {defaultTitle}, {appName}) |
| `app.menuBarOnly` | boolean | false | Masquer l'icône Dock, garder le menu bar |
| `app.newWorkspacePlacement` | string | afterCurrent | top, afterCurrent, end |
| `app.forkConversationDefaultDestination` | string | right | right, left, top, bottom, newTab, newWorkspace |
| `app.workspaceInheritWorkingDirectory` | boolean | true | Hériter du répertoire courant |
| `app.minimalMode` | boolean | false | Masquer le titre du workspace, déplacer les contrôles |
| `app.keepWorkspaceOpenWhenClosingLastSurface` | boolean | false | Garder le workspace ouvert après fermeture du dernier pane |
| `app.focusPaneOnFirstClick` | boolean | true | Le 1er clic active et focus le pane |
| `app.preferredEditor` | string | "" | Commande éditeur personnalisée |
| `app.openSupportedFilesInCmux` | boolean | true | Ouvrir les fichiers supportés en preview cmux |
| `app.openMarkdownInCmuxViewer` | boolean | true | Ouvrir .md/.markdown/.mkd/.mdx en visionneuse |
| `app.globalFontMagnification` | integer | 100 | % de mise à l'échelle de l'UI cmux |
| `app.reorderOnNotification` | boolean | true | Remonter les workspaces notifiés |
| `app.iMessageMode` | boolean | false | Remonter le workspace et afficher le message |
| `app.sendAnonymousTelemetry` | boolean | true | Télémétrie anonyme |
| `app.confirmQuit` | string | always | always, dirty-only, never |
| `app.warnBeforeClosingTab` | boolean | true | Confirmer avant de fermer un onglet |
| `app.warnBeforeClosingTabXButton` | boolean | false | Confirmer avec le bouton X |
| `app.hideTabCloseButton` | boolean | false | Masquer les boutons de fermeture |
| `app.renameSelectsExistingName` | boolean | true | Sélectionner le nom au renommage |
| `app.commandPaletteSearchesAllSurfaces` | boolean | false | Chercher tous les panes, pas que le workspace actif |

### `terminal`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `terminal.showScrollBar` | boolean | true | Barre de défilement à droite |
| `terminal.scrollSpeed` | number | 1 | Multiplicateur de vitesse de défilement |
| `terminal.copyOnSelect` | boolean | false | Copier la sélection dans le presse-papiers |
| `terminal.autoResumeAgentSessions` | boolean | true | Reprendre les sessions agent restaurées |
| `terminal.showTextBoxOnNewTerminals` | boolean | false | Afficher la TextBox par défaut |
| `terminal.focusTextBoxOnNewTerminals` | boolean | false | Focus la TextBox par défaut |
| `terminal.textBoxMaxLines` | integer | 10 | Lignes max avant défilement de la TextBox |
| `terminal.agentHibernation` | object | — | Hibernation agent (enabled, idleSeconds, maxLiveTerminals) |
| `terminal.rendererRealization` | object | — | Récupérer la mémoire GPU des terminaux hors-écran |
| `terminal.resumeCommands` | array | [] | Approbations commande-préfixe pour restauration |

### `notifications`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `notifications.dockBadge` | boolean | true | Badge non-lu sur le Dock |
| `notifications.showInMenuBar` | boolean | true | Afficher dans le menu bar |
| `notifications.unreadPaneRing` | boolean | true | Surligner les panes non-lus |
| `notifications.paneFlash` | boolean | true | Flash du pane à la demande |
| `notifications.sound` | string | default | default, Basso, Blow, Bottle, Frog, Funk, Glass, Hero, Morse, Ping, Pop, Purr, Sosumi, Submarine, Tink, custom_file, none |
| `notifications.customSoundFilePath` | string | "" | Chemin d'un son personnalisé |
| `notifications.command` | string | "" | Commande shell optionnelle |
| `notifications.hooksMode` | string | append | append, replace |
| `notifications.hooks` | array | [] | Hooks shell (JSON en stdin) |

### `sidebar` (contenu / visibilité)
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `sidebar.hideAllDetails` | boolean | false | Masquer les détails par workspace |
| `sidebar.wrapWorkspaceTitles` | boolean | false | Titres sur plusieurs lignes |
| `sidebar.showWorkspaceDescription` | boolean | true | Afficher les descriptions |
| `sidebar.branchLayout` | string | vertical | vertical, inline |
| `sidebar.showNotificationMessage` | boolean | true | Afficher la dernière notification |
| `sidebar.showBranchDirectory` | boolean | true | Afficher le répertoire courant |
| `sidebar.showPullRequests` | boolean | true | Métadonnées PR |
| `sidebar.watchGitStatus` | boolean | true | Surveiller sans polling |
| `sidebar.makePullRequestsClickable` | boolean | true | PR cliquables |
| `sidebar.openPullRequestLinksInCmuxBrowser` | boolean | true | Ouvrir les PR dans le navigateur cmux |
| `sidebar.openPortLinksInCmuxBrowser` | boolean | true | Ouvrir les ports dans le navigateur cmux |
| `sidebar.showSSH` | boolean | true | Détails SSH |
| `sidebar.showPorts` | boolean | true | Ports en écoute |
| `sidebar.showLog` | boolean | true | Snippets de logs |
| `sidebar.showProgress` | boolean | true | Indicateurs de progression |
| `sidebar.showCustomMetadata` | boolean | true | Pills de métadonnées |
| `sidebar.rightMaxWidth` | number | — | Largeur max de la barre droite |

### `sidebarAppearance` (apparence)
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `sidebarAppearance.matchTerminalBackground` | boolean | false | Utiliser le fond du terminal au lieu de la teinte |
| `sidebarAppearance.tintColor` | color | #000000 | Couleur de teinte de base |
| `sidebarAppearance.lightModeTintColor` | color | null | Override en mode clair |
| `sidebarAppearance.darkModeTintColor` | color | null | Override en mode sombre |
| `sidebarAppearance.tintOpacity` | number | 0.03 | Opacité de la teinte (0–1) |

### `workspaceColors`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `workspaceColors.indicatorStyle` | string | leftRail | leftRail, solidFill, rail, border, wash, lift, typography, washRail, blueWashColorRail |
| `workspaceColors.selectionColor` | color | null | Fond du workspace sélectionné |
| `workspaceColors.notificationBadgeColor` | color | null | Couleur du badge non-lu |
| `workspaceColors.colors` | object | 16 couleurs | Palette nommée |
| `workspaceColors.paletteOverrides` | object | {} | Overrides hérités (préférer `colors`) |
| `workspaceColors.customColors` | array | [] | Liste héritée (préférer `colors`) |

Palette par défaut : Red, Crimson, Orange, Amber, Olive, Green, Teal, Aqua, Blue, Navy, Indigo, Purple, Magenta, Rose, Brown, Charcoal.

### `workspaceGroups`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `workspaceGroups.newWorkspacePlacement` | string | afterCurrent | afterCurrent, top, end |
| `workspaceGroups.byCwd` | object | — | Map motifs cwd → personnalisation de groupe |

### `automation`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `automation.socketControlMode` | string | cmuxOnly | off, cmuxOnly, automation, password, allowAll, openAccess, fullOpenAccess, notifications, full |
| `automation.socketPassword` | string | "" | Mot de passe (mode password) |
| `automation.claudeCodeIntegration` | boolean | true | Hooks d'intégration Claude Code |
| `automation.claudeBinaryPath` | string | "" | Chemin du binaire claude |
| `automation.workspaceAutoNaming` | boolean | false | Nommage auto par IA |
| `automation.autoNamingAgent` | string | auto | auto, claude, codex, grok, opencode, pi, omp… |
| `automation.ripgrepBinaryPath` | string | "" | Chemin du binaire ripgrep |
| `automation.suppressSubagentNotifications` | boolean | true | Masquer les notifications des sous-agents |
| `automation.ampIntegration` | boolean | true | Hooks Amp |
| `automation.cursorIntegration` | boolean | true | Hooks Cursor |
| `automation.geminiIntegration` | boolean | true | Hooks Gemini |
| `automation.kiroIntegration` | boolean | true | Hooks Kiro CLI |
| `automation.kiroNotificationLevel` | string | standard | minimal, standard, verbose |
| `automation.portBase` | integer | 9100 | Début des assignations CMUX_PORT |
| `automation.portRange` | integer | 10 | Ports réservés par workspace |

### `browser`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `browser.defaultSearchEngine` | string | google | google, duckduckgo, bing, kagi, startpage, brave, perplexity, exa, yahoo, ecosia, qwant, mojeek, wikipedia, github, baidu, yandex, custom |
| `browser.customSearchEngineName` | string | "" | Nom du moteur personnalisé |
| `browser.customSearchEngineURLTemplate` | string | google | URL de recherche (avec `{query}` ou `%s`) |
| `browser.showSearchSuggestions` | boolean | true | Suggestions dans l'omnibar |
| `browser.theme` | string | system | system, light, dark |
| `browser.discardHiddenWebViews` | boolean | true | Libérer la mémoire des onglets cachés |
| `browser.hiddenWebViewDiscardDelaySeconds` | number | 300 | Délai avant libération |
| `browser.openTerminalLinksInCmuxBrowser` | boolean | true | Liens du terminal dans le navigateur |
| `browser.interceptTerminalOpenCommandInCmuxBrowser` | boolean | true | Intercepter les commandes http(s) |
| `browser.hostsToOpenInEmbeddedBrowser` | array | [] | Liste blanche d'hosts |
| `browser.urlsToAlwaysOpenExternally` | array | [] | URLs toujours ouvertes en externe |
| `browser.insecureHttpHostsAllowedInEmbeddedBrowser` | array | localhost… | Hosts HTTP autorisés |
| `browser.showImportHintOnBlankTabs` | boolean | true | Hint d'import sur onglets vides |
| `browser.reactGrabVersion` | string | 0.1.29 | Version react-grab épinglée |

### `markdown`, `fileEditor`, `fileExplorer`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `markdown.fontSize` | integer | 15 | Taille de police (pt) |
| `markdown.fontFamily` | string | "" | Famille (vide = stack système) |
| `markdown.maxWidth` | integer | 980 | Largeur de colonne max (px) |
| `fileEditor.wordWrap` | boolean | false | Retour à la ligne auto |
| `fileExplorer.doubleClickAction` | string | preview | preview, defaultEditor, preferredEditor |

### `shortcuts`
| Paramètre | Type | Défaut | Description |
|---|---|---|---|
| `shortcuts.showModifierHoldHints` | boolean | true | Pastilles quand Cmd/Control est maintenu |
| `shortcuts.when` | object | {} | Prédicats de contexte par action (style VS Code) |
| `shortcuts.bindings` | object | défauts | Liaisons (string, array de 2, ou null) |

**Syntaxe `when`** — clés booléennes : `sidebarFocus`, `browserFocus`, `markdownFocus`, `terminalFocus`, `commandPaletteVisible`, `terminalFindVisible` ; clés typées : `sidebarMode` (files, find, sessions, feed, dock), `paneCount`, `workspaceCount` ; opérateurs : `!`, `&&`, `||`, `(…)`, `==`, `!=`, `=~`, `<`, `<=`, `>`, `>=`, `in [a, b]`.

```json
"shortcuts": {
  "bindings": { "selectWorkspaceByNumber": "ctrl+1" },
  "when": {
    "selectWorkspaceByNumber": "!sidebarFocus",
    "selectSurfaceByNumber": "sidebarMode == 'find' && paneCount > 1"
  }
}
```

---

## Raccourcis clavier par défaut (extraits)

**App** — Paramètres `cmd+,` · Recharger config `cmd+shift+,` · Palette `cmd+shift+p` · Recherche globale `opt+cmd+f` · Nouvelle fenêtre `cmd+shift+n` · Plein écran `ctrl+cmd+f` · Quitter `cmd+q`

**Espaces de travail** — Sidebar gauche `cmd+b` · Sidebar droite `cmd+opt+b` · Nouvel espace `cmd+n` · Ouvrir dossier `cmd+o` · Aller à `cmd+p` · Suivant/précédent `ctrl+cmd+]` / `ctrl+cmd+[` · Sélectionner 1–9 `cmd+1` · Renommer `cmd+shift+r` · Fermer `cmd+shift+w`

**Surfaces (onglets)** — Nouvelle `cmd+t` · Suivante/précédente `cmd+shift+]` / `cmd+shift+[` · Sélectionner 1–9 `ctrl+1` · Renommer `cmd+r` · Fermer `cmd+w` · Effacer l'écran `cmd+shift+k`

**Panneaux divisés** — Focus `opt+cmd+←/→/↑/↓` · Diviser droite `cmd+d` · Diviser bas `cmd+shift+d` · Zoom pane `cmd+shift+enter` · Égaliser `ctrl+cmd+=`

**Navigateur** — Ouvrir `cmd+shift+l` · Barre d'adresse `cmd+l` · Arrière/avant `cmd+[` / `cmd+]` · Recharger `cmd+r` · Zoom `cmd+=` / `cmd+-` / `cmd+0` · Outils dev `opt+cmd+i`

**Diff** — Ouvrir `ctrl+cmd+shift+d` · Scroll `j` / `k` · Début/fin `gg` / `shift+g` · Chercher `/`

**Recherche** — Chercher `cmd+f` · Dans le dossier `cmd+shift+f` · Suivant/précédent `cmd+g` / `opt+cmd+g`

**Notifications** — Afficher `cmd+i` · Aller au non-lu `cmd+shift+u` · Basculer l'état `opt+cmd+u`

> Liste complète et à jour : voir la [doc officielle](https://cmux.com/fr/docs/configuration).
