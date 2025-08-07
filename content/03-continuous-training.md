# Continuous Training

The continuous training process is about orchestrating and automating the execution of training pipelines.

## Continuous Training Pipeline

```{mermaid}
flowchart TD
    A[Retraining Trigger] --> B{Trigger Type}
    B -->|Scheduled| C[Time-based Job]
    B -->|Event-driven| D[New Data Available]
    B -->|Manual| E[Ad-hoc Request]

    C --> F[Data Ingestion]
    D --> F
    E --> F

    F --> G[Data Validation]
    G --> H{Data Valid?}
    H -->|No| I[Alert & Stop]
    H -->|Yes| J[Data Transformation]

    J --> K[Model Training & Tuning]
    K --> L[Model Evaluation]
    L --> M[Model Validation]
    M --> N{Model Meets Criteria?}

    N -->|No| O[Log Results & Alert]
    N -->|Yes| P[Model Registration]
    P --> Q[Update Metadata Repository]

    Q --> R{Include Deployment?}
    R -->|No| S[End Pipeline]
    R -->|Yes| T[Deploy Model]
    T --> U[Monitor Deployment]

    style A fill:#e1f5fe
    style G fill:#fff3e0
    style M fill:#fff3e0
    style P fill:#c8e6c9
    style H fill:#ffecb3
    style N fill:#ffecb3
```

::::{admonition} Retraining Frequency
:class: tip
How frequently you retrain your model depends on your use case and on the business value and cost of doing so. For example, a recommendation system on a shopping site may require frequent retraining to capture new trends, while an image classification model may not.
::::

Each run of the pipeline can be triggered in several ways:

- Scheduled runs
- Event-driven runs (e.g., new data becomes available, model decay is detected)
- Ad hoc runs (manual invocation)

---

### Continuous Training Workflow

:::{note}
Typical ML training pipelines have workflows that include the following steps:

1.  **Data ingestion:** Extracting training data from the source.
2.  **Data validation:** Ensuring the model is not trained using skewed or corrupted data.
3.  **Data transformation:** Splitting data and engineering features.
4.  **Model training and tuning:** Producing the best possible model.
5.  **Model evaluation:** Assessing the performance of the model.
6.  **Model validation:** Ensuring the model meets performance criteria.
7.  **Model registration:** Storing the validated model in a model registry.
    :::

## Data and Model Validation Flow

```{mermaid}
graph TD
    A[New Training Data] --> B[Schema Validation]
    B --> C{Schema Matches?}
    C -->|No| D[Schema Anomaly Alert]
    C -->|Yes| E[Statistical Validation]
    E --> F{Distribution Normal?}
    F -->|No| G[Distribution Anomaly Alert]
    F -->|Yes| H[Feature Validation]
    H --> I{Features Valid?}
    I -->|No| J[Feature Anomaly Alert]
    I -->|Yes| K[Proceed to Training]

    K --> L[Train Model]
    L --> M[Evaluate Model]
    M --> N[Compare Performance]
    N --> O{Performance Acceptable?}
    O -->|No| P[Model Validation Failed]
    O -->|Yes| Q[Check Fairness & Bias]
    Q --> R{Fairness OK?}
    R -->|No| S[Bias Alert]
    R -->|Yes| T[Register Model]

    style A fill:#e1f5fe
    style T fill:#c8e6c9
    style C fill:#fff3e0
    style F fill:#fff3e0
    style I fill:#fff3e0
    style O fill:#fff3e0
    style R fill:#fff3e0
```

An orchestrated and automated training pipeline mirrors the steps of the typical data science process but with a stronger emphasis on **data validation** and **model validation** as gatekeepers for the overall process.

::::{warning} Data Validation Importance
The data validation step can detect when data anomalies start occurring. These anomalies can include:

- New features appearing
- New feature domains
- Features that have disappeared
- Drastic changes in feature distributions

The data validation step works by comparing the new training data to the expected data schema and reference statistics.
::::

::::{important} Model Validation
The model validation phase detects anomalies when a lack of improvement or even degradation in performance of new model candidates is observed. The pipeline can apply complex validation logic including:

- Several evaluation metrics
- Sensitivity analysis based on specific inputs
- Calibration analysis
- Fairness indicators
  ::::

::::{important} Lineage Analysis
An important aspect of continuous training is tracking. Pipeline runs must track generated metadata and artifacts to enable debugging, reproducibility, and lineage analysis. Lineage analysis allows you to:

- Retrieve the hyperparameters used during training.
- Retrieve all evaluations performed by the pipeline.
- Retrieve processed data snapshots.
- Retrieve data summaries (descriptive statistics, schemas, etc.).
  ::::

::::{admonition} Unified Training and Deployment
:class: tip
An automated pipeline can include deployment steps, effectively acting as a unified, end-to-end training and deployment pipeline. In some use cases where models are trained and deployed several times per day, additional validations are built into the pipeline such as:

- Model size validation
- Serving runtime validation (for dependencies and accelerators)
- Serving latency evaluation
  ::::

---

::::{admonition} Typical Assets
:class: seealso

- A trained and validated model stored in the model registry
- Training metadata and artifacts (pipeline parameters, data stats, evaluation metrics, etc.)
  ::::

::::{admonition} Core MLOps Capabilities
:class: seealso

- Dataset & feature repository
- ML metadata & artifact repository
- Data processing
- Model training
- Model evaluation
- ML pipelines
- Model registry
  ::::
