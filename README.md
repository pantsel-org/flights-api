# Steps

# Flights API CI/CD Workflows

This repository contains a set of GitHub Actions workflows to automate the validation, linting, breaking change detection, and deployment of the Flights API using OpenAPI specifications and Kong.

## Workflows Overview

### 1. OAS Linting (`.github/workflows/10-oas-linting.yaml`)
- **Purpose:** Lints the OpenAPI Specification (OAS) using Spectral and custom rulesets.
- **Key Steps:**
  - Sets the current date as an environment variable for artifact naming.
  - Checks out the repository.
  - Loads environment configuration.
  - Installs Spectral and extra rulesets.
  - Reads the OAS file and patches server information.
  - Runs Spectral linting and archives the results as artifacts.

### 2. OAS Breaking Changes (`.github/workflows/20-oas-breaking-changes.yaml`)
- **Purpose:** Detects breaking changes between the current and previous versions of the OAS using `oasdiff`.
- **Key Steps:**
  - Sets the current date as an environment variable.
  - Checks out the repository (with history for diffing).
  - Loads environment configuration.
  - Extracts the base OAS from the previous commit.
  - Runs breaking change detection with `oasdiff`.
  - Shows and saves breaking changes.
  - Optionally creates a GitHub issue if breaking changes are found.
  - Archives the breaking changes report as an artifact.

### 3. OAS to Kong Sync (`.github/workflows/30-oas-to-kong-sync.yaml`)
- **Purpose:** Converts the OAS to a Kong Gateway configuration and synchronizes it with a Konnect control plane.
- **Key Steps:**
  - Checks out the repository.
  - Installs `deck` (Kong's declarative configuration tool).
  - Loads environment configuration.
  - Converts OAS to Kong configuration.
  - Retrieves system account token from Vault.
  - Backs up the current Kong Gateway configuration.
  - Diffs and syncs the new configuration.
  - Backs up the updated configuration and archives artifacts.

### 4. OAS to Kong Terraform (`.github/workflows/32-oas-to-kong-tf.yaml`)
- **Purpose:** Converts the OAS to Kong configuration, then to Terraform resources for infrastructure-as-code management.
- **Key Steps:**
  - Checks out the repository.
  - Installs `deck`.
  - Loads environment configuration.
  - Converts OAS to Kong configuration.
  - Converts Kong configuration to Terraform using a custom action.
  - Passes required credentials and configuration as inputs and environment variables.

### 5. OAS to Kong Ingress Controller (`.github/workflows/34-oas-to-kong-kic.yaml`)
- **Purpose:** Converts the OAS to Kong configuration, then to Kong Ingress Controller (KIC) manifests for Kubernetes.
- **Key Steps:**
  - Checks out the repository.
  - Installs `deck`.
  - Loads environment configuration.
  - Converts OAS to Kong configuration.
  - Converts Kong configuration to KIC manifests.
  - (Placeholder for applying manifests to a Kubernetes cluster.)

---

## Usage

These workflows are designed to be triggered via `workflow_call` and can be composed in higher-level CI/CD pipelines. Each workflow expects certain inputs (such as the path to the OpenAPI spec, environment, control plane details, etc.) and manages its own dependencies and artifacts.

For more details on each workflow's inputs and outputs, refer to the corresponding YAML files in `.github/workflows/`.

---

## License

See [LICENSE](LICENSE) for details.
