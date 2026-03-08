# amd-ph-core/configs

Configuration profiles and pipeline configs for [amd-ph-core](https://github.com/amd-ph-core) Nextflow pipelines.

## Quick Start

```bash
nextflow run amd-ph-core/tbvarpipe -r 1.2.0 \
  -profile docker,amd \
  -c my-environment.config \
  --input samplesheet.csv --outdir results
```

The `-profile amd` loads container security settings. Pipeline-specific configs (containers, resource tuning, asset paths, AMDP labels) are auto-loaded via `custom_config_base`.

Set `static_assets` in your environment overlay config to resolve all reference file paths automatically.

## Repository Structure

```
amd-ph-core/configs/
├── amd_ph_core_custom.config              # Profile loader (auto-loaded by pipelines)
├── pipeline/
│   └── tbvarpipe.config                   # Auto-loaded master (includes sub-configs)
└── conf/
    └── pipeline/
        └── tbvarpipe/
            ├── params.config         # Asset path resolution from static_assets
            ├── containers.config     # Per-process container overrides
            ├── modules.config        # Per-process resource tuning
            ├── resource_labels.config # AMDP cost tracking labels
            └── README.md             # Pipeline config runbook
```

## How It Works

amd-ph-core pipelines set `custom_config_base` to this repository, which auto-loads two config files:

1. **`amd_ph_core_custom.config`** — defines the `amd` profile: read-only root filesystem, non-root `default` user, scratch directories
2. **`pipeline/tbvarpipe.config`** — includes all pipeline-specific sub-configs: asset path defaults, container overrides, resource tuning, AMDP resource labels

Users provide an environment overlay config with `-c` to set `static_assets` and any environment-specific settings (AWS Batch, S3 paths, etc.).

## Pipelines

| Pipeline | Config | Runbook |
|----------|--------|---------|
| [tbvarpipe](https://github.com/amd-ph-core/tbvarpipe) | `pipeline/tbvarpipe.config` | [README](conf/pipeline/tbvarpipe/README.md) |

## Public Domain Standard Notice
This repository constitutes a work of the United States Government and is not
subject to domestic copyright protection under 17 USC § 105. This repository is in
the public domain within the United States, and copyright and related rights in
the work worldwide are waived through the
[MIT No Attribution (MIT-0)](https://opensource.org/license/mit-0) license.
All contributions to this repository will be released under the MIT-0 license. By
submitting a pull request you are agreeing to comply with this waiver of
copyright interest.

## License Standard Notice
This repository is licensed under the
[MIT No Attribution (MIT-0)](https://opensource.org/license/mit-0) license.

This source code in this repository is free: you can redistribute it and/or modify it under
the terms of the MIT-0 license.

This source code in this repository is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the [LICENSE](LICENSE) file for more details.

The source code forked from other open source projects will inherit its license.

## Privacy Standard Notice
This repository contains only non-sensitive, publicly available data and
information. All material and community participation is covered by the
[Disclaimer](DISCLAIMER.md)
and [Code of Conduct](code-of-conduct.md).
For more information about CDC's privacy policy, please visit [http://www.cdc.gov/other/privacy.html](https://www.cdc.gov/other/privacy.html).

## Contributing Standard Notice
Anyone is encouraged to contribute to the repository by [forking](https://help.github.com/articles/fork-a-repo)
and submitting a pull request. (If you are new to GitHub, you might start with a
[basic tutorial](https://help.github.com/articles/set-up-git).) By contributing
to this project, you grant a world-wide, royalty-free, perpetual, irrevocable,
non-exclusive, transferable license to all users under the terms of the
[MIT No Attribution (MIT-0)](https://opensource.org/license/mit-0) license.

All comments, messages, pull requests, and other submissions received through
CDC including this GitHub page may be subject to applicable federal law, including but not limited to the Federal Records Act, and may be archived. Learn more at [http://www.cdc.gov/other/privacy.html](http://www.cdc.gov/other/privacy.html).

## Records Management Standard Notice
This repository is not a source of government records, but is a copy to increase
collaboration and collaborative potential. All government records will be
published through the [CDC web site](http://www.cdc.gov).

## Additional Standard Notices
Please refer to [CDC's Template Repository](https://github.com/CDCgov/template) for more information about [contributing to this repository](https://github.com/CDCgov/template/blob/main/CONTRIBUTING.md), [public domain notices and disclaimers](https://github.com/CDCgov/template/blob/main/DISCLAIMER.md), and [code of conduct](https://github.com/CDCgov/template/blob/main/code-of-conduct.md).
