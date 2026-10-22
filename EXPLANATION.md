# Lab 2 – what was done and how

This file explains the work in `RV-CLT-PS-Lab2.ipynb`. It is for personal reference, not part of the submission (note: it is committed to the repository).

## Files

| File | Purpose |
|---|---|
| `RV-CLT-PS-Lab2.ipynb` | The executed notebook with all four tasks solved (submission). |
| `RV-CLT-PS-Lab2.pdf` | PDF export of the executed notebook (submission; the assignment accepts PDF or HTML, and we submit PDF). |
| `EXPLANATION.md` | This file. |

## Things to change before submitting

1. **Team ID.** The assignment's team list was not in the paste, so `team_id` is set to `1` in the setup cell. Change it to your team's two-digit number and re-run all cells. Every number in the notebook depends on it.
2. **Team members.** The work breakdown lists only `Liybomyr Voskalo`. Replace `NameSurname2` and `NameSurname3` with your teammates and fill in the effort estimates.
3. **Re-export.** After changing the team ID, re-run the notebook and regenerate the PDF: `jupyter nbconvert --to notebook --execute --inplace RV-CLT-PS-Lab2.ipynb`, then `jupyter nbconvert --to webpdf RV-CLT-PS-Lab2.ipynb`.
4. **Re-check the numbers in the conclusions.** The markdown conclusions quote numbers for `team_id = 1`. With a different ID the printed outputs change, so the quoted values (and the Task 2 remark about the bisection disagreeing with the exact answer) must be updated to match.

## How the notebook was built

The original notebook was kept as the template. The four `# YOUR CODE GOES HERE` cells were replaced with a markdown explanation, the code and a short conclusion for each task. The notebook was then executed top to bottom so that every output is stored in the file.

Everything is vectorized with NumPy, as the hints ask. Random seeds are set from `team_id`.

## Task 1 – Hamming [7,4] code

- Messages are encoded over F2 with `G`, bits are flipped independently with probability `p = (2 + team_id/4)/100`, and the syndrome `z = rH mod 2` locates a single error. `m* = (r3, r5, r6, r7)`. The code asserts `GH = 0 mod 2`.
- `N = 10^6` transmissions. `p_hat` is the mean success indicator (LLN).
- The CI half-length is `eps = z_0.975 * s / sqrt(N)`, with `s` the sample standard deviation (CLT). The `N` needed for `eps <= 0.03` is computed from the same formula.
- The histogram of `k` (wrong bits in `m*`) is compared with `Bin(4, q)`. The conclusion is that it is not binomial, because decoding makes the bit errors dependent.

## Task 2 – Particle decay (Poisson)

- Cs-137: molar mass 137 g/mol, `T = 30.1` years, `m = (10 + team_id) * 1e-14` g. This gives `N_atoms`, `lambda = ln2/T` (per second) and `mu = N_atoms * lambda`, about 0.35.
- Sample means of `n` Poisson counts are compared with `N(mu, mu/n)` for `n = 5, 10, 50` using `K = 10 000` replications: e.c.d.f. plots and the maximal c.d.f. gap on a 200-point grid within `3 sigma`.
- The largest `n` with `P(S_n < 100) >= 0.95` is found by bisection on the simulated probability. It is compared with the Markov, Chernoff and CLT bounds and with the exact answer from the Poisson c.d.f. (`S_n ~ Poisson(n mu)`). For `team_id = 1` the bisection gives 238 because of Monte Carlo noise; the exact answer is 239.

## Task 3 – Geiger counter (exponential)

- `nu_1 = team_id/10 + 1` clicks per minute. The same CLT comparison is done for the sample mean of inter-click times for `n = 5, 10, 50`.
- Safe place: the event "fewer than 100 clicks in a minute" equals "the 100th click happens after 1 minute", so `P(S > 1) >= 0.95` with `S` the sum of 100 exponentials. The bounds on `nu = N nu_1` come from Markov (count `M_1`), Chernoff and CLT; `N` is then found by simulation over `N = 1..100` and confirmed with the exact Poisson c.d.f. of the click count (`N = 76`).

## Task 4 – Expectation fallacy

- 4.1(a): Jensen's inequality for the convex function `1/x`.
- 4.1(b): `1/X_bar` and `Y_bar` for `Y = 1/X`, `X ~ N(5, 10)`. The gap is explained by the small-`X` tail.
- 4.1(c): Q-Q plots and scatterplots for `(X, Y)` and `(X, Z)` with `Z = ln X + 5`.
- 4.2: The exact values are `E X = 1.5`, `Var X = 0.75`, `E Y = 3`, `Var Y = 7.5`, so the game is fair. Simulated estimates, a CLT 95% CI and the number of rounds needed to tell `E Y = 3` from `E Y = 2.9` (about 11 500) are given.

## Assumptions and simplifications

- `K = 10 000` replications for the e.c.d.f. and bisection tasks, and `N = 10^6` for the Hamming simulation. Both follow the hints or exceed the stated minimums.
- The 95% level uses `z = 1.96` (`norm.ppf(0.975)`).
- Results change with the seed and with `team_id`; the numbers quoted in the conclusions are for `team_id = 1`.
- Simulated probabilities near a threshold (Task 2 `n = 238`, Task 3 `N = 76`) have a Monte Carlo error of about 0.002–0.003, so the simulation alone only determines the threshold to about one unit. The exact Poisson c.d.f. is used to settle it.
