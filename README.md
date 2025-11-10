# 🚀 Space Invaders Game

Ein modernes, spielbares Space Invaders Spiel, gebaut mit Vanilla JavaScript und HTML5 Canvas. Vollständig deploy-fähig und optimiert für GitHub Pages.

## 🎮 Features

- **Klassisches Gameplay**: Steuere dein Raumschiff und verteidige die Erde vor Invasoren
- **Progressive Schwierigkeit**: Die Gegner werden schneller, je mehr du besiegst
- **Moderne Technologie**: Vanilla JavaScript ohne externe Dependencies
- **Vollständige Kollisionserkennung**: Genaue AABB-Hitboxen für alle Objekte
- **Responsive Design**: Anpassbar an verschiedene Bildschirmgrößen
- **Mobile-ready**: Keyboard-Steuerung mit geplanter Touchpad-Unterstützung

## 🕹️ Spielanleitung

### Steuerung
- **Pfeil Links / Pfeil Rechts**: Raumschiff bewegen
- **Leertaste**: Schießen
- **Nur am Spielfeldrand möglich**: Keine Bewegung außerhalb der Grenzen

### Spielziel
- Vernichte alle Gegner (👾) bevor sie dich erreichen
- Jeder Gegner bringt +100 Punkte
- Gewinne durch Vernichtung aller Wellen oder verliere durch:
  - Gegner-Geschosse treffen dein Schiff (🚀)
  - Gegner erreichen die untere Bildschirmkante

## 📁 Projektstruktur

```
space-invaders-game/
├── index.html          # Hauptdatei (komplettes Spiel)
├── README.md           # Diese Datei
├── LICENSE             # MIT License
├── .gitignore          # Git-Ignore Regeln
└── docs/               # Dokumentation
    └── ARCHITECTURE.md # Technische Architektur
```

## 🚀 Deploy-Optionen

### 1. GitHub Pages (Empfohlen)
```bash
# Aktiviere GitHub Pages in den Repository-Settings:
# Settings → Pages → Source: Deploy from a branch → main
# Die App ist dann unter https://guevo8.github.io/space-invaders-game erreichbar
```

### 2. Lokal testen
```bash
# Option A: Mit Python
python3 -m http.server 8000

# Option B: Mit Node.js (http-server)
npx http-server

# Dann öffne: http://localhost:8000
```

### 3. Andere Hosting-Optionen
- **Vercel**: Direkt mit GitHub verbinden
- **Netlify**: Drag & Drop oder Git-Integration
- **Surge.sh**: `npm install -g surge && surge`

## 🔧 Technische Architektur

Das Spiel verwendet ein klassenbasiertes Objekt-Modell mit folgenden Komponenten:

- **Vector**: Utility-Klasse für Positionen und Geschwindigkeiten
- **Player**: Steuerbare Spielfigur
- **Projectile**: Geschosse von Spieler und Gegnern
- **Invader**: Einzelne Gegner-Einheit
- **Grid**: Verwaltung der Invader-Formation

Siehe `docs/ARCHITECTURE.md` für detaillierten Aufbau.

## 📊 Spielbalancing

- **Gegner pro Wave**: 8 Spalten × 4 Reihen = 32 Gegner
- **Basisgeschwindigkeit**: 2 Pixel pro Update
- **Geschwindigkeitssteigerung**: +5% bei jeder Richtungsänderung
- **Spieler-Geschossgeschwindigkeit**: 8 Pixel pro Update (aufwärts)
- **Gegner-Geschossgeschwindigkeit**: 4 Pixel pro Update (abwärts)
- **Gegner-Schusswahrscheinlichkeit**: 2% pro Update

## 🎨 Design-Überblick

- **Farbschema**: Dark Mode (Slate/Blue Palette)
- **Emojis**: 🚀 (Spieler) und 👾 (Gegner) für visuellen Appeal
- **Schriftart**: Inter (Google Fonts) für modernes UI
- **Canvas-Auflösung**: 600×400 Pixel

## 🔄 Roadmap

- [ ] Sound-Effekte hinzufügen
- [ ] Touch-Steuerung für Mobile
- [ ] Mehrere Schwierigkeitsstufen
- [ ] Leaderboard System
- [ ] Powerups und spezielle Waffen
- [ ] Boss-Gegner
- [ ] Particle-Effekte bei Treffern

## 📝 Lizenz

Dieses Projekt ist unter der MIT License lizenziert. Siehe `LICENSE` für Details.

## 🤝 Beitragen

Contributions sind willkommen! Bitte erstelle einen Fork, entwickle auf einem Feature-Branch und stelle einen Pull Request.

## 👨‍💻 Autor

**Guevo** - Solo Developer & Creative Technologist

---

**Viel Spaß beim Spielen! 🎮**