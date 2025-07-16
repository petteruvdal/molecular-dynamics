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

Save using:
```
module save lammps
```

Reload using:
```
module restore lammps
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


Start interactive session
```

salloc --nodes=1 --ntasks=1 --mem-per-cpu=3988 --cpus-per-task=8 --partition=interactive --time=01:00:00

```

Compile
```
module purge
module restore lammps
cd /storage/eng/esrwwn/lammps/build

# Run cmake and make
cmake ../cmake \
  -D CMAKE_INSTALL_PREFIX=$HOME/lammps-install \
  -D BUILD_MPI=on \
  -D BUILD_OMP=on \
  -D PKG_MOLECULE=on \
  -D PKG_KSPACE=on \
  -D PKG_RIGID=on \
  -D PKG_USER-REAXC=on \
  -D LAMMPS_EXCEPTIONS=on \
  -D BUILD_SHARED_LIBS=on

make -j 28
make install
```
```
exit
```
---
6. Submit a script

```
[esrwwn@godzilla esrwwn]$ ls
epoxy  lammps  lammps_sim
[esrwwn@godzilla esrwwn]$ ssh dedicated120
The authenticity of host 'dedicated120 (137.205.63.79)' can't be established.
ED25519 key fingerprint is SHA256:nkWfJzMDiRv3kMlHS3GNvocjK7vXa96CBkiAnNbg0Cg.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'dedicated120' (ED25519) to the list of known hosts.
(esrwwn@dedicated120) Password: 
(esrwwn@dedicated120) Password: 
(esrwwn@dedicated120) Password: 
esrwwn@dedicated120: Permission denied (publickey,keyboard-interactive).
[esrwwn@godzilla esrwwn]$                                                                                
SCRTP ESSENTIAL MAINTENANCE                                                    
godzilla.csc.warwick.ac.uk will reboot at                                      
Wed 16 Jul 13:14:08 BST 2025                                                   
                                                                               

Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:09:12 BST):

The system will reboot at Wed 2025-07-16 13:14:12 BST!


Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:10:12 BST):

The system will reboot at Wed 2025-07-16 13:14:12 BST!


Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:11:12 BST):

The system will reboot at Wed 2025-07-16 13:14:12 BST!


Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:12:12 BST):

The system will reboot at Wed 2025-07-16 13:14:12 BST!


Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:13:12 BST):

The system will reboot at Wed 2025-07-16 13:14:12 BST!


Broadcast message from root@godzilla.csc.warwick.ac.uk (Wed 2025-07-16 13:14:12 BST):

The system will reboot now!

Connection to godzilla.csc.warwick.ac.uk closed by remote host.
Connection to godzilla.csc.warwick.ac.uk closed.
(base) xuvdpe@ml-250401-128 ~ % ssh esrwwn@godzilla.csc.warwick.ac.uk
Enter passphrase for key '/Users/xuvdpe/.ssh/id_rsa': 
# DO NOT RUN COMPUTATIONALLY INTENSIVE JOBS ON THIS MACHINE
# including, but not limited to:
# - comsol
# - dropbox
# - gromacs (gmx)
# FAILURE TO COMPLY MAY RESULT IN ACCOUNT SUSPENSION
Last login: Wed Jul 16 11:37:53 2025 from 172.24.9.95
Wed 16 Jul 13:36:20 BST 2025
[esrwwn@godzilla ~]$ cd /storage/eng/esrwwn
[esrwwn@godzilla esrwwn]$ ls
epoxy  lammps  lammps_sim
[esrwwn@godzilla esrwwn]$ cd lammps_sim
[esrwwn@godzilla lammps_sim]$ ls
my_epoxy_higher_density
[esrwwn@godzilla lammps_sim]$ cd my_epoxy_higher_density
[esrwwn@godzilla my_epoxy_higher_density]$ ls
build.emc                          my_epoxy_higher_density.data    my_epoxy_higher_density.params
esrwwn@godzilla.csc.warwick.ac.uk  my_epoxy_higher_density.emc.gz  my_epoxy_higher_density.pdb.gz
jobscript                          my_epoxy_higher_density.esh     my_epoxy_higher_density.psf.gz
my_epoxy_crosslink.in              my_epoxy_higher_density.in      my_epoxy_higher_density.vmd
[esrwwn@godzilla my_epoxy_higher_density]$ nano jobscript
[esrwwn@godzilla my_epoxy_higher_density]$ sbatch jobscript
Submitted batch job 5186016
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
[esrwwn@godzilla my_epoxy_higher_density]$ ls
build.emc                          my_epoxy_higher_density.emc.gz  my_epoxy_higher_density.psf.gz
esrwwn@godzilla.csc.warwick.ac.uk  my_epoxy_higher_density.esh     my_epoxy_higher_density.vmd
jobscript                          my_epoxy_higher_density.in      sim_0010_270p00_p00.out.5186016
my_epoxy_crosslink.in              my_epoxy_higher_density.params
my_epoxy_higher_density.data       my_epoxy_higher_density.pdb.gz
[esrwwn@godzilla my_epoxy_higher_density]$ nano sim_0010_270p00_p00.out.5186016
[esrwwn@godzilla my_epoxy_higher_density]$ nano jobscript
[esrwwn@godzilla my_epoxy_higher_density]$ sbatch jobscript
Submitted batch job 5186037
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       0:03      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       0:05      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       0:07      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       0:11      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ ls
build.emc                          my_epoxy_higher_density.emc.gz  my_epoxy_higher_density.psf.gz
esrwwn@godzilla.csc.warwick.ac.uk  my_epoxy_higher_density.esh     my_epoxy_higher_density.vmd
jobscript                          my_epoxy_higher_density.in      sim_0010_270p00_p00.out.5186016
my_epoxy_crosslink.in              my_epoxy_higher_density.params  sim_0010_270p00_p00.out.5186037
my_epoxy_higher_density.data       my_epoxy_higher_density.pdb.gz
[esrwwn@godzilla my_epoxy_higher_density]$ less sim_0010_270p00_p00.out.5186037
Restoring modules from user's lammps
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       1:04      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ top

