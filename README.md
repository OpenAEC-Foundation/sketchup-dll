# sketchup-dll

Pure-Rust reader for SketchUp SKP files, classic and ZIP/VFF containers. The crate is `skp-reader`.

```rust,no_run
let bytes = std::fs::read("model.skp")?;
let geometry = skp_reader::read(&bytes).map_err(std::io::Error::other)?;
# Ok::<(), std::io::Error>(())
```

`read(&[u8])` returns meshes, one per tag/material batch. The crate owns its format output types.
Applications convert that output to their own scene representation. This library has no
dependency on a shared application model, loader registry, UI or GPU.

## Validation

`cargo test` runs the self-contained tests. `cargo check --target wasm32-unknown-unknown` checks browser support.

## Current coverage and limitations

Partial reader ported from openskp 1.3/1.4.0: classic SketchUp 3–2020 MFC archives and
2021+ ZIP/VFF containers, triangulated faces, placed groups/components, tags, hidden tags,
material colours and dynamic attributes. Inches become metres, Z stays up.
Textures use their average colour; texture mapping and soft-edge smoothing are not implemented.
SKP support is partial; this library does not claim complete compatibility.

Large corpus fixtures stay in the viewer repository. To run its 21-file regression suite,
set `SKP_TEST_MODELS` to the absolute `test-models` directory of [open-bim-suite](https://github.com/OpenAEC-Foundation/open-bim-suite) and run
`cargo test --features corpus-tests`. `skp_dir_survey` is an ignored diagnostic that uses `SKP_DIR`.

## Attribution

The parsing implementation is a Rust port of [openskp](https://github.com/iamahsanmehmood/openskp),
MIT, copyright Ahsan Mehmood. Preserve the upstream MIT license in THIRD_PARTY_LICENSES.md.
No sample models are distributed with this crate.

## Use in another application

Add this repository as a Cargo Git dependency pinned to a commit, then call `read`.
These repositories are private; Git requires read access. There are no private dependencies
on other OpenAEC repositories.
