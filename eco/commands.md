# Commands used for the layer-hop edit

Net `_06259_`, sink `_12662_/A_N` (met1, partial area ratio 464 vs 400 required).
DEF units are DBU (1000 per um).

## 1. Locate the segments in the DEF
    grep -n "( 270250 142970 )" hop_base.def   # 167395: met1 vertical to remove
    grep -n "( 270250 143650 )" hop_base.def   # 167488: long met1 run
    grep -n "( 271170 142970 )" hop_base.def   # M1M2_PR spelling reference

## 2. Apply the edit (on a copy)
    cp hop_base.def hop_edit.def
    sed -i '167488s/.*/      NEW met1 ( 271170 143650 ) ( 317170 * )/' hop_edit.def
    sed -i '167395s/.*/      NEW met2 ( 271170 142970 ) ( * 143650 )/' hop_edit.def
    sed -i '167395a\      NEW met1 ( 271170 143650 ) M1M2_PR' hop_edit.def

Line replacements go first so line numbers do not shift; the insertion is last.

## 3. Load and check in OpenROAD
    read_lef merged.nom.lef
    read_def hop_edit.def
    write_db hop_edit.odb
    check_antennas -verbose
