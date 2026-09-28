# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_001320.jpg
- L1+R6: LR_noM (edge)
- L4+R2+M4: LRM (center)
- L5+R3: LR_noM (mid)
- L2+R1+M2: LRM (center)
- L3+R5: LR_noM (mid)
- R4+M1: RM_noL (edge)
- M3: M_only (center)
- M5: M_only (mid)
- M6: M_only (edge)
## adasind_012570.jpg
- L6+R6: LR_noM (center)
- L2+R5+M1: LRM (mid)
- L5+R4: LR_noM (center)
- L8+R1+M4: LRM (mid)
- L1+R2+M2: LRM (mid)
- L4+R8+M6: LRM (center)
- L9+R3+M3: LRM (mid)
- L7+R7+M9: LRM (center)
- L3+M12: LM_noR (center)
- R9+M13: RM_noL (center)
- M7: M_only (center)
- M8: M_only (center)
- M10: M_only (center)
- M11: M_only (center)
## adasind_036720.jpg
- L4+R4: LR_noM (edge)
- L2+R1: LR_noM (mid)
- L1+R3: LR_noM (center)
- L3+R2+M3: LRM (center)
- M1: M_only (edge)
- M2: M_only (edge)
- M4: M_only (center)
- M6: M_only (edge)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 5 | 3 | 1 | 0 | 1 | 0 | 6 |
| mid | 4 | 3 | 0 | 0 | 0 | 0 | 1 |
| edge | 0 | 2 | 0 | 0 | 1 | 0 | 4 |
