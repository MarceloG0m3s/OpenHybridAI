# OpenHybridAI
Open-source platform for local and hybrid AI workstations.

**A local-first, hardware-agnostic platform for local and hybrid AI workstations.**  
**Uma plataforma hardware-agnostic e local-first para estações de trabalho de IA local e híbrida.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![Project status](https://img.shields.io/badge/status-early%20development-orange.svg)](#project-status--status-do-projeto)

OpenHybridAI is an open-source project focused on making AI workloads easier to run, manage, and monitor across local hardware and optional cloud services — while keeping users in control of their data, costs, hardware, and desktop experience.

OpenHybridAI é um projeto de código aberto dedicado a facilitar a execução, o gerenciamento e o monitoramento de cargas de trabalho de IA em hardware local e serviços de nuvem opcionais — mantendo o usuário no controle dos dados, dos custos, do hardware e da experiência de uso do computador.

> **Project stage / Estágio do projeto:** early development. The repository is being established and the architecture is evolving. The principles and roadmap below describe the project direction; they must not be interpreted as a claim that every planned component is already implemented.
>
> **Estágio do projeto:** desenvolvimento inicial. O repositório está sendo estruturado e a arquitetura está em evolução. Os princípios e o roadmap abaixo descrevem a direção do projeto; não significam que todos os componentes planejados já estejam implementados.

------------------------------------------------------------------------

## Contents / Sumário

- [English](#english)
  - [Overview](#overview)
  - [Core principles](#core-principles)
  - [Architecture direction](#architecture-direction)
  - [Current status](#project-status--status-do-projeto)
  - [Initial hardware validation](#initial-hardware-validation)
  - [Roadmap](#roadmap)
  - [Getting started](#getting-started)
  - [Contributing](#contributing)
  - [License](#license)
- [Português](#português)
  - [Visão geral](#visão-geral)
  - [Princípios fundamentais](#princípios-fundamentais)
  - [Direção da arquitetura](#direção-da-arquitetura)
  - [Status do projeto](#project-status--status-do-projeto)
  - [Validação inicial de hardware](#validação-inicial-de-hardware)
  - [Roadmap](#roadmap-1)
  - [Como começar](#como-começar)
  - [Como contribuir](#como-contribuir)
  - [Licença](#licença)

------------------------------------------------------------------------

# English

## Overview

AI workstations often combine different GPUs, drivers, runtimes, and applications. Managing these components can become complex — especially when local processing, cloud services, power consumption, and desktop responsiveness all matter.

OpenHybridAI aims to provide a unified, transparent approach to this environment. The project is being designed around local execution first, with cloud services available only as an optional and user-controlled extension.

The long-term goal is to help users make better use of the hardware they already own, choose where workloads should run, and understand what is happening on their systems.

## Core principles

### 1. Local-first execution

Local hardware is the preferred execution path whenever it can handle the workload.

### 2. Cloud is optional

Cloud services are an extension, not a requirement. A workload must not be sent to a paid cloud service without the user’s authorization.

### 3. Permission by category

The intended policy model allows users to authorize or deny cloud processing by workload category, instead of granting unrestricted access to every task.

### 4. User-controlled spending

Cloud spending must be governed by an explicit user-defined budget. A zero budget (`R$ 0`) means no cloud spending is authorized. When cloud execution is unavailable or not authorized, the system should prefer a suitable local path whenever possible.

### 5. Desktop-first resource protection

Interactive desktop use must remain a priority. The planned protection layer will aim to prevent background AI workloads from making the workstation unpleasant or impractical to use.

### 6. Hardware independence

The project is intended to avoid vendor lock-in and to support heterogeneous workstations. Compatibility must be validated per hardware, driver, runtime, and operating-system combination.

## Architecture direction

The following is the intended direction, not a declaration that these components are already available.

| Area                | Intended responsibility                                           | Status              |
|:--------------------|-------------------------------------------------------------------|:--------------------|
| System diagnostics  | Discover hardware, drivers, memory, and runtime availability      | Planned             |
| Hardware adapters   | Identify and interface with supported AMD and NVIDIA environments | Under investigation |
| Local inference     | Connect to local runtimes such as Ollama and llama.cpp            | Planned             |
| Workload routing    | Select an execution path based on capability and policy           | Planned             |
| Hybrid/cloud policy | Apply category permissions and explicit spending limits           | Planned             |
| Desktop protection  | Prioritize interactive use and manage background workloads        | Planned             |
| Observability       | Report workload activity, resource use, and relevant diagnostics  | Planned             |

Potential integrations such as Ollama, llama.cpp, and LiteLLM are part of the exploration and roadmap. Their inclusion here does not mean that an integration is already implemented or officially supported.

## Project status / Status do projeto

OpenHybridAI is in **early development**.

The public repository is currently at its initial scaffolding stage. It does not yet provide a stable application release, installer, or complete end-user setup procedure.

The project will document implementation status explicitly so that tested capabilities, work in progress, and future plans remain distinguishable.

## Initial hardware validation

An initial Linux workstation validation was performed using a mixed AMD and NVIDIA GPU configuration.

| Component           | Observed configuration                                         | Recorded result                                         |
|:--------------------|:---------------------------------------------------------------|:--------------------------------------------------------|
| Operating system    | Bazzite Linux                                                  | Test environment                                        |
| AMD GPU             | Radeon RX 6800, `amdgpu` / ROCm                                | ROCm stress test reported as passed                     |
| NVIDIA GPU          | GeForce RTX 2060, NVIDIA Open Kernel Module `620.73.04` / CUDA | CUDA VectorAdd and N-Body tests reported as passed      |
| Mixed-GPU operation | AMD and NVIDIA workloads active during the same test session   | No noticeable desktop slowdown was reported by the user |

### Important thermal note

The RX 6800 monitoring log recorded hotspot temperatures reaching approximately **98 °C**, along with `THROTTLED` status during periods of high GPU activity.

This requires further investigation and thermal validation. The observed run is useful as an initial compatibility test, but it must not be presented as a certified or fully stable thermal profile.

These results apply only to the specific test machine and software configuration. They do not establish universal hardware compatibility, unified VRAM, automatic model sharding, or production readiness.

## Roadmap

The roadmap is incremental and subject to change.

- [x] Create the public GitHub repository and establish the MIT license.
- [x] Perform initial AMD and NVIDIA workload validation on one Linux workstation.
- [ ] Expand bilingual project and technical documentation.
- [ ] Build reproducible system and hardware diagnostic tools.
- [ ] Create repeatable CUDA and ROCm validation procedures.
- [ ] Evaluate and integrate local inference runtimes such as Ollama and llama.cpp.
- [ ] Evaluate workload routing and orchestration options, including LiteLLM.
- [ ] Design the category-based permission and cloud-budget policy engine.
- [ ] Implement local-first fallback behavior.
- [ ] Develop desktop-first resource protection and workload prioritization.
- [ ] Expand the compatibility matrix across hardware and Linux environments.

A roadmap item is not an implemented feature until it has code, documentation, and reproducible validation.

## Getting started

At this stage, the repository is intended for project tracking and development. There is no stable application installer or end-user installation procedure yet.

To obtain the repository:

``` bash
git clone https://github.com/MarceloG0m3s/OpenHybridAI.git
cd OpenHybridAI
```

Check the project documentation and open issues before attempting to use or build components that are still under development.

## Contributing

Contributions, technical feedback, hardware reports, and documentation improvements are welcome.

When reporting a compatibility issue, include as much of the following information as possible:

- Operating system and version.
- Kernel version.
- CPU and GPU models.
- GPU driver versions.
- Runtime versions (for example, CUDA or ROCm).
- The exact command or workload tested.
- Relevant logs, with personal information, tokens, and credentials removed.

For code contributions, open a pull request with a clear description of the change, its scope, and the tests performed. Please do not submit secrets, access tokens, or unredacted private logs.

## License

OpenHybridAI is distributed under the [MIT License](LICENSE).

------------------------------------------------------------------------

# Português

## Visão geral

Estações de trabalho voltadas à inteligência artificial frequentemente combinam diferentes GPUs, drivers, runtimes e aplicativos. Gerenciar esses componentes pode se tornar complexo — especialmente quando processamento local, serviços em nuvem, consumo de energia e fluidez do desktop são importantes.

O OpenHybridAI pretende oferecer uma abordagem unificada e transparente para esse ambiente. O projeto está sendo concebido com prioridade para execução local, permitindo serviços em nuvem apenas como uma extensão opcional e controlada pelo usuário.

O objetivo de longo prazo é ajudar as pessoas a aproveitar melhor o hardware que já possuem, escolher onde cada carga de trabalho será executada e entender o que está acontecendo em seus sistemas.

## Princípios fundamentais

### 1. Execução local em primeiro lugar

O hardware local é o caminho preferencial sempre que tiver capacidade para executar a tarefa.

### 2. Nuvem opcional

Serviços em nuvem são uma extensão, não uma exigência. Nenhuma tarefa deve ser enviada a um serviço pago de nuvem sem autorização do usuário.

### 3. Permissões por categoria

O modelo de políticas pretendido permitirá autorizar ou bloquear o processamento em nuvem por categoria de carga de trabalho, em vez de conceder acesso irrestrito a todas as tarefas.

### 4. Gastos sob controle do usuário

Os gastos com serviços em nuvem deverão obedecer a um orçamento definido explicitamente pelo usuário. Um orçamento zerado (`R$ 0`) significa que nenhum gasto com nuvem está autorizado. Quando a execução em nuvem não estiver disponível ou autorizada, o sistema deverá priorizar um caminho local adequado sempre que possível.

### 5. Proteção do desktop em primeiro lugar

A utilização interativa do computador deve ser prioridade. A camada de proteção planejada deverá evitar que cargas de trabalho de IA executadas em segundo plano tornem a estação de trabalho lenta, desconfortável ou impraticável de usar.

### 6. Independência de hardware

O projeto pretende evitar dependência de um único fabricante e atender estações de trabalho com hardware heterogêneo. A compatibilidade deverá ser validada para cada combinação de hardware, driver, runtime e sistema operacional.

## Direção da arquitetura

A tabela abaixo apresenta a direção pretendida, e não uma declaração de que esses componentes já estejam disponíveis.

| Área                     | Responsabilidade pretendida                                                 | Status          |
|:-------------------------|:----------------------------------------------------------------------------|:----------------|
| Diagnóstico do sistema   | Identificar hardware, drivers, memória e disponibilidade de runtimes        | Planejado       |
| Adaptadores de hardware  | Identificar e integrar ambientes AMD e NVIDIA compatíveis                   | Em investigação |
| Inferência local         | Conectar a runtimes locais como Ollama e llama.cpp                          | Planejado       |
| Roteamento de tarefas    | Selecionar o caminho de execução conforme capacidade e políticas            | Planejado       |
| Políticas híbridas/nuvem | Aplicar permissões por categoria e limites explícitos de gastos             | Planejado       |
| Proteção do desktop      | Priorizar o uso interativo e gerenciar tarefas em segundo plano             | Planejado       |
| Observabilidade          | Apresentar atividade das tarefas, uso de recursos e diagnósticos relevantes | Planejado       |

Integrações potenciais como Ollama, llama.cpp e LiteLLM fazem parte da pesquisa e do roadmap. Sua menção não significa que já estejam implementadas ou oficialmente suportadas.

## Status do projeto

O OpenHybridAI está em **estágio inicial de desenvolvimento**.

O repositório público encontra-se em sua fase inicial de estruturação. Ainda não existe uma versão estável do aplicativo, instalador ou procedimento completo de instalação para usuários finais.

O projeto documentará explicitamente o status de cada implementação para manter claras as diferenças entre funcionalidades testadas, trabalhos em andamento e planos futuros.

## Validação inicial de hardware

Foi realizada uma validação inicial em uma estação de trabalho Linux com uma configuração mista de GPUs AMD e NVIDIA.

| Componente               | Configuração observada                                         | Resultado registrado                                     |
|:-------------------------|:---------------------------------------------------------------|:---------------------------------------------------------|
| Sistema operacional      | Bazzite Linux                                                  | Ambiente de testes                                       |
| GPU AMD                  | Radeon RX 6800, `amdgpu` / ROCm                                | Teste de estresse ROCm reportado como aprovado           |
| GPU NVIDIA               | GeForce RTX 2060, NVIDIA Open Kernel Module `620.73.04` / CUDA | Testes CUDA VectorAdd e N-Body reportados como aprovados |
| Operação com GPUs mistas | Cargas AMD e NVIDIA ativas durante a mesma sessão de testes    | Usuário não percebeu lentidão no desktop                 |

### Observação térmica importante

O registro de monitoramento da RX 6800 apresentou temperaturas de hotspot próximas de **98 °C**, além de indicação `THROTTLED` durante períodos de alta atividade da GPU.

Esse comportamento exige investigação e validação térmica adicionais. A execução observada é útil como teste inicial de compatibilidade, mas não deve ser apresentada como um perfil térmico certificado ou plenamente estável.

Esses resultados são válidos somente para a máquina e a configuração de software utilizadas nos testes. Eles não comprovam compatibilidade universal de hardware, VRAM unificada, divisão automática de modelos entre GPUs ou prontidão para produção.

## Roadmap

O roadmap será desenvolvido de forma incremental e poderá ser alterado.

- [x] Criar o repositório público no GitHub e estabelecer a licença MIT.
- [x] Realizar validação inicial de cargas AMD e NVIDIA em uma estação Linux.
- [ ] Ampliar a documentação bilíngue do projeto e da parte técnica.
- [ ] Desenvolver ferramentas reproduzíveis de diagnóstico do sistema e hardware.
- [ ] Criar procedimentos repetíveis de validação CUDA e ROCm.
- [ ] Avaliar e integrar runtimes de inferência local, como Ollama e llama.cpp.
- [ ] Avaliar opções de roteamento e orquestração de tarefas, incluindo LiteLLM.
- [ ] Projetar o mecanismo de políticas de permissões por categoria e orçamento de nuvem.
- [ ] Implementar o comportamento de fallback local.
- [ ] Desenvolver proteção de recursos e priorização de tarefas para preservar o desktop.
- [ ] Ampliar a matriz de compatibilidade entre hardwares e ambientes Linux.

Um item do roadmap só deverá ser considerado uma funcionalidade implementada quando possuir código, documentação e validação reproduzível.

## Como começar

Neste estágio, o repositório serve para acompanhamento e desenvolvimento do projeto. Ainda não existe instalador estável ou procedimento de instalação para usuários finais.

Para obter o repositório:

``` bash
git clone https://github.com/MarceloG0m3s/OpenHybridAI.git
cd OpenHybridAI
```

Consulte a documentação e as issues abertas antes de tentar utilizar ou compilar componentes que ainda estejam em desenvolvimento.

## Como contribuir

Contribuições, sugestões técnicas, relatos de compatibilidade de hardware e melhorias na documentação são bem-vindos.

Ao relatar um problema de compatibilidade, inclua, sempre que possível:

- Sistema operacional e versão.
- Versão do kernel.
- Modelos de CPU e GPU.
- Versões dos drivers de GPU.
- Versões dos runtimes (por exemplo, CUDA ou ROCm).
- Comando ou carga de trabalho exata utilizada no teste.
- Logs relevantes, removendo informações pessoais, tokens e credenciais.

Para contribuições de código, abra um pull request com uma descrição clara da alteração, seu escopo e os testes realizados. Não envie segredos, tokens de acesso ou logs privados sem sanitização.

## Licença

O OpenHybridAI é distribuído sob a [Licença MIT](LICENSE).

------------------------------------------------------------------------

------------------------------------------------------------------------

**OpenHybridAI**  
Local-first. Hybrid by choice. User-controlled.  
Local-first. Híbrido por escolha. Controle do usuário.
