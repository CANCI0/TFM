# TFM — Exploración, detección y mitigación de sesgos en modelos de IA

Pipeline completo para:
1. Auditoría baseline de sesgo (CrowS-Pairs + BBQ).
2. Curación de dataset SFT.
3. Mitigación con SFT + QLoRA.
4. Evaluación de impacto (alignment tax).

## Estructura

```
./
  00_project_setup.ipynb        # Validación de entorno y prerrequisitos
  01_download_benchmarks.ipynb  # Descarga de CrowS-Pairs y BBQ
  02_baseline_audit.ipynb       # Auditoría de sesgo (LLM calls via OpenRouter/Ollama)
  03_baseline_analysis.ipynb    # Visualización y análisis de resultados
  04_sft_dataset_curation.ipynb # Generación y curación del dataset SFT
  05_qlora_sft.ipynb            # Fine-tuning QLoRA con PEFT/TRL
  06_impact_evaluation.ipynb    # Re-evaluación de sesgo + alignment tax
  07_final_report.ipynb         # Síntesis ejecutiva y exportación
data/
  raw/hf_datasets/   # CrowS-Pairs y BBQ descargados localmente
  processed/         # Dataset SFT curado (train/valid)
                    # + auditoría de rewrites (`sft_rewrite_audit.csv`)
outputs/
  eval/       # Métricas baseline (*_metrics.xlsx)
  finetuned/  # Checkpoint fine-tuned
  analysis/   # Tablas y figuras listas para memoria/defensa
```

## Requisitos (uv)

```bash
uv sync
```

> Para `meta-llama/Llama-3.1-8B-Instruct` se necesita acceso autorizado en Hugging Face (`HF_TOKEN`).
> `datasets` está fijado a `2.21.0` por compatibilidad con benchmarks que requieren `trust_remote_code`.
> Para auditar modelos comerciales vía OpenRouter, exportar `OPENROUTER_API_KEY`.

## Ejecución (pipeline de notebooks)

Ejecuta los notebooks en orden desde Jupyter Lab / VS Code:

| Notebook | Acción |
|---|---|
| `00_project_setup` | Valida entorno, dependencias y config |
| `01_download_benchmarks` | Descarga CrowS-Pairs y BBQ a `data/raw/` |
| `02_baseline_audit` | Auditoría de sesgo con OpenRouter o Ollama |
| `03_baseline_analysis` | Análisis y visualizaciones del baseline |
| `04_sft_dataset_curation` | Genera y curada el dataset SFT |
| `05_qlora_sft` | Entrena Llama 3.1 8B Instruct con QLoRA |
| `06_impact_evaluation` | Evalúa impacto del fine-tuning |
| `07_final_report` | Reporte final comparativo |