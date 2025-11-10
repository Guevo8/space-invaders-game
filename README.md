# 🚀 Space Invaders Game

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![HTML](https://img.shields.io/badge/HTML-100%25-orange.svg)](https://github.com/Guevo8/space-invaders-game)

A modern, playable Space Invaders game built with vanilla JavaScript and HTML5 Canvas. Fully deploy-ready and optimized for GitHub Pages.

## 🎮 Play Now

**[▶️ Play the Game](https://guevo8.github.io/space-invaders-game/)**

## 🎮 Features

- **Classic Gameplay**: Control the spaceship and defend Earth from invaders
- **Progressive Difficulty**: Enemies speed up as more are defeated
- **Modern Technology**: Vanilla JavaScript with no external dependencies
- **Full Collision Detection**: Precise AABB hitboxes for all objects
- **Responsive Design**: Adapts to different screen sizes
- **Mobile-ready**: Keyboard controls with planned touchpad support

## 🕹️ How to Play

### Controls

- **Arrow Left / Arrow Right**: Move spaceship
- **Spacebar**: Shoot
- **Boundary limits**: No movement outside game area

### Objective

- Destroy all enemies (👾) before they reach the bottom
- Each enemy gives +100 points
- Win by eliminating all waves or lose if:
  - Enemy projectiles hit the spaceship (🚀)
  - Enemies reach the bottom edge

## 📁 Project Structure

```
space-invaders-game/
├── index.html          # Main file (complete game)
├── README.md           # This file
├── LICENSE             # MIT License
├── .gitignore          # Git ignore rules
└── docs/               # Documentation
    ├── ARCHITECTURE.md # Technical architecture
    └── DEPLOYMENT.md   # Deployment guide
```

## 🚀 Deployment Options

### 1. GitHub Pages (Recommended)

```bash
# Enable GitHub Pages in Repository Settings:
# Settings → Pages → Source: Deploy from a branch → main
# The app will be available at https://guevo8.github.io/space-invaders-game
```

### 2. Local Testing

```bash
# Option A: With Python
python3 -m http.server 8000

# Option B: With Node.js (http-server)
npx http-server

# Then open: http://localhost:8000
```

### 3. Other Hosting Options

- **Vercel**: Connect directly with GitHub
- **Netlify**: Drag & drop or Git integration
- **Surge.sh**: `npm install -g surge && surge`

## 🔧 Technical Architecture

The game uses a class-based object model with the following components:

- **Vector**: Utility class for positions and velocities
- **Player**: Controllable spaceship
- **Projectile**: Projectiles from player and enemies
- **Invader**: Individual enemy unit
- **Grid**: Management of invader formation

See `docs/ARCHITECTURE.md` for detailed structure.

## 📊 Game Balancing

- **Enemies per Wave**: 8 columns × 4 rows = 32 enemies
- **Base Speed**: 2 pixels per update
- **Speed Increase**: +5% with each direction change
- **Player Projectile Speed**: 8 pixels per update (upward)
- **Enemy Projectile Speed**: 4 pixels per update (downward)
- **Enemy Shoot Probability**: 2% per update

## 🎨 Design Overview

- **Color Scheme**: Dark Mode (Slate/Blue Palette)
- **Emojis**: 🚀 (Player) and 👾 (Enemy) for visual appeal
- **Font**: Inter (Google Fonts) for modern UI
- **Canvas Resolution**: 600×400 pixels

## 🔄 Roadmap

- [ ] Add sound effects
- [ ] Touch controls for mobile
- [ ] Multiple difficulty levels
- [ ] Leaderboard system
- [ ] Powerups and special weapons
- [ ] Boss enemies
- [ ] Particle effects on hits

## 📝 License

This project is licensed under the MIT License. See `LICENSE` for details.

## 🤝 Contributing

Contributions are welcome! Please fork the repository, develop on a feature branch, and submit a pull request.

## 👨‍💻 Author

**Guevo** - Solo Developer & Creative Technologist

---

**Enjoy the game! 🎮**
