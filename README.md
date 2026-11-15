# Superconducting qubit chip: an end-to-end design study

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FarukSamiBilgin/superconducting-qubit-chip-design/blob/main/notebooks/superconducting_qubit_project.ipynb)

**Faruk Sami Bilgin** (Independent Researcher) · ORCID [0009-0001-9064-4410](https://orcid.org/0009-0001-9064-4410)

**Status:** Learning / design study in a single Colab notebook (not a publication). The notebook is
committed **without saved outputs**; all figures and numbers are produced by running it. No results
are quoted in this README.

## Question

How far can one get, with open-source tools only, from the layout of a single fixed-frequency
transmon to a simulated logical qubit? The notebook chains the standard design steps (layout, lumped
circuit model, Hamiltonian, readout, coherence, gates, multi-qubit layout, error correction) so that
each stage takes its inputs from the previous one.

## Method (notebook phases)

| Phase | Content | Tools |
|---|---|---|
| 0 | Environment setup and version check | pip |
| 1 | Single TransmonPocket + quarter-wave meandered readout resonator (4.3 mm) | Quantum Metal (Qiskit Metal) |
| 2 | Lumped-oscillator model → E_C, E_J, f01, anharmonicity, coupling g (charge-basis diagonalisation) | NumPy |
| 3a | Coupled spectrum, dispersive shift χ and simulated readout S21 for qubit in 0 and 1 | scqubits |
| 3b-i | T1 / T2 decay and Ramsey (Lindblad master equation) | QuTiP |
| 3b-ii | DRAG X gate on a 3-level transmon; leakage to 2 vs gate time, with and without DRAG | QuTiP |
| 4 | Two-qubit CZ via the 11 ↔ 02 resonance: population return and conditional phase | QuTiP |
| 5 | 5-qubit linear chip (D0-A0-D1-A1-D2), 5 readout resonators, 4 couplers, GDS export | Quantum Metal |
| 6 | Repetition-code memory, logical error vs physical error for d = 3, 5, 7 | Stim + PyMatching |

## Inputs and honest limitations

- **Lumped values are representative, not extracted.** Phase 2 uses C_Σ = 88 fF, C_g = 6 fF,
  C_r = 400 fF, L_J = 10 nH, f_r = 7.0 GHz, which the notebook states are representative values for
  this geometry used *in place of* an Ansys Q3D capacitance extraction (not available in Colab). The
  layout of Phase 1 and the parameters of Phase 2 are therefore not linked by an EM simulation.
- T1 = 50 µs and T2 = 40 µs in Phase 3b-i are **assumed inputs**, not predictions.
- The CZ simulation (Phase 4) uses its own fixed parameters (f1 = 5.135 GHz, α = −245 MHz,
  J = 12 MHz) rather than the Phase 2 output.
- The gate fidelities stored in Phase 4 (`f_1q = f_2q = 0.999`) are **placeholders**, not computed.
- Phase 6 uses Stim's generated circuit-level repetition code (bit-flip only) with a uniform
  depolarising/measurement-flip noise parameter p; it is not a surface code and not a noise model
  derived from the device.
- GDS export is wrapped in try/except because the renderer can fail in some Quantum Metal versions.
- The notebook has not been re-executed for this repository.

## Reproducing

Open the notebook in Colab (badge above) and run the cells in order. After Phase 0a, restart the
runtime (quantum-metal pins NumPy < 2 while qutip/scqubits/stim need NumPy 2.x; the cell
force-reinstalls NumPy 2.3.5), then continue from Phase 0b. Locally: `pip install -r requirements.txt`.

## Research note (Turkish)

[`docs/tr/kuantum_cip_kitabi.html`](docs/tr/kuantum_cip_kitabi.html): *Süperiletken Kuantum Çip —
Sıfırdan Bir Araştırma Notu*, a long-form Turkish explainer of the same project written for readers
without a physics background (can be served with GitHub Pages). It also covers parts that are **not**
in this notebook (surface code, flux-tunable SQUID transmon, a qubit emulator, SQuADDS inverse design).
Its embedded figures come from earlier project runs and are not all consistent with the notebook or
with its own text (e.g. its transmon-level figure is labelled E_J/E_C = 68, f01 = 4.91 GHz, while the
text quotes 74 and 5.135 GHz; its "~99.9%" single-qubit fidelity corresponds to the placeholder value
above). For that reason none of its numbers are quoted here as results.

## Related work by the author

- Twisted hBN preprint: <https://doi.org/10.5281/zenodo.24201814>
- Twisted MoS₂ hole-spin coherence preprint: <https://www.alphaxiv.org/abs/2611.twisted-mos2-hole-spin-coherence>
- Moiré microwave loss preprint: <https://doi.org/10.5281/zenodo.24204124>
- josim-cpr-bridge: <https://github.com/FarukSamiBilgin/josim-cpr-bridge>

## Citation

```bibtex
@software{bilgin2026scqubitchip,
  author = {Bilgin, Faruk Sami},
  title  = {Superconducting qubit chip: an end-to-end design study},
  year   = {2026},
  url    = {https://github.com/FarukSamiBilgin/superconducting-qubit-chip-design}
}
```

See [`CITATION.cff`](CITATION.cff). Code: MIT ([`LICENSE`](LICENSE)); text, figures, data: CC BY 4.0
([`LICENSE-CONTENT`](LICENSE-CONTENT)).

AI assistance: AI coding assistants were used for code development and for adversarial review. This repository has no automated test suite; results are produced by running the notebook, and the author is responsible for every claim.
