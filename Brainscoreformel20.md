# Brain Score – Final Formula (Locked)

This document defines the final Brain Score calculation.
The formula is tuned to match human intuition and fixed anchor points.

---

## Design Assumptions

- Two observation phases exist in the game
- Each phase grants 15 seconds free observation
- Best performance is achieved at 20 seconds total observation time
- Maximum observation time: 120 seconds
- Hits dominate over time
- Each additional hit remains meaningful up to 6 hits
- Random luck is capped and clearly separated from skill

---

## Definitions

T = total observation time in seconds  
(sum of all observation phases, including joker time)

H = number of hits (integer, 0…6)

---

## Constants

FREE_TIME = 20 seconds  
MAX_TIME = 120 seconds  
EXTRA_TIME_RANGE = 100 seconds  

---

## Clamp Observation Time

If T < FREE_TIME → T = FREE_TIME  
If T > MAX_TIME → T = MAX_TIME  

---

## Time Factor

Extra observation time:

E = clamp(T - FREE_TIME, 0…EXTRA_TIME_RANGE)

Time score factor:

S_time = 1 - 0.31 * (E / EXTRA_TIME_RANGE) ^ 0.855

Properties:

S_time = 1.00 at T = 20 s  
S_time = 0.69 at T = 120 s  

---

## Hit Factor (Logistic, Fairness-Balanced)

k = 1.0  
c = 2.5976583  

Raw logistic function:

L(H) = 1 / (1 + exp(-k * (H - c)))

Normalized hit factor:

S_hits = L(H) / L(6)

Special rule:

If H = 0 → no percentage score

---

## Final Brain Score

If H = 0:  
Display: "Brain Score: No Hit, no Score"

Else:

BrainScore (%) = 100 * S_time * S_hits

---

## Anchor Validation

T = 20 s, H = 6 → 100 %  
T = 120 s, H = 6 → 69 %  
T = 120 s, H = 1 → 12 %  
H = 0 → No Hit, no Score  

---

## Design Intent

Skill (hits) outweighs observation time  
Extra observation time reduces score smoothly  
Six hits always represent strong performance  
Random luck is capped and clearly distinguishable from skill  

---

## Modification Rule

This formula is locked.

Any change to time constants, logistic parameters (k, c),
coefficients or exponents requires full re-validation
of all anchor points.
