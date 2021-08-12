# vasp-scripts
Some hopefully useful /bin/bash scripts for extracting data from VASP output.
Currently this contains:

- "dffromxml":       extracts the dielectric function from "vasprun.xml" files
                     of DFT, GW, and BSE calculations and writes them into two
                     files "REEPS" and "IMEPS" in an xy-format suitable for
                     feeding them directly into "xmgrace"; in the case of GW
                     two version are created: (RE/IM)EPS.ip for the simple
                     independent-particle dielectric function and (RE/IM)EPS.lfh
                     for the RPA dielectric function including local-field
                     effects (on the Hartree level) -> usage is:
                            "dffromxml \<filename\>"
                        (argument \<filename\> is optional, default is
                        "vasprun.xml" if not specified)

- "extract-kpoints"  extracts band structure data for selected k-points from
                     file EIGENVAL creating a file EIGENVAL.band which looks
                     like an EIGENVAL file of a band structure calculation with
                     the given k-points -> usage is:
                            "extract-kpoints \<list of k-points\>"
                     where the list of k-points has to be a space-separated
                     list of integers representing the number (index) of the
                     corresponding k-point on the list of irreducible k-points
                     found in OUTCAR (or on IBZKPT for non-HF-like runs ...).
                     The order is arbitrary and the same k-point may occur
                     several times on the list. The purpose is to misuse band
                     structure calculations on (sufficiently dense!) regular
                     k-meshes for creation of band structures along certain
                     high-symmetry lines of the Brillouin zone by extracting
                     all k-points of the 3D mesh which are members of these
                     high-symmetry lines. It is mainly intended for GW results
                     (where additional "band structure k-points of zero weight"
                     cannot be supplied). The bad news is: you need to find out
                     yourself the corresponding (index) list of k-points ...

- "extract-pdos"     extracts the total and (if present) also partial/projected
                     densities of states (DOS) from file POSCAR. Thereby, it
                     offers various possibilities for partial (l and lm) sums
                     over given lists of atoms -> usage is:
                            "extract-pdos" \<lists of atoms to be summed\>"
                     where the list of atoms can be empty (optional argument if
                     you are not interested in any partial sums) or can be a
                     space-separated list of several atom number list (these
                     number have to reflect the numbering of atoms according to
                     the order of coordinates on file POSCAR); each of these
                     lists can be either be given in "dash format" (like "1-4"
                     or "5-10") or as comma-separated list (like "1,5,9,10")
                     and if several lists are specified formats can be "mixed".
                     At the end it creates always files:
                     "dos.dat"           the total DOS and integrated DOS
                                         as found in the head of file DOSCAR
                     "lmdos.\<i\>.dat"    the lm-projected DOS of atom i
                     "ldos.\<i\>.dat"     the l-projected DOS of atom i
                     "ados.\<i\>.dat"     the total projected DOS of atom i
                     "lmdos.\<list\>.dat" the lm-projected DOS summed up ...
                     "ldos.\<list\>.dat"  the l-projected DOS summed up ...
                     "ados.\<list\>.dat"  the total projected DOS summed up ...
                     ... over all atoms specified in the list; even if no lists
                     were provided as argument always (independent of what was
                     given) the list "1-\<natoms\>" (sum over all atoms) is
                     written (with the ados.\<all\>.dat to be compared to DOS
                     data on file dos.dat -- since projection is never complete
                     one can then check this way how incomplete the projection
                     was; ideally the plain total DOS and the sum over all
                     projections should not differ too much ...).

- "swapdos"          requires an input file called "DOSPLOT" (and writes then
                     output to file "DOSPLOT-swap") which shall contain the
                     total DOS and integrated DOS in the form of a list
                         "energy   DOS   int-DOS"
                     (file "dos.dat" created by "extract-pdos" has such a format
                     but one can also take some DOSCAR and kill its header lines
                     and all data on projected DOS if present). Upon output the
                     integrated DOS will be thrown away and DOS data are written
                     in an "inverted order"
                         "DOS    energy"
                     suitable to load it (as a data file) into a combined plot
                     of band structures together with a 90-degree-rotated DOS
                     (y as the energy axis and bands/DOS on the x-axis ...).

- "swapldos"         requires an input file called "LDOS" (and writes then
                     output to "LDOS-swap-s", "LDOS-swap-p", and "LDOS-swap-d"
                     with s, p, d representing the corresponding angular momenta
                     l=0,1,2 -- no f/l=3 is forseen yet!() which shall contain
                     the total l-projected DOS in the form of a list
                         "energy   s-DOS   p-DOS   d-DOS"
                     (file "ldos.\<list of all atoms\>dat" created by script
                     "extract-pdos" has such a format). Upon output DOS data are
                     written in an "inverted order" (on file LDOS-swap-\<l\>)
                         "l-DOS    energy"
                     suitable to load it (as a data file) into a combined plot
                     of band structures together with a 90-degree-rotated l-DOS
                     (y as the energy axis and bands/l-DOS on the x-axis ...).
