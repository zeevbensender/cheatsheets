# 🚨 Ubuntu: "No space left on device" — Practical Cheat Sheet

## 🔴 0. FAST START (always first)

```bash
df -h
df -i
```

Interpret:
- `Use% = 100%` → disk full
- `IUse% = 100%` → too many files (inode issue)
- Specific mount full → work **there**, not blindly on `/`

---

## 🔴 1. VERIFY REAL WRITE LOCATION

```bash
df -h /path/you/use
```

> Common mistake: expected mount is not used → writing to `/`

---

## 🔴 2. CHECK MOUNTS (NVMe / NFS)

```bash
lsblk -f
df -h -t nfs,nfs4
```

### Cases

### A. Mount missing
```bash
findmnt /your/mount/point
```

If empty → data goes to `/`

Fix:
```bash
sudo mount -a
```

---

### B. NFS full (server-side)

```bash
df -h /mount/point
```

→ Must clean on **server**

---

### C. NFS broken / stuck

Symptoms:
- `ls` hangs
- random I/O errors

```bash
timeout 5 ls /mount/point
```

Fix:
```bash
sudo umount -f /mount/point
# if stuck
sudo umount -l /mount/point
```

---

## 🔴 3. FIND WHERE SPACE IS USED

```bash
sudo du -h --max-depth=1 / | sort -h
```

Drill down:
```bash
sudo du -h --max-depth=1 /var | sort -h
```

---

## 🔴 4. QUICK SAFE CLEANUP

### Logs (very common)
```bash
sudo journalctl --vacuum-time=3d
sudo truncate -s 0 /var/log/*.log
```

---

### APT
```bash
sudo apt clean
sudo apt autoremove -y
```

---

### Temp
```bash
sudo rm -rf /tmp/*
```

---

## 🔴 5. IF INODES FULL

```bash
df -i
```

Find source:
```bash
sudo find / -xdev -type f | cut -d "/" -f2 | sort | uniq -c | sort -n
```

> Usually logs, caches, temp files

---

## 🔴 6. HIDDEN SPACE (VERY IMPORTANT)

Deleted files still open:

```bash
sudo lsof | grep deleted
```

Fix:
- restart process
- or reboot

---

## 🔴 7. FIND HUGE FILES

```bash
sudo find / -type f -size +1G 2>/dev/null
```

---

## 🔴 8. EMERGENCY CLEAN (SAFE MINIMAL)

```bash
sudo journalctl --vacuum-size=100M
sudo apt clean
sudo rm -f /var/log/*.gz
```

---

## 🔴 9. VERIFY

```bash
df -h
df -i
```

---

# 📦 APPENDIX A — Docker

## Where
```bash
/var/lib/docker
```

## Check
```bash
docker system df
sudo du -h --max-depth=1 /var/lib/docker | sort -h
```

## Cleanup
```bash
docker system prune -a --volumes
```

## Consequences
- removes unused containers/images/volumes
- no impact on running containers

---

# 📦 APPENDIX B — Milvus

## Where
```bash
/var/lib/milvus
```

## Check
```bash
sudo du -h --max-depth=1 /var/lib/milvus | sort -h
```

## Cleanup ideas
- drop unused collections
- compact collections
- remove old test data

## Consequences
- possible data loss if careless

---

# 📦 APPENDIX C — Datasets (FBIN / FVECS / NPZ)

## Find
```bash
sudo find / -type f \( -name "*.fbin" -o -name "*.fvecs" -o -name "*.npz" \) -size +1G 2>/dev/null
```

## Check dirs
```bash
du -sh /mnt/*
```

## Notes
- often tens/hundreds of GB
- easy to forget old runs

---

## ⚠️ NFS dataset trap

Deleting locally:
- may NOT free space immediately
- depends on server cleanup

---

# 📦 APPENDIX D — JetBrains IDEs

## Where
```bash
~/.cache/JetBrains
```

## Cleanup
```bash
rm -rf ~/.cache/JetBrains/*
```

## Consequences
- reindex on next start
- no data loss

---

# 📦 APPENDIX E — VSCode / Remote Server

## Where
```bash
~/.vscode-server
```

## Check
```bash
du -sh ~/.vscode-server
```

## Cleanup
```bash
rm -rf ~/.vscode-server/bin/*
rm -rf ~/.vscode-server/data/*
```

## Consequences
- server reinstalled on next connect
- slower first connection

---

# 📦 APPENDIX F — Go

## Where
```bash
~/go/pkg/mod
~/.cache/go-build
```

## Cleanup
```bash
go clean -cache
go clean -modcache
```

## Consequences
- dependencies re-downloaded
- slower next build

---

# 📦 APPENDIX G — Build Systems (CMake / Ninja / Make)

## Where
```bash
build/
cmake_build/
out/
```

## Cleanup
```bash
rm -rf build cmake_build out
```

## Consequences
- full rebuild required

---

# 📦 APPENDIX H — Python

## Where
```bash
~/.cache/pip
__pycache__/
```

## Cleanup
```bash
pip cache purge
find . -name "__pycache__" -type d -exec rm -rf {} +
```

## Consequences
- minor slowdown only

---

# 📦 APPENDIX I — SAFE DEV CLEAN (COMBINED)

```bash
rm -rf ~/.cache/*
go clean -cache
pip cache purge
docker system prune -a --volumes
```

## Warning
- safe for dev machines
- not for production without review

---

# 📦 APPENDIX J — COMMON ROOT CAUSES

- Writing to `/` instead of mounted disk
- NFS not mounted → silent fallback to root
- Docker filling `/var/lib/docker`
- Logs growing endlessly
- Deleted files still open
- Millions of small files (inode exhaustion)
- Old datasets never cleaned

---

# 🧠 CORE RULE

Always run:

```bash
df -h
df -i
df -h /your/path
```

Everything else is secondary.
