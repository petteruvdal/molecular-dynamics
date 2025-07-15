# 🧪 Installing LAMMPS on Cluster Documentation

This folder documents how to install **LAMMPS (Large-scale Atomic/Molecular Massively Parallel Simulator) for molecular dynamics simulations.

---

### 1. Folder Content

Navigate to personal storage in the cluster (so that **LAMMPS installation does not take up memory in your user directory)

```
cd /storage/eng/username
```

---

### 2. Check pre-requisits

```
module spider x

```

- CMake/3.29.3
- GCC/13.3.0
- OpenMPI/5.0.3
- Python/3.12.3
- LAMMPS/23June2022-kokkos

---
### 3. Load relevant modules

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

### 4. Download LAMMPS

- Git:

```
git clone -b release https://github.com/lammps/lammps.git lammps
```

### (Probably not: download LAMMPS)

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

### 4. Navigate directory

```
cd lammps
mkdir build
cd build
```
---

### 5. Build LAMMPS

Compile: 

```
cmake ../cmake           # configuration reading CMake scripts from ../cmake
#cmake --build .         # REPLACE THIS LINE
make -j 28               # compilation (or type "make"), with 28 being the maximum number of concurrently executed tasks
```

Optional: install the LAMMPS executable into your system with:

```
make install    # optional, copy compiled files into installation location

```


---


## 🧠 Notes

---

## 🔗 References

- [LAMMPS Documentation](https://docs.lammps.org/)
