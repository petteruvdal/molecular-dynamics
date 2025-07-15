# 🧪 Installing LAMMPS on Cluster Documentation

This folder documents how to install **LAMMPS (Large-scale Atomic/Molecular Massively Parallel Simulator) for molecular dynamics simulations.

---

### 1. Folder Content

Navigate to personal storage in the cluster (so that **LAMMPS installation does not take up memory in your user directory)

```
cd /storage/eng/username
```

---

## 2. Check pre-requisits

```
module spider x

```

- CMake/3.29.3
- GCC/13.3.0
- OpenMPI/5.0.3
- Python/3.12.3
- LAMMPS/23June2022-kokkos

---
## 3. Load relevant modules

```bash
module purge
module load GCC
module load OpenMPI
module load Python
module load CMake
```

And check using:

```
module list
```

---

## 4. Install LAMMPS and navigate directory

- Git:

```
git clone -b release https://github.com/lammps/lammps.git lammps
```

- Static Linux, go to: https://download.lammps.org/static/

- Ubuntu and Debian Linux:
```
sudo apt-get install lammps
```

- Fedora Linux
```
dnf install lammps-openmpi
```

- OpenSuse Linux
```
zypper install lammps
```

- Gentoo Linux
```
emerge --ask lammps
```

- Archlinux build-script
```
git clone https://aur.archlinux.org/lammps.git
```




---

## 4. Navigate directory

```
cd lammps
mkdir build
cd build
```

---

## 🧠 Notes

---

## 🔗 References

- [LAMMPS Documentation](https://docs.lammps.org/)
