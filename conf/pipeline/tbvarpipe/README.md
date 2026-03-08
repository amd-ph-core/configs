Pipeline Config Runbook
================
amd-ph-core/tbvarpipe

- [Config Files](#config-files)
- [Commands](#commands)
  - [Pipeline Workflows](#pipeline-workflows)
    - [Varpipe with Environment Overlay](#varpipe-with-environment-overlay)
    - [Container Inspection](#container-inspection)
- [Environment Overlay](#environment-overlay)

Pipeline-specific configs are auto-loaded via `custom_config_base`.
Users provide an environment overlay config with `-c` to set
`static_assets` and environment-specific settings (AWS Batch, etc.).

# Config Files

| File | Description |
|------|-------------|
| `params.config` | Asset path resolution from `static_assets` |
| `containers.config` | Per-process container image overrides (`cdc-amd/`) |
| `modules.config` | Per-process CPU, memory, and time tuning |
| `resource_labels.config` | AMDP workspace/workflow/run/user labels |

# Commands

## Pipeline Workflows

### Varpipe with Environment Overlay

```bash
nextflow run amd-ph-core/tbvarpipe -r 1.2.0 \
  -profile docker,amd \
  -c my-environment.config \
  --input samplesheet.csv \
  --outdir results
```

### Container Inspection

```bash
nextflow inspect main.nf \
  -profile docker,amd \
  -c my-environment.config \
  --input samplesheet.csv \
  --outdir results \
  -format json
```

# Environment Overlay

Create an environment overlay config that sets `static_assets` and
any environment-specific settings. Example for AMD dev:

```groovy
params {
    static_assets = 's3://amdp-dev-static-assets/amdp-dev-ncezid-tb/static-assets/varpipe-static-assets-1.0.0'
}

aws {
    batch {
        cliPath         = '/home/ec2-user/miniconda/bin/aws'
        maxSpotAttempts  = 3
    }
    region = 'us-east-1'
}

process {
    executor      = 'awsbatch'
    queue         = 'amdp-dev-batch-ec2-job-queue'
}
```

*Notes:* Container images are hosted at `quay.io/us-cdcgov/cdc-amd/`.
The pipeline sets `docker.registry = 'quay.io/us-cdcgov'` so container
directives use the short form `cdc-amd/<tool>:<tag>`.
