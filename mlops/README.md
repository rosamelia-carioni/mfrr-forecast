MLOps pipeline for mfrr activation forecasting: training, validation, deployment, and inference.

## Workflows
Jobs are declared under `resources/` and wired in `databricks.yml` to call stub entry points in `src/entry_points.py`.

| Workflow | Description |
| --- | --- |
| `training` | Train a candidate model and log to MLflow (stub) |
| `validation` | Evaluate metrics and gate promotion (stub) |
| `deployment` | Promote the validated model to serving/batch (stub) |
| `inference` | Run batch inference and persist predictions (stub) |
| `publish-results` | Push predictions to downstream tables/APIs (stub) |
| `ml-pipeline` | Orchestrated pipeline: train → validate → deploy → infer → publish |

## Quick Start

```bash
cd mlops
uv venv
uv sync --dev

# Run a local stub step (replace with real logic as implemented)
uv run python src/entry_points.py train --catalog ccm_dev --schema mfrr_forecast

# Validate bundle structure once DATABRICKS_HOST/PROFILE are set
databricks bundle validate --target dev
```

## Development

```bash
# Lint
uv run ruff check src tests

# Test
uv run pytest tests -q

# Run orchestrated bundle (after filling real code/config)
databricks bundle run ml_pipeline --target dev
```

## Project Structure

```
mlops/
	databricks.yml
	resources/
		training-workflow-resource.yml
		validation-workflow-resource.yml
		deployment-workflow-resource.yml
		inference-workflow-resource.yml
		publish-results-workflow-resource.yml
		orchestrated-workflow-resource.yml
	src/
		entry_points.py
		training/
		validation/
		deployment/
		inference/
		feature_selection/
		publish_results/
		utils/
	tests/
		integration/
		unit/
```

## Configuration

Key variables in `databricks.yml`:

```bash
export DATABRICKS_HOST="https://adb-*****.**.azuredatabricks.net"
export DATABRICKS_PROFILE="ccm-*****-dev"
```

Targets inherit the shared variables above:

| Target | Catalog | Schema |
| --- | --- | --- |
| dev | `ccm_dev` | `mfrr_forecast` |
| staging | `ccm_stg` | `mfrr_forecast` |
| prod | `ccm_prod` | `mfrr_forecast` |

## Entrypoints

- Console scripts (`pyproject.toml`) map to `src/entry_points.py` functions: `run_forecasting`, `run_validation`, `run_deployment`, `run_inference`, `publish_results`.
- Each command currently prints a stub message; replace with real training/validation/deployment/inference/publish logic.

## Model Details

- **Target**: 
- **Output**: 
- **Zones**: SE_2, SE_3,
- **Tracking**: MLflow experiments in `/Shared/ccm_*/mfrr_forecast*`
