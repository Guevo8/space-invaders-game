# 🏗️ Space Invaders - Technische Architektur

## Übersicht

Dieses Dokument beschreibt die Architektur und das Design des Space Invaders Spiels auf Codeebene.

## Game Loop

Das Herzstück des Spiels ist eine klassische Game Loop, die kontinuierlich ausgeführt wird:

```
┌─────────────────────────────────┐
│   requestAnimationFrame()       │
├─────────────────────────────────┤
│   1. INPUT (Keys/Events)        │
│   2. UPDATE (Logik & Physik)    │
│   3. DRAW (Rendering)           │
└─────────────────────────────────┘
         ↓
    ~60 FPS
```

## Klassen-Struktur

### 1. Vector
Eine Utility-Klasse für 2D-Vektoren, die Positionen und Geschwindigkeiten darstellt.

```javascript
class Vector {
    constructor(x, y) {
        this.x = x;  // X-Koordinate
        this.y = y;  // Y-Koordinate
    }
}
```

**Verwendung**: Position und Velocity aller beweglichen Objekte.

---

### 2. Player
Repräsentiert das Spieler-Raumschiff 🚀.

**Eigenschaften**:
- `position`: Vector mit Spieler-Position
- `velocity`: Vector mit Bewegungsgeschwindigkeit
- `speed`: 5 Pixel pro Frame
- `width` / `height`: 40x20 Pixel
- `emoji`: '🚀'

**Methoden**:
- `draw()`: Rendert das Emoji auf dem Canvas
- `update()`: Aktualisiert die Position basierend auf Velocity und enforced Randbegrenzungen
- `shoot()`: Erstellt ein neues Geschoss (`Projectile`) und fügt es zum Array hinzu

**Input-Handling**:
```
ArrowLeft  → velocity.x = -5
ArrowRight → velocity.x = +5
Space      → shoot() aufrufen
```

---

### 3. Projectile
Repräsentiert Geschosse (sowohl vom Spieler als auch von Gegnern).

**Eigenschaften**:
- `position`: Vector mit Geschoss-Position
- `speed`: Pixel pro Frame (negativ = aufwärts, positiv = abwärts)
- `color`: Farbe (Spieler: '#84cc16', Gegner: '#ef4444')
- `width` / `height`: 5x10 Pixel

**Methoden**:
- `draw()`: Rendert ein Rechteck
- `update()`: Aktualisiert Position.y um speed

**Lifecycle**:
1. Erstellt durch `player.shoot()` oder `invader.shoot()`
2. Wird jedes Frame aktualisiert
3. Wird gelöscht wenn außerhalb des Canvas oder bei Kollision

---

### 4. Invader
Repräsentiert einen einzelnen Gegner 👾.

**Eigenschaften**:
- `position`: Vector mit Gegner-Position
- `width` / `height`: 30x30 Pixel
- `emoji`: '👾'

**Methoden**:
- `draw()`: Rendert das Emoji
- `update(velocity)`: Aktualisiert Position basierend auf Grid-Geschwindigkeit
- `shoot()`: Erstellt ein Gegner-Geschoss

**Hinweis**: Einzelne Invader bewegen sich NICHT unabhängig. Sie werden durch die `Grid`-Klasse verwaltet.

---

### 5. Grid
Verwaltet die Formation aller Invader und deren Bewegung.

**Eigenschaften**:
- `invaders`: Array aller aktiven Invader
- `velocity`: Vector für Formation-Geschwindigkeit (startet mit x=2)
- `position`: Vector für Formation-Position (nicht aktiv genutzt)

**Grid-Layout**:
```
8 Spalten × 4 Reihen = 32 Gegner

Abstand: 45 Pixel horizontal, 40 Pixel vertikal
Startposition: Oben-Mitte (zentriert)
```

**Methoden**:

#### `update()`
Diese Methode führt die komplexe Bewegungslogik durch:

1. **Randprüfung**: Überprüft ob ein Invader den Rand erreicht
   - Links-Rand (x ≤ 0) bei Linksbewegung
   - Rechts-Rand (x + width ≥ canvas.width) bei Rechtsbewegung

2. **Richtungswechsel**: 
   - Wenn Rand erreicht → `velocity.x *= -1`
   - Geschwindigkeit erhöhen: `velocity.x *= 1.05` (5% schneller)

3. **Bewegung nach unten**:
   - Bei Richtungswechsel alle Invader 20 Pixel nach unten verschieben

4. **Seitenbewegung**:
   - Normalerweise um `velocity.x` Pixel seitwärts bewegen

5. **Schießen**:
   - Zufällig (2% Wahrscheinlichkeit pro Frame)
   - Zufälliger Invader aus Formation schießt

```
Update-Logik (Pseudocode):

FÜR JEDEN Invader:
    Falls Rand erreicht:
        velocity.x *= -1
        velocity.x *= 1.05
        Alle nach unten verschieben (20px)
    SONST:
        Normal seitwärts bewegen

Random(2%) → Zufälliger Invader schießt
```

---

## Kollisionserkennung (AABB)

Das Spiel nutzt **Axis-Aligned Bounding Box (AABB)** Kollisionserkennung.

