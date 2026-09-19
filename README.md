# IEEE 754 Float Inspector

See exactly what a float stores — every bit, the exact decimal value it really holds, the rounding error, the ULP, and the neighbours on either side. Covers binary16, bfloat16, binary32 and binary64. Runs entirely in your browser.

**Live:** <https://float-inspector.slippylabs.com/>

## What it does

- **Number → bits:** type a decimal, a hex float (`0x1.921fb54442d18p+1`) or a special, and see what each of the four formats does with it — stored value, bit pattern, class, and whether your input survived exactly.
- The full, exact decimal expansion of what is stored. All 751 digits of the smallest binary64 subnormal, if that is what you asked for.
- Click a bit to flip it; step to the previous or next representable value; flip the sign or nudge the exponent.
- **Bits → number:** paste a hex or binary pattern and read it back.
- **Arithmetic lab:** watch `0.1 + 0.2` become `0.30000000000000004`, with the exact operands, the exact real result, and the single rounding that produced the answer.
- **Format limits:** range, precision, epsilon, round-trip digits, and the spacing near any magnitude you name.

## How it works

Everything is computed with exact rational arithmetic in `BigInt` — never via a double. A tool that explains rounding must not round on the way to the screen. Decimal input is rounded to the format with round-half-to-even over exact fractions, including subnormals and overflow-to-infinity, and the "shortest decimal" is found by searching for the fewest digits that round-trip.

For binary64 and binary32 the arithmetic lab also compares its own answer with what your CPU produces, and says so.

## Verification

Checked against an independent reference that binary-searches the encoding (bit patterns are monotone in value) and picks the nearer neighbour by comparing two exact `Fraction` distances — a different route to the same bits. Plus Python's own `float()` for parsing, `Decimal(float)` for the exact expansions, `math.ulp` and `math.nextafter` for spacing and neighbours, and numpy's `finfo` for the format limits. Round-trip decimals are checked for *minimality*, not just correctness.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/float-inspector.slippylabs.com.git
cd float-inspector.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
