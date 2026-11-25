<h1 align="center">RecoBundles X</h1>

<p align="center">
  Recognizes white matter bundles in a whole-brain .trk tractogram with scilpy BundleSeg (RecoBundlesX) against a population atlas downloaded from Zenodo, and outputs a WMC classification and per-bundle .tck files.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.1000"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.1000-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Recognizes white matter bundles in a whole-brain .trk tractogram with scilpy BundleSeg (RecoBundlesX) against a population atlas downloaded from Zenodo, and outputs a WMC classification and per-bundle .tck files.**

Downloads the selected RecobundlesX/BundleSeg atlas from Zenodo, filters streamlines by length (minL-maxL), registers the atlas T1 to the subject T1 with an ANTs affine (antsRegistrationSyNQuick), and runs scil_tractogram_segment_with_bundleseg with multi-parameter voting. Use it for atlas-based bundle extraction from whole-brain tractograms. It needs internet access; the WMC index covers only recognized streamlines, not the full input tractogram.

| | |
|---|---|
| **Inputs** | `neuro/track/trk` · `neuro/anat/t1w` |
| **Outputs** | `neuro/wmc` · `neuro/tcks` |
| **Container** | `scilus/scilus:2.3.0` |
| **Run it** | [doi.org/10.25663/brainlife.app.1000](https://doi.org/10.25663/brainlife.app.1000) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `track` | `neuro/track/trk` | yes | Whole-brain tractogram in TrackVis (.trk) format. |
| `anat` | `neuro/anat/t1w` | yes | Subject T1-weighted anatomy used for ANTs affine registration to the atlas space. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `wmc` | `neuro/wmc` | White matter classification: recognized bundle assignments for each streamline. |
| `tcks` | `neuro/tcks` | Recognized bundles as a collection of MRtrix (.tck) files, one per bundle. |

<details>
<summary><b>Parameters</b> (6)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `atlas` | enum | `v3.0` | Atlas version to download from Zenodo at runtime. v3.0 = Zenodo 7950602 (RecobundlesX → BundleSeg transition); v2.2 = Zenodo 7729059; v2.0 = Zenodo 7562635. Options: `v3.0`, `v2.2`, `v2.0`. |
| `minL` | number | `20` | Minimum streamline length (mm). Streamlines shorter than this are discarded before bundle recognition. |
| `maxL` | number | `200` | Maximum streamline length (mm). Streamlines longer than this are discarded before bundle recognition. |
| `seeds` | number | `0` | Random number generator seed for reproducibility. |
| `minimal_vote_ratio` | number | `0.5` | Minimum fraction of multi-parameter runs in which a streamline must be recognized to be kept in the final bundle. |
| `processes` | number | `1` | Number of parallel threads. |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.1000](https://doi.org/10.25663/brainlife.app.1000), choose `neuro/track/trk`, `neuro/anat/t1w` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/wmc`, `neuro/tcks`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `scilus/scilus:2.3.0`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "track": "/path/to/track/track",
       "t1": "/path/to/anat/t1",
       "atlas": "v3.0",
       "minL": 20,
       "maxL": 200,
       "seeds": 0,
       "minimal_vote_ratio": 0.5,
       "processes": 1
   }
   ```

3. Run `./main`.

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- RecoBundles X. brainlife.io App. [doi:10.25663/brainlife.app.1000](https://doi.org/10.25663/brainlife.app.1000)
- Avants et al. A reproducible evaluation of ANTs similarity metric performance in brain image registration. *NeuroImage* 54:2033-2044 (2011). [doi:10.1016/j.neuroimage.2010.09.025](https://doi.org/10.1016/j.neuroimage.2010.09.025)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Gabriele Amorosino

#### MIT Copyright (c) 2026 Gabriele Amorosino

## License

See [LICENSE](LICENSE).
