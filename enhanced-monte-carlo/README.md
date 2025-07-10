# 🧪 Enhanced Monte Carlo (EMC) for LAMMPS Input Generation

This folder documents how to use **EMC (Enhanced Monte Carlo)** to generate LAMMPS-compatible `.data` and `.field` files for molecular dynamics simulations.

We use EMC to pack monomers (e.g. epoxy) with appropriate force field parameters (e.g. OPLS-AA), and export structured input for LAMMPS simulations.

---

## 📁 Folder Contents

```
epoxy.pdb                        # Structure of the epoxy monomer
epoxy_pack.emc                   # EMC input script
run_emc.sh                       # Bash script to execute EMC
opls-aa/                         # Force field directory
└── opls-aa.field                # OPLS-AA parameter file
```

---

## 🛠️ How to Use

### 1. Edit the EMC script

Create a file called `epoxy_pack.emc`:

```emc
(* EMC: Script *)

variables = {
  seed            -> 12345,
  ntotal          -> 1,
  output          -> "epoxy_lammps",
  field           -> "opls-aa",
  location1       -> "./opls-aa/",
  density1        -> 1.1,
  temperature     -> 300
};

pdb = {
  name -> "epoxy",
  forcefield -> opls
};

systems = {
  {
    molecules -> {epoxy -> 1},
    density -> 1.1,
    name -> "epoxybox",
    origin -> {0.0, 0.0, 0.0},
    relax -> {ncycles -> 100, radius -> 5.0}
  }
};

out = {
  name -> "epoxy_lammps",
  lammps -> true,
  forcefield -> true,
  pdb -> true
};
```

---

### 2. Create the run script

Create a file called `run_emc.sh`:

```bash
#!/bin/bash
# Run EMC to generate LAMMPS input files

emc -i epoxy_pack.emc
```

Make it executable:

```bash
chmod +x run_emc.sh
```

Then run it:

```bash
./run_emc.sh
```

Alternatively, run directly:

```bash
emc -i epoxy_pack.emc
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

- `epoxy.pdb` must be valid (e.g., from PubChem or Open Babel).
- `opls-aa/` should contain `opls-aa.field` (can be found in EMC package).
- EMC must be installed and callable via `emc` in terminal.

---

## 🔗 References

- [EMC GitHub](https://github.com/uchicago-voth/emc)
- [LAMMPS Docs](https://docs.lammps.org/)
- [OPLS-AA paper](https://doi.org/10.1021/ja9621760)
