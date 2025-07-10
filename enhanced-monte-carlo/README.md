# 🧪 Enhanced Monte Carlo (EMC) for LAMMPS Input Generation

This folder documents how to use **EMC (Enhanced Monte Carlo)** to generate LAMMPS-compatible `.data` and `.field` files for molecular dynamics simulations.

We use EMC to pack monomers (e.g. epoxy) with appropriate force field parameters (e.g. OPLS-AA), and export structured input for LAMMPS simulations.

---

## 📁 Folder Contents

```
my_epoxy.esh                     # EMC input script
build.emc
```

---

## 🛠️ How to Use

### 1. Edit the ESH script

Create a file called `my_epoxy.esh`:

```esh
#!/usr/bin/env emc.pl                      

ITEM OPTIONS
replace		true
# mass		  true
ntotal		24
density		0.2
field		  opls/2024/opls-aa
# build_dir	.
ITEM		END

ITEM SHORTHAND
epoxy,    C1C(O1)COC2=CC=C(C=C2)CC3=CC=C(C=C3)OCC4CO4,   67   # 16 / 24
hardener, NCCNCCNCCN,                                    33   #  8 / 24
ITEM END

ITEM		END
```

---

Then run it executable:

```bash
perl */emc_setup.pl -replace my_epoxy.esh
```

### 2. Edit the build script

You now have a file called `build.emc`:

Change number of molecules:

```emc
    id		-> epoxy, system -> main, group -> epoxy, n -> n_epoxy},
  cluster	-> {
    id		-> hardener, system -> main, group -> hardener, n -> n_hardener}
```

To:

```emc
    id		-> epoxy, system -> main, group -> epoxy, n -> 16},
  cluster	-> {
    id		-> hardener, system -> main, group -> hardener, n -> 8}
```

---

## 📦 Output Files

After running EMC, you should get:

```text
epoxy_lammps.data     # LAMMPS structure file
epoxy_lammps.field    # Force field info
epoxy_lammps.pdb      # Packed structure (optional)
```

---

## 🧬 Example LAMMPS Input

Your `in.lammps` should include:

```lammps
units real
atom_style full
read_data epoxy_lammps.data
include epoxy_lammps.field

# (Add fixes, pair_style, thermo, etc. here)
```

---

## 🧠 Notes

- `opls-aa/` in the emc should contain `opls-aa.field` (can be found in EMC package).
- EMC must be installed and callable via `emc` in terminal.

---

## 🔗 References

- [EMC GitHub](https://github.com/uchicago-voth/emc)
- [LAMMPS Docs](https://docs.lammps.org/)
- [OPLS-AA paper](https://doi.org/10.1021/ja9621760)
