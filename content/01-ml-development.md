# ML Development

Experimentation is the core activity in ML development, where your data scientists can rapidly try several ideas for data preparation and ML modeling.

## ML Development Workflow

```{mermaid}
flowchart TD
    A[ML Use Case Definition] --> B{Questions Answered?}
    B -->|No| C[Define Task & Metrics]
    C --> B
    B -->|Yes| D[Data Discovery & Exploration]
    D --> E[Data Preparation & Feature Engineering]
    E --> F[Model Prototyping & Validation]
    F --> G{Satisfactory Results?}
    G -->|No| D
    G -->|Yes| H{Need Regular Retraining?}
    H -->|No| I[Submit to Model Registry]
    H -->|Yes| J[Implement Training Pipeline]
    J --> K[Version Control & CI/CD]
    K --> L[Deploy Training Pipeline]

    style A fill:#e1f5fe
    style I fill:#c8e6c9
    style L fill:#c8e6c9
    style G fill:#fff3e0
    style H fill:#fff3e0
```

::::{admonition} Pre-Experimentation Checklist
:class: tip
Experimentation starts when the ML use case is well defined, meaning that the following questions have been answered:

- What is the task?
- How can we measure business impact?
- What is the evaluation metric?
- What is the relevant data?
- What are the training and serving requirements?
  ::::

::::{admonition} Infrastructure Considerations
:class: note
MLOps processes take place on an integrated ML platform that has the required development and operations capabilities. Infrastructure engineers can provision this type of platform in different environments (development, test, staging, production) using configuration management and infrastructure-as-code (IaC) tools like Terraform.
::::

Experimentation aims to arrive at an effective prototype model for the ML use case at hand. In addition to experimentation, data scientists need to formalize their ML training procedures. They do this by implementing an end-to-end pipeline, so that the procedures can be operationalized and run in production.

## Experiment Tracking Architecture

```{mermaid}
graph LR
    A[Data Scientists] --> B[Experimentation Environment]
    B --> C[Version Control System]
    B --> D[ML Metadata Repository]
    B --> E[Dataset & Feature Repository]

    F[Experiment Run] --> G[Code Version]
    F --> H[Model Architecture]
    F --> I[Hyperparameters]
    F --> J[Data Splits]
    F --> K[Evaluation Metrics]

    G --> C
    H --> D
    I --> D
    J --> E
    K --> D

    style A fill:#e3f2fd
    style B fill:#fff3e0
    style C fill:#f3e5f5
    style D fill:#e8f5e8
    style E fill:#fff8e1
```

:::{note}
During experimentation, data scientists typically perform the following steps:

1.  **Data discovery, selection, and exploration.**
2.  **Data preparation and feature engineering,** using interactive data processing tools.
3.  **Model prototyping and validation.**
    :::

Performing these iterative steps can lead data scientists to refining the problem definition. For example, your data scientists or researchers might change the task from regression to classification, or they might opt for another evaluation metric. The primary source of development data is the dataset and feature repository.

In general, the key success aspects for this process are experiment tracking, reproducibility, and collaboration.

::::{important} What to Track for Reproducibility
To be able to reproduce an experiment, your data science team needs to track configurations for each experiment, including the following:

- A pointer to the version of the training code in the version control system.
- The model architecture and pretrained modules that were used.
- Hyperparameters, including trials of automated hyperparameter tuning and model selection.
- Information about training, validation, and testing data splits that were used.
- Model evaluation metrics and the validation procedure that was used.
  ::::

::::{admonition} Collaboration Benefits
:class: tip
When data scientists begin working on an ML use case, it can save them time if they can find previous experiments that have similar use cases and reproduce the results of those experiments. Data scientists can then adapt those experiments to the task at hand. They also need to compare various experiments and different runs of the same experiment to understand factors that lead to changes in model behavior and performance improvements.
::::

If there is no need to retrain the model on a regular basis, then the produced model at the end of the experimentation is submitted to the model registry.

However, in most cases, ML models need to be retrained on a regular basis when new data is available or when the code changes. In this case, the output of the ML development process is not the model to be deployed in production. Instead, the output is the **implementation of the continuous training pipeline** to be deployed to the target environment.

::::{warning} Version Control Requirements
Whether you use code-first, low-code, or no-code tools to build continuous training pipelines, the development artifacts, including source code and configurations, must be version controlled (for example, using Git-based source control systems). This enables standard software engineering practices like code review, code analysis, and automated testing.
::::

::::{admonition} Data Engineering Pipeline Requirements
:class: tip
Experimentation activities usually produce novel features and datasets. If the new data assets are reusable in other ML and analytics use cases, they can be integrated into the feature and dataset repository through a data engineering pipeline. Therefore, a common output of the experimentation phase is the requirements for upstream data engineering pipelines.
::::

---

::::{admonition} Typical Assets
:class: seealso

- Notebooks for experimentation and visualization
- Metadata and artifacts of the experiments
- Data schemas
- Query scripts for the training data
- Source code for data validation, transformation, model creation, training, and evaluation
- Source code for the training-pipeline workflow and tests
  ::::

::::{admonition} Core MLOps Capabilities
:class: seealso

- Dataset & feature repository
- Data processing
- Experimentation
- Model training
- Model registry
- ML metadata & artifact repository
  ::::
