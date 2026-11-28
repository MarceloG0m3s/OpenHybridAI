# OpenHybridAI — Architecture / Arquitetura

**Technical architecture proposal / Proposta de arquitetura técnica**  
**Status / Estado:** Draft — early development / Rascunho — desenvolvimento inicial  
**Document version / Versão do documento:** 0.1

> **Important / Importante**
>
> This document describes the intended architecture and engineering direction of OpenHybridAI. It is not a statement that all components, integrations, or behaviors are already implemented.
>
> Este documento descreve a arquitetura pretendida e a direção de engenharia do OpenHybridAI. Ele não afirma que todos os componentes, integrações ou comportamentos já estejam implementados.

------------------------------------------------------------------------

## Contents / Sumário

- [1. English](#1-english)
  - [1.1 Purpose and scope](#11-purpose-and-scope)
  - [1.2 Architecture principles](#12-architecture-principles)
  - [1.3 System context](#13-system-context)
  - [1.4 Logical components](#14-logical-components)
  - [1.5 Workload execution lifecycle](#15-workload-execution-lifecycle)
  - [1.6 Hybrid and cloud authorization](#16-hybrid-and-cloud-authorization)
  - [1.7 Hardware and GPU strategy](#17-hardware-and-gpu-strategy)
  - [1.8 Desktop protection and scheduling](#18-desktop-protection-and-scheduling)
  - [1.9 Observability and diagnostics](#19-observability-and-diagnostics)
  - [1.10 Security and privacy boundaries](#110-security-and-privacy-boundaries)
  - [1.11 Deployment direction](#111-deployment-direction)
  - [1.12 Failure handling](#112-failure-handling)
  - [1.13 Proposed implementation sequence](#113-proposed-implementation-sequence)
  - [1.14 Technology evaluation](#114-technology-evaluation)
  - [1.15 Open architecture questions](#115-open-architecture-questions)
- [2. Português](#2-português)
  - [2.1 Objetivo e escopo](#21-objetivo-e-escopo)
  - [2.2 Princípios de arquitetura](#22-princípios-de-arquitetura)
  - [2.3 Contexto do sistema](#23-contexto-do-sistema)
  - [2.4 Componentes lógicos](#24-componentes-lógicos)
  - [2.5 Ciclo de execução de uma tarefa](#25-ciclo-de-execução-de-uma-tarefa)
  - [2.6 Autorização híbrida e uso da nuvem](#26-autorização-híbrida-e-uso-da-nuvem)
  - [2.7 Estratégia de hardware e GPUs](#27-estratégia-de-hardware-e-gpus)
  - [2.8 Proteção do desktop e escalonamento](#28-proteção-do-desktop-e-escalonamento)
  - [2.9 Observabilidade e diagnósticos](#29-observabilidade-e-diagnósticos)
  - [2.10 Limites de segurança e privacidade](#210-limites-de-segurança-e-privacidade)
  - [2.11 Direção de implantação](#211-direção-de-implantação)
  - [2.12 Tratamento de falhas](#212-tratamento-de-falhas)
  - [2.13 Sequência proposta de implementação](#213-sequência-proposta-de-implementação)
  - [2.14 Avaliação de tecnologias](#214-avaliação-de-tecnologias)
  - [2.15 Questões arquiteturais em aberto](#215-questões-arquiteturais-em-aberto)

------------------------------------------------------------------------

# 1. English

## 1.1 Purpose and scope

OpenHybridAI is intended to be a local-first control and orchestration layer for AI workloads on personal workstations. It should help users discover available hardware, identify usable execution backends, choose where a workload runs, and understand the resource and cost implications.

The system is designed for heterogeneous environments, including workstations with AMD and NVIDIA GPUs. Local execution is the default preference. Cloud execution is an optional capability that must remain subject to explicit user authorization and spending limits.

The architecture separates policy decisions from inference engines and hardware-specific runtimes. This separation is intended to make the system easier to extend without binding the whole project to one GPU vendor, inference runtime, or cloud provider.

### Scope

The intended scope includes:

- Read-only system and hardware discovery.
- Detection of relevant drivers and compute runtimes.
- A common capability description for available execution backends.
- Local inference integration.
- Workload classification and policy evaluation.
- Local-first workload routing.
- Optional, permission-controlled cloud execution.
- User-defined cloud spending limits.
- Desktop responsiveness and resource protection.
- Diagnostics, logs, and workload observability.

### Non-goals and explicit limitations

The initial architecture does **not** assume:

- That VRAM from different GPUs can be combined into one memory pool.
- That a model can automatically be split across AMD and NVIDIA GPUs.
- That every model or runtime supports every GPU.
- That a successful test on one workstation proves universal compatibility.
- That cloud execution is enabled by default.
- That every technology listed in this document is already integrated.

These capabilities require independent implementation and reproducible validation before they can be advertised as supported.

## 1.2 Architecture principles

### Local-first

The scheduler should prefer a suitable local backend. Local execution avoids unnecessary network dependency and can reduce recurring cloud costs and data exposure.

### Cloud by explicit choice

Cloud execution is an optional route. The system must not send a workload to a paid provider merely because a local request failed.

### Authorization by workload category

Cloud permission should be configurable by workload category. A user’s approval for one category must not silently authorize unrelated categories.

The category taxonomy is still an architecture decision to be finalized. Every workload must carry a category or be rejected from cloud routing until its classification is known.

### Explicit spending limits

The user must be able to define a cloud budget. A budget of `R$ 0` means that paid cloud execution is not authorized.

The policy layer must check authorization and budget before dispatching a cloud request. If the expected cost cannot be determined with sufficient confidence, the request should not be sent automatically.

### Desktop-first behavior

Interactive use of the workstation takes priority over background AI workloads. The scheduler should be able to defer, reduce, or pause background work when resource pressure threatens desktop responsiveness.

### Hardware and vendor independence

Hardware-specific behavior belongs behind adapters. The core policy and scheduling logic should not depend directly on AMD-only or NVIDIA-only APIs.

### Observable decisions

The system should be able to explain why a workload was routed, deferred, rejected, or sent to a particular backend. Resource use and cloud spending should be visible to the user.

### Fail safely

Missing permission, unknown workload classification, unavailable pricing, or uncertain cloud authorization must not result in an automatic paid request.

## 1.3 System context

The following diagram shows the intended high-level boundaries. It is conceptual and does not represent a completed implementation.

``` mermaid
flowchart TB
    USER["User"]
    CLIENTS["AI Applications and Interfaces<br/>Potential clients: Open WebUI, ComfyUI"]
    OHAI["OpenHybridAI Control Layer"]
    POLICY["Policy and Authorization"]
    ROUTER["Workload Scheduler and Router"]
    DIAG["System Discovery and Diagnostics"]
    LOCAL["Local Inference Adapters<br/>Potential: Ollama, llama.cpp"]
    GPU["Hardware and Compute Backends<br/>AMD ROCm / NVIDIA CUDA"]
    CLOUD["Optional Cloud Adapter<br/>Provider integration under evaluation"]
    OBS["Logs, Metrics and Diagnostics"]

    USER --> CLIENTS
    CLIENTS --> OHAI
    OHAI --> POLICY
    OHAI --> ROUTER
    OHAI --> DIAG
    ROUTER --> LOCAL
    LOCAL --> GPU
    ROUTER --> CLOUD
    OHAI --> OBS
    DIAG --> OBS
    POLICY --> OBS
    ROUTER --> OBS
```

The control layer coordinates policy, discovery, scheduling, and observability. It should not reimplement GPU drivers or inference engines. Instead, it should communicate with supported runtimes through well-defined adapters.

Potential user-facing clients and integrations are candidates for evaluation, not confirmed dependencies.

## 1.4 Logical components

The system is intended to be divided into the following logical components. They may initially run in one application process; the boundaries are logical and do not require a distributed deployment.

| Component              | Responsibility                                           | Current status      |
|------------------------|----------------------------------------------------------|---------------------|
| System Discovery       | Detect hardware, drivers, memory, and available runtimes | Planned             |
| Capability Registry    | Describe what each detected backend can do               | Planned             |
| Hardware Adapters      | Expose vendor-specific device and runtime information    | Under investigation |
| Workload Classifier    | Attach a category and execution requirements to a task   | Planned             |
| Policy Engine          | Evaluate local/cloud permissions and spending limits     | Planned             |
| Scheduler and Router   | Select, defer, or reject an execution path               | Planned             |
| Local Runtime Adapters | Connect to local inference engines                       | Planned             |
| Cloud Adapter          | Connect to an explicitly authorized cloud service        | Under evaluation    |
| Desktop Guard          | Protect interactive responsiveness                       | Planned             |
| Observability          | Record decisions, resource use, and diagnostics          | Planned             |

### System Discovery

Discovery should collect the information needed to make routing decisions, such as:

- Operating system and kernel.
- CPU and system memory.
- GPU vendor, model, and device identity.
- Driver and compute-runtime availability.
- Available device memory and current utilization, where supported.
- Runtime health and basic execution capability.

Discovery should be read-only in its initial form. It must not silently install drivers, change kernel parameters, or alter system configuration.

### Capability Registry

Discovery results should be converted into a normalized capability description.

A capability record may include:

- Device identifier and vendor.
- Supported runtime or backend.
- Available memory and relevant limits.
- Supported task or model formats, when known.
- Health and availability state.
- Evidence source and last successful validation.

The registry must distinguish **detected**, **available**, and **validated** capabilities. Detecting a GPU does not prove that a usable inference backend is installed.

### Hardware Adapters

Vendor-specific detection and runtime communication should be isolated behind adapters.

Initial areas of interest:

- AMD: `amdgpu` and ROCm.
- NVIDIA: NVIDIA driver and CUDA.
- Additional hardware backends may be considered later.

An adapter should report capabilities and errors without making global policy decisions. The policy engine and scheduler remain responsible for deciding whether a workload may run.

### Workload Classifier

Each task should have structured metadata, including:

- Workload category.
- Required runtime or execution capabilities.
- Approximate resource requirements, when available.
- Interactive or background priority.
- Whether remote processing is permitted by the task and user policy.

Unknown or missing category metadata must not be treated as permission to use the cloud.

### Policy Engine

The policy engine is the authority for permission and spending decisions. It should evaluate:

1.  Is local execution preferred or required?
2.  Is cloud execution enabled by the user?
3.  Is this workload category authorized for cloud processing?
4.  Is the configured budget greater than zero?
5.  Is the expected request cost within the remaining budget?
6.  Is the selected provider allowed by the user’s policy?

A denied policy decision must stop cloud dispatch. The scheduler may then use an eligible local route, queue the task, or return a clear explanation.

### Scheduler and Router

The scheduler should select an execution path using:

- Workload requirements.
- Detected and validated backend capabilities.
- Current resource availability.
- Desktop protection rules.
- User authorization and budget policy.

The first implementation should favor selecting a suitable backend for a task. It must not imply that different GPUs can transparently share VRAM or that cross-vendor model sharding is available.

### Local Runtime Adapters

Local runtime adapters provide a consistent interface to supported inference engines.

Ollama and llama.cpp are candidates for evaluation. The exact adapter contract, supported versions, model formats, and runtime lifecycle behavior remain to be defined.

### Cloud Adapter

The cloud adapter should be isolated from the core scheduler and invoked only after a positive policy decision.

A provider abstraction or a routing layer such as LiteLLM may be evaluated. No cloud provider should be assumed to be enabled or configured by default.

### Desktop Guard

The Desktop Guard is intended to protect interactive use. It should observe resource pressure and communicate with the scheduler to defer or limit background work.

The exact Linux control mechanisms and thresholds remain open engineering decisions. The first version should favor safe observation and scheduling decisions over aggressive system changes.

### Observability

Observability should make the system’s behavior understandable without requiring users to inspect low-level logs manually.

It should record, as appropriate:

- Workload identifier and category.
- Selected backend and device.
- Routing decision and reason.
- Execution status and duration.
- Resource observations.
- Estimated and actual cloud cost, when applicable.
- Failures and recovery actions.

Sensitive prompt and payload logging should be minimized and must be governed by an explicit privacy policy.

## 1.5 Workload execution lifecycle

The intended execution sequence is:

1.  An application submits a workload.
2.  OpenHybridAI validates the request and its metadata.
3.  The classifier identifies the workload category and requirements.
4.  Discovery and the capability registry provide available backend information.
5.  The policy engine evaluates local and cloud permissions.
6.  The scheduler checks local capacity and desktop protection constraints.
7.  A suitable local backend is selected when possible.
8.  If local execution is unavailable or unsuitable, cloud eligibility is evaluated.
9.  Cloud execution proceeds only if every required permission and budget check succeeds.
10. The result, decision, and relevant diagnostics are returned.

``` mermaid
flowchart TD
    A["Workload received"] --> B["Validate request and category"]
    B --> C["Discover eligible backends"]
    C --> D["Evaluate user policy"]
    D --> E{"Suitable local path?"}
    E -->|Yes| F["Check desktop and resource limits"]
    F --> G{"Can run locally now?"}
    G -->|Yes| H["Execute locally"]
    G -->|No| I["Evaluate cloud eligibility"]
    E -->|No| I
    I --> J{"Cloud explicitly authorized?"}
    J -->|No| K["Queue, defer, or explain failure"]
    J -->|Yes| L{"Budget and provider checks pass?"}
    L -->|No| K
    L -->|Yes| M["Execute through cloud adapter"]
    H --> N["Return result and diagnostics"]
    M --> N
    K --> O["Return status and reason"]
```

The diagram expresses the intended policy. The exact retry, queue, and timeout behavior must be defined during implementation.

## 1.6 Hybrid and cloud authorization

Cloud execution is a controlled capability, not an automatic fallback.

### Required authorization gates

A cloud request should proceed only when all applicable conditions are true:

- Cloud use is enabled by the user.
- The workload category is explicitly authorized.
- The selected provider is allowed.
- The configured budget permits the request.
- The expected cost can be evaluated.
- The task’s data and execution requirements permit remote processing.

### Zero-budget behavior

`R$ 0` is a hard prohibition on paid cloud execution. It must not be interpreted as an unlimited budget, a missing setting, or permission to use a free trial without the user’s approval.

### Budget accounting

The architecture should distinguish:

- Configured budget.
- Estimated request cost.
- Reserved or committed cost, if supported.
- Actual reported cost.
- Remaining budget.

The exact accounting model, currency handling, billing reconciliation, and provider-specific pricing behavior are implementation decisions still to be finalized.

### Cloud failure

If a cloud request is denied, exceeds the budget, or cannot be priced reliably, it must not be dispatched. The system should explain the reason and offer an eligible local, queued, or cancelled outcome.

## 1.7 Hardware and GPU strategy

OpenHybridAI is intended to support heterogeneous workstations. Initial validation has focused on a Linux system with:

- AMD Radeon RX 6800 using `amdgpu` / ROCm.
- NVIDIA GeForce RTX 2060 using the NVIDIA Open Kernel Module `620.73.04` / CUDA.
- Bazzite Linux as the reference test environment.

The recorded validation included successful ROCm stress testing and CUDA VectorAdd and N-Body tests. AMD and NVIDIA workloads were also active during the same test session, with no noticeable desktop slowdown reported.

### What this validation proves

It provides initial evidence that both vendor stacks can execute workloads in the same workstation environment.

### What it does not prove

It does not prove:

- Shared or pooled VRAM across devices.
- Automatic model splitting across AMD and NVIDIA.
- That one inference runtime can use both vendors interchangeably.
- Compatibility with all GPU models, drivers, kernels, or distributions.
- Long-duration thermal stability.

### Thermal follow-up

The RX 6800 monitoring log recorded hotspot temperatures near **98 °C** and `THROTTLED` status under heavy activity. Further investigation and thermal validation are required before treating the observed configuration as a stable long-duration workload profile.

### Initial scheduling rule

Until multi-GPU inference is separately implemented and validated, the scheduler should treat devices as distinct execution resources. A workload should be assigned to a backend that independently satisfies its requirements.

## 1.8 Desktop protection and scheduling

The workstation is both an AI execution environment and an interactive desktop. OpenHybridAI must account for both roles.

The intended priority order is:

1.  Interactive desktop responsiveness.
2.  User-requested foreground workloads.
3.  Background workloads.
4.  Opportunistic or queued workloads.

The scheduler should consider GPU utilization, device memory, system memory, CPU load, and backend health where reliable telemetry is available.

Possible actions include:

- Defer a background workload.
- Limit the number of concurrent tasks.
- Pause or resume eligible background work.
- Select another suitable local backend.
- Ask the user before changing execution location.
- Use cloud only when separately authorized and within budget.

The architecture does not yet prescribe a specific Linux resource-control implementation. Any future mechanism must be tested carefully and must not destabilize the operating system or GPU drivers.

## 1.9 Observability and diagnostics

Diagnostics are a foundational part of the project, not an afterthought.

The system should make it possible to answer:

- Which GPUs and runtimes were detected?
- Which backends are actually usable?
- Why was a workload assigned to a particular backend?
- Why was a workload deferred or rejected?
- Was cloud use considered, authorized, and charged?
- Did a driver, runtime, memory, or thermal issue affect execution?

Logs should distinguish between detection, validation, execution