top - 13:40:19 up 21 min,  5 users,  load average: 0.06, 0.10, 0.09
Tasks: 664 total,   1 running, 663 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem : 384918.1 total, 381986.0 free,   3543.9 used,   1285.6 buff/cache
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used. 381374.2 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                
   1612 root      20   0   82940   4224   3840 S   6.2   0.0   0:00.54 irqbalance                             
  10819 esrwwn    20   0    8496   4608   3456 R   6.2   0.0   0:00.01 top                                    
      1 root      20   0  171904  13440   9600 S   0.0   0.0   0:04.43 systemd                                
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd                               
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_                        
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_g                        
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-sync_                        
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-slub_                        
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-netns                        
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri            
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-mm_pe                        
     12 root      20   0       0      0      0 I   0.0   0.0   0:00.91 kworker/u192:1-ixgbe                   
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_kthre                        
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_rude_                        
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_trace                        
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.00 ksoftirqd/0                            
     17 root      20   0       0      0      0 I   0.0   0.0   0:00.08 rcu_preempt                            
     18 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_exp_par_gp_                        
     19 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_exp_gp_kthr                        
     20 root      rt   0       0      0      0 S   0.0   0.0   0:00.00 migration/0                            
     21 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0                          
     23 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0                                
     24 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1                                
     25 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/1                          
[esrwwn@godzilla my_epoxy_higher_density]$ ll
total 402
-rw-r--r-- 1 esrwwn esrwwn   4368 Jul 14 17:22 build.emc
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 esrwwn@godzilla.csc.warwick.ac.uk
-rw-r----- 1 esrwwn esrwwn    931 Jul 16 13:39 jobscript
-rw-r--r-- 1 esrwwn esrwwn   8687 Jul 14 17:36 my_epoxy_crosslink.in
-rw-r--r-- 1 esrwwn esrwwn 403047 Jul 14 17:22 my_epoxy_higher_density.data
-rw-r--r-- 1 esrwwn esrwwn  48870 Jul 14 17:22 my_epoxy_higher_density.emc.gz
-rw-r--r-- 1 esrwwn esrwwn    351 Jul 14 17:10 my_epoxy_higher_density.esh
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 my_epoxy_higher_density.in
-rw-r--r-- 1 esrwwn esrwwn  30679 Jul 14 17:22 my_epoxy_higher_density.params
-rw-r--r-- 1 esrwwn esrwwn  16347 Jul 14 17:22 my_epoxy_higher_density.pdb.gz
-rw-r--r-- 1 esrwwn esrwwn  26793 Jul 14 17:22 my_epoxy_higher_density.psf.gz
-rwx------ 1 esrwwn esrwwn   1346 Jul 14 17:22 my_epoxy_higher_density.vmd
-rw-r----- 1 esrwwn esrwwn   1844 Jul 16 13:38 sim_0010_270p00_p00.out.5186016
-rw-r----- 1 esrwwn esrwwn  18405 Jul 16 13:39 sim_0010_270p00_p00.out.5186037
[esrwwn@godzilla my_epoxy_higher_density]$ less sim_0010_270p00_p00.out.5186037
Restoring modules from user's lammps
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5186037       mnf sim_0010   esrwwn  R       2:03      1 dedicated120
[esrwwn@godzilla my_epoxy_higher_density]$ scancel 5186037
[esrwwn@godzilla my_epoxy_higher_density]$ squeue -u esrwwn
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
[esrwwn@godzilla my_epoxy_higher_density]$ ll
total 402
-rw-r--r-- 1 esrwwn esrwwn   4368 Jul 14 17:22 build.emc
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 esrwwn@godzilla.csc.warwick.ac.uk
-rw-r----- 1 esrwwn esrwwn    931 Jul 16 13:39 jobscript
-rw-r--r-- 1 esrwwn esrwwn   8687 Jul 14 17:36 my_epoxy_crosslink.in
-rw-r--r-- 1 esrwwn esrwwn 403047 Jul 14 17:22 my_epoxy_higher_density.data
-rw-r--r-- 1 esrwwn esrwwn  48870 Jul 14 17:22 my_epoxy_higher_density.emc.gz
-rw-r--r-- 1 esrwwn esrwwn    351 Jul 14 17:10 my_epoxy_higher_density.esh
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 my_epoxy_higher_density.in
-rw-r--r-- 1 esrwwn esrwwn  30679 Jul 14 17:22 my_epoxy_higher_density.params
-rw-r--r-- 1 esrwwn esrwwn  16347 Jul 14 17:22 my_epoxy_higher_density.pdb.gz
-rw-r--r-- 1 esrwwn esrwwn  26793 Jul 14 17:22 my_epoxy_higher_density.psf.gz
-rwx------ 1 esrwwn esrwwn   1346 Jul 14 17:22 my_epoxy_higher_density.vmd
-rw-r----- 1 esrwwn esrwwn   1844 Jul 16 13:38 sim_0010_270p00_p00.out.5186016
-rw-r----- 1 esrwwn esrwwn  18405 Jul 16 13:39 sim_0010_270p00_p00.out.5186037
[esrwwn@godzilla my_epoxy_higher_density]$ cat jobscript 
#!/bin/bash
#
# SLURM job submission script generated by MyCluster
#
#SBATCH --nodelist dedicated120
# Job name
#SBATCH -J sim_0010_270p00_p00