### AABB-Formel:
```
Treffer, wenn:
  rect1.left   < rect2.right  &&
  rect1.right  > rect2.left   &&
  rect1.top    < rect2.bottom &&
  rect1.bottom > rect2.top
```

### Implementierung:
```javascript
if (
    p.x - p.width/2 < inv.x + inv.width &&
    p.x + p.width/2 > inv.x &&
    p.y < inv.y + inv.height &&
    p.y + p.height > inv.y
) {
    // TREFFER!
}
```

### Kollisions-Szenarien:

1. **Spieler-Geschoss → Invader**
   - Geschoss wird gelöscht
   - Invader wird gelöscht
   - Score += 100

2. **Gegner-Geschoss → Spieler**
   - Game Over
   - Message: "Du wurdest getroffen!"

3. **Invader → Spieler (y-Position)**
   - Game Over
   - Message: "Die Invasion hat den Boden erreicht!"

4. **Alle Invader eliminiert**
   - Game Over
   - Message: "🎉 Du hast gewonnen!"

---

## Spielzustände

### 1. INIT
- `score = 0`
- `gameActive = true`
- Player initialisiert
- Grid mit 32 Invader erstellt
- `projectiles` und `invaderProjectiles` Arrays geleert

### 2. PLAYING
- Game Loop läuft kontinuierlich
- Events werden verarbeitet
- Kollisionen werden geprüft
- Score wird aktualisiert

### 3. GAME OVER
- `gameActive = false`
- Game Loop wird mit `cancelAnimationFrame()` gestoppt
- Game Over Screen angezeigt
- Final Score angezeigt
- Warten auf Neustart-Button

---

## Input-Handling

### Keyboard Events:

```javascript
keydown: {
    ArrowLeft:  keys.ArrowLeft = true
    ArrowRight: keys.ArrowRight = true
    Space:      player.shoot() + keys.Space = true
}

keyup: {
    ArrowLeft:  keys.ArrowLeft = false
    ArrowRight: keys.ArrowRight = false
    Space:      keys.Space = false
}
```

### Input → Update Mapping:
```
keyboard state → player.velocity.x → player.position.x
```

---

## Rendering

### Canvas Setup:
- **Auflösung**: 600×400 Pixel (fest)
- **Context**: 2D Canvas
- **Color Space**: sRGB

### Draw-Reihenfolge (pro Frame):
1. `ctx.clearRect()` - Canvas löschen (schwarz)
2. `player.draw()` - Spieler-Emoji
3. `grid.draw()` - Alle Invader-Emojis
4. Alle `projectiles` - Grüne Linien
5. Alle `invaderProjectiles` - Rote Linien

### Emoji-Rendering:
```javascript
ctx.font = '28px';          // Schriftgröße
ctx.textAlign = 'center';   // Horizontal zentriert
ctx.textBaseline = 'middle'; // Vertikal zentriert
ctx.fillText(emoji, x, y);
```

---

## Performance-Überlegungen

### 1. Canvas Clearing
- Nur das gesamte Canvas wird geleert (`clearRect`)
- Nicht einzelne Objekte

### 2. Array-Verwaltung
- Geschosse werden gelöscht wenn außerhalb
- Invader werden gelöscht bei Treffer
- Rückwärts iterieren um Index-Probleme zu vermeiden

### 3. Zufallszahlen
- `Math.random() < 0.02` für Gegner-Schießen
- Effizient ohne komplexe RNG

### 4. requestAnimationFrame
- Browser-optimiert (60 FPS auf den meisten Geräten)
- Pausiert wenn Tab nicht sichtbar
- Besser als `setInterval`

---

## Balancing-Parameter

| Parameter | Wert | Effekt |
|-----------|------|--------|
| Player Speed | 5 px/frame | Steuerbarkeit |
| Grid Base Speed | 2 px/frame | Gegner-Tempo |
| Speed Increase | 1.05x | Progressive Schwierigkeit |
| Player Projectile Speed | 8 px/frame ↑ | Schnell genug zum Treffen |
| Invader Projectile Speed | 4 px/frame ↓ | Ausweichbar |
| Enemy Shoot Chance | 0.02 (2%) | ~1 Schuss pro 50 Frames |
| Grid Drop | 20 px | Schneller Abstieg |
| Score per Kill | 100 points | Motivation |

---

## Erweiterungsmöglichkeiten

### 1. Soundeffekte
- Web Audio API
- Datei-Pfade: `sounds/shoot.mp3`, `sounds/explosion.mp3`

### 2. Touch-Steuerung
- `touchstart`, `touchmove` Events
- Finger-Position → Player-Position

### 3. Schwierigkeitsstufen
- Hard: `velocity *= 1.1` statt 1.05
- Easy: `velocity *= 1.02`

### 4. Powerups
- Neue Klasse: `Powerup`
- Typen: Triple Shot, Shield, Speed Boost

### 5. Boss-Gegner
- Größere Invader
- Mehrere Health-Points
- Andere Bewegungsmuster

---

## Browser-Kompatibilität

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ✅ Mobile Safari (iOS 14+)
- ✅ Chrome Mobile (90+)

---

**Letzte Änderung**: November 2025
