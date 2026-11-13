# clone-lin-maker-vat-flux

Experimental **LIN** clone of MakerDAO / Sky `Vat.flux` — the unlocked
collateral (gem) transfer kernel (unsigned `_sub`/`_add` of `gem[ilk][src]`
and `gem[ilk][dst]` after src-only `wish`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/vat.sol` sha256 `9016fd94f0f1b014abf184edab86a3f5843042b17e3734407e0f0fabb920f49f` blob `ff90bbed5b36e54318bd5461cbf32e4167df2b28`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_vat_flux.lin`](src/lin_maker_vat_flux.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_vat_flux_c11.c`](test/oracles/maker_vat_flux_c11.c)
- **Proofs (lin-open only):** `examples/maker_vat_flux/` and `make maker-vat-flux-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production vat replacement.

Named goldens: SAFE_XFER `src=5e9 dst=3e9 wad=1e9 wish=1` → **flux_ok=1**, leftovers `4e9/4e9`, conserved;
NO_WISH → **0**; UNDERFLOW wad>src → **0**; OVERFLOW `7e18+3e18` dst add fail-closes → **0**;
zero wad is a no-op **ok=1**; full drain src→0 dst=8e9; no `live`, no `wards` (src-only wish);
dst cannot refuse incoming gem (`dst_consent_required=0`); naive i64 `7e18+3e18` wraps, checked u_add fail-closes.

## Reproduce proofs (in lin-open)

```bash
make maker-vat-flux-proof
# or: python3 test/prove_maker_vat_flux_external.py
```

## Not claimed

Uint256 / mainnet gem-wad parity.
LIN replacing Maker, Sky, vat, or the EVM. TVL or dollar savings. zk soundness.
`wish` mappings as hidden storage. `vat.move` (dai rad). `vat.fork` (see clone-lin-maker-vat-fork).
