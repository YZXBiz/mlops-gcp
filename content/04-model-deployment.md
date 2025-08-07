# Model Deployment

After a model has been trained, validated, and added to the model registry, it is ready for deployment. During this process, the model is packaged, tested, and deployed to a target serving environment.

## Progressive Delivery Workflow

```{mermaid}
flowchart TD
    A[Model Registry] --> B[Model Governance Check]
    B --> C{Approved?}
    C -->|No| D[Return for Fixes]
    C -->|Yes| E[Package Model]

    E --> F[CI Pipeline]
    F --> G[Interface Testing]
    F --> H[Infrastructure Compatibility]
    F --> I[Latency Testing]

    G --> J{CI Tests Pass?}
    H --> J
    I --> J

    J -->|No| K[Fix Issues]
    J -->|Yes| L[CD Pipeline]

    L --> M[Deploy to Staging]
    M --> N[Smoke Testing]
    N --> O{Smoke Tests Pass?}

    O -->|No| P[Rollback]
    O -->|Yes| Q[Deploy to Production]

    Q --> R[Progressive Delivery]
    R --> S[Canary Deployment]
    R --> T[Blue-Green Deployment]
    R --> U[Shadow Deployment]

    S --> V[A/B Testing]
    T --> V
    U --> V

    V --> W[Monitor Results]
    W --> X{Performance Good?}
    X -->|No| Y[Rollback to Previous]
    X -->|Yes| Z[Full Release]

    style A fill:#e1f5fe
    style E fill:#fff3e0
    style Q fill:#fff8e1
    style Z fill:#c8e6c9
    style C fill:#ffecb3
    style J fill:#ffecb3
    style O fill:#ffecb3
    style X fill:#ffecb3
```

::::{admonition} Deployment Methods
:class: tip
The model deployment process can range from a streamlined, automated process using no-code/low-code solutions to a more complex CI/CD routine for greater control.
::::

---

### CI/CD for Model Deployment

In a CI/CD approach, the system reads the model serving code from a source repository, fetches the model from the model registry, and then integrates, builds, tests, and validates the model serving service before deploying it through a progressive delivery process.

## Online Experimentation Strategy

```{mermaid}
graph TD
    A[New Model Candidate] --> B[Deploy in Parallel]
    B --> C[Champion Model 90%]
    B --> D[Challenger Model 10%]

    C --> E[Existing Users]
    D --> F[Test Users]

    E --> G[Collect Metrics]
    F --> G

    G --> H[Compare Performance]
    H --> I{Challenger Better?}

    I -->|No| J[Keep Champion]
    I -->|Yes| K[Gradual Traffic Shift]

    K --> L[20% Traffic]
    L --> M[Monitor Results]
    M --> N{Still Better?}

    N -->|No| O[Revert to Champion]
    N -->|Yes| P[50% Traffic]
    P --> Q[Final Validation]
    Q --> R[Full Deployment]

    style A fill:#e1f5fe
    style C fill:#fff3e0
    style D fill:#fff8e1
    style R fill:#c8e6c9
    style I fill:#ffecb3
    style N fill:#ffecb3
```

:::{important} Stages of Model Deployment

1.  **Continuous Integration (CI):**
    - Test the model interface (input/output format).
    - Validate compatibility with the target infrastructure (packages, accelerators).
    - Check that the model's latency is acceptable.
2.  **Continuous Deployment (CD):** - The model undergoes progressive delivery (e.g., canary, blue-green, or shadow deployments). - Smoke testing focuses on service efficiency (latency, throughput) and errors. - Online experiments (e.g., A/B testing, MAB testing) are run to test the model's effectiveness in production with live traffic.
    :::

::::{warning} Online Experimentation is Key
Deciding whether a new model candidate should replace the production version is a more complex and multi-dimensional task compared to deploying other software assets. In a progressive delivery approach, a new model candidate does not immediately replace the previous version but runs in parallel, with a subset of users redirected to the new model in stages.
::::

---

::::{admonition} Typical Assets
:class: seealso

- Model serving executable application (e.g., a container image)
- Online experimentation evaluation metrics
  ::::

::::{admonition} Core MLOps Capabilities
:class: seealso

- Model serving
- Model registry
- Online experimentation
- ML metadata & artifact repository
  ::::