# Send status information to this email address.
##SBATCH --mail-user=
# Send me an e-mail when the job has finished.
##SBATCH --mail-type=ALL
# Redirect output stream to this file.
#SBATCH --output sim_0010_270p00_p00.out.%j
# Which project should be charged
##SBATCH -A turbine
# Partition name
#SBATCH -p mnf
# Number of tasks
# Exclusive node use
##SBATCH --exclusive
# Do not requeue job on node failure
##SBATCH --no-requeue
# High performance cpu governor

##SBATCH --cpu-freq=Performance

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=16  #1-28, 8 recommended
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=3988
#SBATCH --time=04:00:00

module purge
module restore lammps
cd /storage/eng/esrwwn/lammps_sim

srun /storage/eng/esrwwn/lammps/build/lmp -in my_epoxy_higher_density.in
[esrwwn@godzilla my_epoxy_higher_density]$ pwd
/storage/eng/esrwwn/lammps_sim/my_epoxy_higher_density
[esrwwn@godzilla my_epoxy_higher_density]$ nano jobscript
[esrwwn@godzilla my_epoxy_higher_density]$ sbatch jobscript
Submitted batch job 5186048
[esrwwn@godzilla my_epoxy_higher_density]$ 
[esrwwn@godzilla my_epoxy_higher_density]$ less sim_0010_270p00_p00.out.5186048
Restoring modules from user's lammps
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

[esrwwn@godzilla my_epoxy_higher_density]$ scancel 5186048
[esrwwn@godzilla my_epoxy_higher_density]$ nano jobscript
[esrwwn@godzilla my_epoxy_higher_density]$ sbatch jobscript
Submitted batch job 5186049
[esrwwn@godzilla my_epoxy_higher_density]$ less sim_0010_270p00_p00.out.5186049
Restoring modules from user's lammps
/storage/eng/esrwwn/lammps_sim/my_epoxy_higher_density
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.

