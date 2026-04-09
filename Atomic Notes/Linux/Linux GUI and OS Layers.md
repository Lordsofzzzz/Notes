---
status: seedling
tags: [linux, gui, desktop]
created: 2026-04-10
updated: 2026-04-10
---

# Linux GUI and OS Layers

## Summary
The layers of the graphical stack in Linux, including graphics drivers, display servers, window managers, and desktop environments.

## Details
Graphical stack and user interface layers in Linux systems.

### The Linux GUI Stack
```
Hardware (GPU, Display)
└── Kernel Graphics Drivers (DRM/KMS, Mesa, NVIDIA)
    └── Display Server / Protocol (X11 or Wayland)
        └── Window Manager (WM)
            └── Desktop Environment (DE)
                └── Display Manager (DM / Login Manager)
```

### Component Definitions
- **Display Server / Protocol (X11 / Wayland)**: Handles display output, input devices, and rendering.
- **Window Manager (WM)**: Controls window placement, borders, resizing, and workspace management (e.g., KWin, Mutter, i3).
- **Desktop Environment (DE)**: A complete graphical suite including a WM, panels, menus, and system tools (e.g., KDE Plasma, GNOME, XFCE).
- **Display Manager (DM)**: The graphical login screen (e.g., SDDM, GDM, LightDM).

### Display Servers: X11 vs. Wayland
- **X11 (X.Org Server)**: The older client-server architecture (released in 1984); supports network transparency natively via SSH forwarding.
- **Wayland**: The modern replacement (released in 2008) with a direct compositor model. It offers lower latency, smoother graphics, and improved security through input isolation.

### Support in Popular DEs
- **GNOME**: Defaulting to Wayland.
- **KDE Plasma**: Defaulting to Wayland on Plasma 6.
- **XFCE/LXQt**: Still primarily X11 with ongoing experimental Wayland support.

## Associative Trails
Explains how Linux handles graphical output, from raw hardware to polished desktop environments. This note refines the general [[Linux Architecture (Core)]] by detailing the UI-specific layers and documents the industry transition from X11 to Wayland.

## Connections
- [[Linux Architecture (Core)]]
- [[Linux Systems (MOC)]]

## Sources
- [[Linux GUI & OS Layers.md]]
