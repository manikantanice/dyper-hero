# 🏘️ VILLAGE HERO

A complete 3D open-world action game built with React, Three.js, and Rapier physics.

## 🎮 Assets

| Asset | Source | Format |
|---|---|---|
| Village Environment | BitGem Fantasy Environment Level Set | GLTF + textures |
| Hero Character | Baby Superhero (Tripo AI) | GLB |

## 🚀 Quick Start

```bash
cd d:\antigravity_projects\village_hero_game
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in a desktop browser.

Click **Start Game** on the main menu. Click the game canvas to activate pointer lock for mouse camera control.

## 🕹️ Controls

| Key | Action |
|---|---|
| `Arrow Keys` | Move / walk |
| `Ctrl` | Sprint |
| `Space` | Jump (only while grounded) |
| `Shift` or `Left Click` | Attack (melee combo) |
| `Right Click` / `F` / `R` | Throw a milk bottle |
| `Q` / `H` | Drink milk to heal |
| `E` | Interact with NPCs |
| `Esc` | Pause / Release mouse |
| `Mouse` | Rotate camera |

## 📜 Missions

1. **Village Exploration** — Reach the village gate marker
2. **Meet the Villagers** — Find the Village Elder (teal NPC) and press `E` to talk
3. **Defend the Village** — Defeat all 3 red enemies (attack them with left-click)
4. **Report to Elder** — Return to the Village Elder to complete the game

## 🏗️ Architecture

```
src/
├── App.tsx                    # Root component, canvas & UI orchestration
├── main.tsx                   # React DOM entry
├── index.css                  # Game UI styles
├── store/
│   └── gameStore.ts           # Zustand state (game phase, missions, health)
├── hooks/
│   ├── useKeyboard.ts         # Keyboard input tracking
│   └── usePointerLock.ts      # Mouse pointer lock management
├── systems/
│   ├── HeroController.tsx     # Third-person physics controller
│   ├── Camera.tsx             # Orbit follow camera
│   ├── VillageEnvironment.tsx # GLTF village loader + physics ground
│   ├── EnemyAI.tsx            # Enemy state machine (idle/chase/attack)
│   ├── NPCSystem.tsx          # NPC interaction & dialogue triggers
│   └── WaypointMarker.tsx     # Mission objective markers
└── components/ui/
    ├── MainMenu.tsx
    ├── HUD.tsx                # Health bar, mission panel, crosshair
    ├── PauseMenu.tsx
    ├── SettingsMenu.tsx
    ├── DialoguePanel.tsx
    ├── DeathScreen.tsx
    └── MissionComplete.tsx
```

## 🔧 Tech Stack

- **React 18** + TypeScript
- **Vite** 5 dev server
- **Three.js** r167
- **@react-three/fiber** — React renderer for Three.js
- **@react-three/drei** — Helpers (useGLTF, Html, etc.)
- **@react-three/rapier** — Rapier physics engine (RigidBody, CapsuleCollider)
- **Zustand** — Game state management

## 🎯 Game Systems

### Hero Controller
- Physics-based movement via Rapier RigidBody
- Camera-relative arrow-key movement
- Ctrl sprint and grounded Space jump
- Attack broadcasts via EventTarget (`heroAttackEvent`)
- Procedural walk bob and idle sway animations
- Display stand filtered from hero model at runtime

### Enemy AI
- Three enemies, each with: idle → chase (14m range) → attack (2m range)
- Health: 60HP, 20 damage per hit from hero, 10 damage per hit to player
- Takes damage from hero attack events (not raw click polling)
- Health bars rendered via R3F `Html` component
- Mission 2 auto-advances when all 3 are defeated

### Camera
- Spherical orbit, follows hero position
- Mouse sensitivity configurable
- Pitch clamped (−15° to +55°)
- Smooth lerp with `12 * delta` factor

### Mission Flow
```
Start → Mission 0 (reach center) → Mission 1 (talk to elder)
      → Mission 2 (defeat 3 enemies) → Mission 3 (return to elder)
      → Victory Screen
```

## 🛠️ Development Notes

### Adding Enemy Models
Replace the red cylinder placeholder in `EnemyAI.tsx`. The physics collider and AI logic are separate from the visual mesh.

### Adding Animations
Install `@react-three/drei`'s `useAnimations`. If the GLB contains animation clips, pass the `actions` object to the controller and play clips by name.

### Changing Enemy Count / Positions
Edit `ENEMY_SPAWN_POSITIONS` in `src/systems/EnemyAI.tsx`.

### Changing NPC Dialogue
Edit the `elderLines` / `villagerLines` arrays in `src/systems/NPCSystem.tsx`.

## ⚠️ Known Limitations

- Neither the village nor hero GLB contains animation clips — procedural animations are used
- The hero display stand is hidden by name filtering (keywords: stand, base, pedestal, disc, etc.)
- Village collision uses a trimesh collider which may be expensive — consider simplified box colliders for very large maps
- No audio assets included — the audio system structure is ready for asset integration

## 📦 Build for Production

```bash
npm run build
npm run preview
```