Host:      dedicated120.csc.warwick.ac.uk
Framework: psec
Component: munge
--------------------------------------------------------------------------
--------------------------------------------------------------------------
A requested component was not found, or was unable to be opened.  This
means that this component is either not installed or is unable to be
used on your system (e.g., sometimes this means that shared libraries
that the component requires are unable to be found/loaded).  Note that
PMIx stopped checking at the first component that it did not find.
[esrwwn@godzilla my_epoxy_higher_density]$ scancel 5186049
[esrwwn@godzilla my_epoxy_higher_density]$ ll
total 438
-rw-r--r-- 1 esrwwn esrwwn   4368 Jul 14 17:22 build.emc
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 esrwwn@godzilla.csc.warwick.ac.uk
-rw-r----- 1 esrwwn esrwwn    960 Jul 16 13:45 jobscript
-rw-r----- 1 esrwwn esrwwn    628 Jul 16 13:45 log.lammps
-rw-r--r-- 1 esrwwn esrwwn   8687 Jul 14 17:36 my_epoxy_crosslink.in
-rw-r--r-- 1 esrwwn esrwwn 403047 Jul 14 17:22 my_epoxy_higher_density.data
-rw-r--r-- 1 esrwwn esrwwn  48870 Jul 14 17:22 my_epoxy_higher_density.emc.gz
-rw-r--r-- 1 esrwwn esrwwn    351 Jul 14 17:10 my_epoxy_higher_density.esh
-rw-r--r-- 1 esrwwn esrwwn   2946 Jul 14 17:21 my_epoxy_higher_density.in
-rw-r--r-- 1 esrwwn esrwwn  30679 Jul 14 17:22 my_epoxy_higher_density.params
-rw-r--r-- 1 esrwwn esrwwn  16347 Jul 14 17:22 my_epoxy_higher_density.pdb.gz
-rw-r--r-- 1 esrwwn esrwwn  26793 Jul 14 17:22 my_epoxy_higher_density.psf.gz
-rwx------ 1 esrwwn esrwwn   1346 Jul 14 17:22 my_epoxy_higher_density.vmd
-rw-r----- 1 esrwwn esrwwn   1844 Jul 16 13:38 sim_0010_270p00_p00.out.5186016
-rw-r----- 1 esrwwn esrwwn  18584 Jul 16 13:41 sim_0010_270p00_p00.out.5186037
-rw-r----- 1 esrwwn esrwwn  20797 Jul 16 13:43 sim_0010_270p00_p00.out.5186048
-rw-r----- 1 esrwwn esrwwn  20852 Jul 16 13:45 sim_0010_270p00_p00.out.5186049
[esrwwn@godzilla my_epoxy_higher_density]$ cat 
build.emc                          my_epoxy_higher_density.emc.gz     my_epoxy_higher_density.vmd
esrwwn@godzilla.csc.warwick.ac.uk  my_epoxy_higher_density.esh        sim_0010_270p00_p00.out.5186016
jobscript                          my_epoxy_higher_density.in         sim_0010_270p00_p00.out.5186037
log.lammps                         my_epoxy_higher_density.params     sim_0010_270p00_p00.out.5186048
my_epoxy_crosslink.in              my_epoxy_higher_density.pdb.gz     sim_0010_270p00_p00.out.5186049
my_epoxy_higher_density.data       my_epoxy_higher_density.psf.gz     
[esrwwn@godzilla my_epoxy_higher_density]$ cat jobscript 
#!/bin/bash
#
# SLURM job submission script generated by MyCluster
#
#SBATCH --nodelist dedicated120
# Job name
#SBATCH -J sim_0010_270p00_p00

# Send status information to this email address.
##SBATCH --mail-user=
# Send me an e-mail when the job has finished.
##SBATCH --mail-type=ALL
# Redirect output stream to this file.
#SBATCH --output sim_0010_270p00_p00.out.%j
# Which project should be charged
##SBATCH -A turbine
# Partition name
#SBATCH -p mnf
# Number of tasks
# Exclusive node use
##SBATCH --exclusive
# Do not requeue job on node failure
##SBATCH --no-requeue
# High performance cpu governor

##SBATCH --cpu-freq=Performance

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=16  #1-28, 8 recommended
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=3988
#SBATCH --time=04:00:00

module purge
module restore lammps
cd /storage/eng/esrwwn/lammps_sim/my_epoxy_higher_density

pwd

srun /storage/eng/esrwwn/lammps/build/lmp -in my_epoxy_higher_density.in
[esrwwn@godzilla my_epoxy_higher_density]$ module purge
module restore lammps
cd /storage/eng/esrwwn/lammps_sim/my_epoxy_higher_density
Restoring modules from user's lammps
[esrwwn@godzilla my_epoxy_higher_density]$ nano jobscript 

  GNU nano 5.6.1                                                             jobscript                                                                       
#!/bin/bash
#
# SLURM job submission script generated by MyCluster
#
#SBATCH --nodelist dedicated120
# Job name
#SBATCH -J sim_0010_270p00_p00

# Send status information to this email address.
##SBATCH --mail-user=
# Send me an e-mail when the job has finished.
##SBATCH --mail-type=ALL
# Redirect output stream to this file.
#SBATCH --output sim_0010_270p00_p00.out.%j
# Which project should be charged
##SBATCH -A turbine
# Partition name
#SBATCH -p mnf
# Number of tasks
# Exclusive node use
##SBATCH --exclusive
# Do not requeue job on node failure
##SBATCH --no-requeue
# High performance cpu governor

##SBATCH --cpu-freq=Performance

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=16  #1-28, 8 recommended
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=3988
#SBATCH --time=04:00:00

module purge
module restore lammps
cd /storage/eng/esrwwn/lammps_sim/my_epoxy_higher_density

```



---


## 🧠 Notes

---

## 🔗 References

- [LAMMPS Documentation](https://docs.lammps.org/)
