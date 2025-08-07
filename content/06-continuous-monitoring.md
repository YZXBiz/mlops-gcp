# Continuous Monitoring

Continuous monitoring is the process of monitoring the effectiveness and efficiency of a model in production. It is essential to regularly and proactively verify that the model performance doesn't decay.

As the serving data changes over time, its properties start to deviate from the properties of the data that was used for training and evaluating the model. This leads to model performance degradation. In addition, changes or errors in upstream systems that produce the prediction requests might lead to changes in the serving data properties, consequently producing bad predictions.

## Continuous Monitoring Workflow

```{mermaid}
flowchart TD
    A[Model in Production] --> B[Capture Request-Response]
    B --> C[Store in Serving Logs]
    C --> D[Monitoring Engine]

    D --> E[Load Latest Logs]
    E --> F[Generate Schema]
    E --> G[Compute Statistics]

    F --> H[Schema Comparison]
    G --> I[Distribution Comparison]

    H --> J{Schema Skew?}
    I --> K{Distribution Skew?}

    J -->|Yes| L[Schema Alert]
    J -->|No| M[Schema OK]

    K -->|Yes| N[Distribution Alert]
    K -->|No| O[Distribution OK]

    M --> P[Ground Truth Available?]
    O --> P

    P -->|Yes| Q[Evaluate Performance]
    P -->|No| R[Monitor Efficiency Only]

    Q --> S{Performance Decay?}
    S -->|Yes| T[Performance Alert]
    S -->|No| U[Performance OK]

    L --> V[Trigger Actions]
    N --> V
    T --> V

    V --> W[Notify Owners]
    V --> X[Trigger Retraining]
    V --> Y[Update Dashboards]

    style A fill:#e1f5fe
    style D fill:#fff3e0
    style V fill:#ffcdd2
    style U fill:#c8e6c9
```

### Continuous Monitoring Process

A typical continuous monitoring process consists of the following steps:

1.  A sample of the request-response payloads is captured and stored.
2.  The monitoring engine periodically loads the latest inference logs, generates a schema, and computes statistics for the serving data.
3.  The engine compares the generated schema and statistics to a baseline to identify schema and distribution skews.
4.  If the ground truth for the serving data is available, the engine uses it to evaluate the model's predictive effectiveness.
5.  If anomalies are identified or performance is decaying, alerts can be sent to notify owners or trigger a new retraining cycle.

---

### Model Decay

::::{warning}
Effectiveness performance monitoring aims to detect **model decay**, which is usually defined in terms of data and concept drift.

- **Data drift** describes a growing skew between the training dataset and the production data.
- **Concept drift** is an evolving relationship between the input predictors and the target feature.
  ::::

## Drift Detection Framework

```{mermaid}
graph TD
    A[Production Data] --> B[Data Drift Detection]
    A --> C[Concept Drift Detection]

    B --> D[Schema Validation]
    B --> E[Distribution Analysis]
    B --> F[Feature Statistics]

    C --> G[Prediction Accuracy]
    C --> H[Target Relationship]
    C --> I[Model Performance]

    D --> J{Schema Changed?}
    E --> K{Distribution Shifted?}
    F --> L{Features Changed?}

    G --> M{Accuracy Dropped?}
    H --> N{Relationships Changed?}
    I --> O{Performance Degraded?}

    J -->|Yes| P[Schema Skew Alert]
    K -->|Yes| Q[Distribution Skew Alert]
    L -->|Yes| R[Feature Alert]

    M -->|Yes| S[Accuracy Alert]
    N -->|Yes| T[Concept Drift Alert]
    O -->|Yes| U[Performance Alert]

    P --> V[Remediation Actions]
    Q --> V
    R --> V
    S --> V
    T --> V
    U --> V

    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#fff8e1
    style V fill:#ffcdd2
```

Data drift can involve two types of skews:

- **Schema skew:** Training and serving data do not conform to the same schema.
- **Distribution skew:** The distribution of feature values for training data is significantly different from serving data.

::::{admonition} Additional Drift Detection Techniques
:class: tip
Besides identifying schema and distribution skews, other techniques for detecting data and concept drift include:

- Novelty and outlier detection
- Feature attributions change analysis
- Population Stability Index (PSI) monitoring
  ::::

::::{important} Ground Truth Collection
In some scenarios, your system can store ground truth for your serving data. For example:

- Whether a customer bought a product recommended by your model
- Actual demand compared to forecasted demand

This information can be used as true labels for continuous evaluation and further model training cycles.
::::

---

## Monitoring Dashboard Overview

```{mermaid}
graph LR
    A[Model Metrics] --> B[Performance Dashboard]
    C[Infrastructure Metrics] --> D[Efficiency Dashboard]
    E[Data Quality Metrics] --> F[Data Health Dashboard]

    G[CPU/GPU Usage] --> C
    H[Latency] --> C
    I[Throughput] --> C
    J[Error Rates] --> C

    K[Accuracy] --> A
    L[Precision/Recall] --> A
    M[F1 Score] --> A
    N[AUC-ROC] --> A

    O[Schema Validation] --> E
    P[Distribution Stats] --> E
    Q[Missing Values] --> E
    R[Data Volume] --> E

    style B fill:#c8e6c9
    style D fill:#e1f5fe
    style F fill:#fff3e0
```

::::{admonition} Monitoring Model Efficiency
:class: tip
Besides monitoring model effectiveness, monitoring model serving efficiency focuses on metrics like:

- Resource utilization (CPUs, GPUs, memory)
- Latency
- Throughput
- Error rates

Measuring these metrics is helpful for maintaining and improving system performance, and for predicting and managing costs.
::::

---

::::{admonition} Typical Assets
:class: seealso

- Anomalies detected in serving data during drift detection
- Evaluation metrics produced from continuous evaluation
  ::::

::::{admonition} Core MLOps Capabilities
:class: seealso

- Dataset & feature repository
- Model monitoring
- ML metadata & artifact repository
  ::::
