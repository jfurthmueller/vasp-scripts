# vasp-scripts
Some hopefully useful /bin/bash scripts for extracting data from VASP output. Currently this contains:

- "dffromxml":        extracts the dielectric function from "vasprun.xml" files of DFT, GW, and BSE
                      calculations and writes them into two files "REEPS" and "IMEPS" in an xy-format
                      suitable for feeding them directly into "xmgrace"; in the case of GW two version
                      are created: (RE/IM)EPS.ip for the simple independent-particle dielectric function
                      and (RE/IM)EPS.lfh for the RPA dielectric function including local-field effects
                      (on the Hartree level) -> usage is:
                         "dffromxml \<filename\>"
                      (argument \<filename\> is optional, default is "vasprun.xml" if not specified)
