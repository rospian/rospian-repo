# rospian APT Repository

This repository hosts **prebuilt ROS 2 Jazzy Debian packages** for  
**Debian / Raspberry Pi OS Trixie (arm64)**.

It is intended for headless and embedded systems (e.g. Raspberry Pi) where
installing ROS 2 from source or from the full desktop stack is impractical.

---

## Supported platform

- **OS**: Debian / Raspberry Pi OS **Trixie**
- **Architecture**: `arm64`
- **ROS distribution**: **Jazzy**
- **Packaging**: native `.deb` packages, installable via `apt`

All ROS packages are namespaced using the standard ROS convention:

```
ros-jazzy-*
```

---

## Using the repository

### 1. Install the signing key

```bash
curl -fsSL https://rospian.github.io/rospian-repo/rospian-archive-keyring.asc   | gpg --dearmor | sudo tee /usr/share/keyrings/rospian-archive-keyring.gpg >/dev/null
```

### 2. Add the APT source

```bash
echo "deb [arch=arm64 signed-by=/usr/share/keyrings/rospian-archive-keyring.gpg] https://rospian.github.io/rospian-repo trixie-jazzy main"   | sudo tee /etc/apt/sources.list.d/rospian.list
```

### 3. Install packages

```bash
sudo apt update
sudo apt install ros-jazzy-<package-name>
```

---

## Why the APT suite is called `trixie-jazzy`

The APT **suite/codename** used by this repository is:

```
trixie-jazzy
```

This is **intentional**.

It encodes:
- the **OS base** (`trixie`)
- the **ROS distribution** (`jazzy`)

This prevents accidental mixing of packages from different ROS distributions
on the same OS.

### Important distinction

| Concept | Value | Used by |
|------|------|------|
| OS version | `trixie` | `rosdep`, Debian |
| ROS distro | `jazzy` | Package naming (`ros-jazzy-*`) |
| APT suite | `trixie-jazzy` | `apt` |

`rosdep` **must** continue to use:

```yaml
debian:
  trixie:
```

even though the APT repository uses `trixie-jazzy`.  
These are separate concepts and are expected to differ.

---

## Debug symbol packages

Some packages produce `*-dbgsym` packages containing debug symbols.
These can be large.

They may be included or excluded from publication depending on size
constraints, but **normal runtime packages are always published**.

---

## Repository structure

- The **default branch** contains documentation only.
- The **`gh-pages` branch** contains generated APT repository data:
  - `dists/`
  - `pool/`
  - `public/`

The `gh-pages` branch is **generated output**, published as snapshots,
and should not be edited manually.

---

## Intended audience

This repository is aimed at:
- Raspberry Pi users running ROS 2 Jazzy on Trixie
- Headless systems
- CI and build environments
- Advanced users who want fine-grained control over installed ROS components

---

## Disclaimer

This repository is not an official ROS distribution.
Packages are provided as-is and are intended for experienced users who
understand Debian-based systems and ROS packaging.

---

## License

Package licenses are defined by the upstream ROS packages they are built from.
This repository itself contains no original source code.
