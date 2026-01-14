# helm-vllm

A Crossplane Configuration package that installs the vLLM production stack Helm chart with a minimal, stable interface.

## Overview

`helm-vllm` renders a single Helm release for vLLM production stack. It exposes only the inputs needed
for chart values, namespace, and release name, keeping the interface stable while allowing full Helm overrides.

## Features

- **Minimal Helm interface**: values and overrideAllValues with stable defaults
- **Predictable naming**: defaults to `<clusterName>-vllm` in the `vllm` namespace
- **GitOps friendly**: ships a `.gitops/` deploy chart

## Prerequisites

- Crossplane installed in the cluster
- Crossplane providers:
  - `provider-helm` (>=v1.0.6)
- Crossplane function:
  - `function-auto-ready` (>=v0.6.0)
- GPU resources available in the cluster (NVIDIA recommended)
- NVIDIA Kubernetes Device Plugin

## Quick Start

```yaml
apiVersion: pkg.crossplane.io/v1
kind: Configuration
metadata:
  name: helm-vllm
spec:
  package: ghcr.io/hops-ops/helm-vllm:latest
```

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: VLLM
metadata:
  name: vllm
  namespace: example-env
spec:
  clusterName: example-cluster
  values:
    servingEngineSpec:
      runtimeClassName: nvidia
      modelSpec:
        - name: llama-3
          repository: meta-llama/Llama-3.2-1B-Instruct
          gpu: 1
```

## Development

```bash
make render
make validate
make test
```
