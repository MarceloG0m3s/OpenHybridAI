# OpenHybridAI — Development Instructions / Instruções de Desenvolvimento

**Project:** OpenHybridAI  
**Document status:** Initial development guidelines / Diretrizes iniciais de desenvolvimento

---

## 1. Purpose and Scope

These instructions define the development rules that AI coding agents must follow when working on OpenHybridAI.

All agents must respect the project's documented architecture, principles, security requirements, and approved development scope.

Before making changes, agents must review the relevant project documentation.

Primary references:

- `README.md`
- `ARCHITECTURE.md`
- `ROADMAP.md`, when available
- Relevant issues and approved pull requests

The project is in early development. Planned features must never be presented as implemented features.

## 2. Core Project Principles

### 2.1 Local-first

Prefer local execution whenever suitable local hardware and software capabilities are available.

### 2.2 Cloud by explicit authorization

Cloud execution is optional and must never be enabled as an unrestricted automatic fallback.

Paid cloud requests require explicit user authorization.

### 2.3 Category-based permissions

Cloud permissions must be evaluated by workload category.

Unknown or unclassified workloads must not be automatically sent to cloud providers.

### 2.4 Spending protection

A cloud budget of `R$ 0` means that paid cloud execution is prohibited.

If the expected cost cannot be reliably determined, do not automatically dispatch the request.

### 2.5 Desktop-first

Interactive desktop responsiveness has priority over background AI workloads.

### 2.6 Hardware independence

Avoid unnecessary vendor lock-in.

Hardware-specific implementations must remain isolated behind appropriate adapters.

Different GPUs must be treated as independent execution resources unless a specific shared-execution capability has been implemented and validated.

Never assume unified VRAM or automatic cross-vendor model sharding.

## 3. Development Scope and Decisions

- Implement only features within the explicitly approved project scope.
- Do not independently expand the MVP.
- Do not introduce architectural changes without explaining their impact.
- Do not select major frameworks, runtimes, or dependencies without evaluating alternatives.
- Treat potential integrations as candidates until they are formally approved.
- Do not create unnecessary scaffolding or placeholder implementations.
- When requirements are ambiguous, ask for clarification before making assumptions.

## 4. Security and Data Protection

Security and user control are mandatory requirements.

Agents must:

- Never expose passwords, API keys, access tokens, or private credentials.
- Never commit secrets or sensitive configuration files.
- Avoid logging sensitive prompts and user payloads.
- Minimize collection and retention of personal data.
- Never initiate paid cloud requests without explicit authorization.
- Never silently transmit local user data to external services.

Any operation involving user data must respect the documented privacy and authorization policies.

## 5. System and Hardware Safety

Initial system discovery and diagnostics should be read-only.

Without explicit user authorization, agents must not:

- Install or remove system drivers.
- Modify kernel parameters or system configuration.
- Change partitions or filesystems.
- Format, wipe, or discard storage devices.
- Delete user data.
- Execute destructive administrative commands.
- Perform uncontrolled hardware stress tests.
- Change resource-control settings that could destabilize the workstation.

Prefer reversible changes and explain the impact before executing operations that affect the host system.

## 6. Architecture Boundaries

Keep the following responsibilities logically separated:

- System Discovery
- Capability Registry
- Hardware Adapters
- Workload Classifier
- Policy Engine
- Scheduler and Router
- Local Runtime Adapters
- Cloud Adapter
- Desktop Guard
- Observability

These are intended architectural boundaries, not a requirement to implement every component immediately.

Agents must preserve the separation between policy decisions, hardware detection, execution runtimes, and cloud integrations.

A detected device must not automatically be considered a validated execution backend.

Distinguish between:

- Detected capabilities
- Available capabilities
- Validated capabilities

## 7. Coding and Quality Standards

- Prefer simple, maintainable, modular implementations.
- Avoid unnecessary complexity and premature optimization.
- Keep changes focused on one logical objective.
- Preserve existing functionality unless a change is explicitly approved.
- Add tests appropriate to the modified functionality.
- Document relevant limitations and failure conditions.
- Do not claim that tests passed unless they were actually executed.
- Record commands and relevant validation results.

New dependencies must have a clear technical justification.

## 8. Git and Contribution Workflow

The main branch must remain protected by the project's review workflow.

Expected workflow:

1. Synchronize with `main`.
2. Create a dedicated feature, fix, documentation, or test branch.
3. Implement one logical change.
4. Review the diff.
5. Run relevant validation.
6. Create a descriptive commit.
7. Open a pull request.
8. Review and approve changes before merging.

Use conventional commit prefixes where appropriate:

- `feat:` — New functionality
- `fix:` — Bug fixes
- `docs:` — Documentation
- `test:` — Tests
- `refactor:` — Internal restructuring
- `chore:` — Maintenance

Never push directly to `main` or merge pull requests without explicit authorization from the project owner.

## 9. Documentation Standards

All project documentation must be maintained in English and Portuguese.

Requirements:

- Keep corresponding sections in both languages.
- Preserve consistent technical terminology.
- Use English for source-code identifiers and filenames.
- Clearly distinguish implemented, experimental, planned, and unsupported capabilities.
- Document prerequisites, limitations, and validation procedures.
- Update documentation when approved architectural decisions change.

Never document an experimental feature as officially supported without reproducible validation.

## 10. Hardware Compatibility and Validation

Hardware compatibility must be validated against specific combinations of:

- Operating system
- Kernel
- CPU
- GPU
- Drivers
- Compute runtimes
- Inference runtimes

A successful test on one workstation does not establish universal compatibility.

Do not assume that AMD and NVIDIA devices can share memory or execute a model cooperatively.

Thermal behavior, resource consumption, and desktop responsiveness must be considered during validation.

## 11. Agent Communication and Execution

Before significant modifications:

1. Explain the proposed change.
2. Identify affected components.
3. Identify potential risks.
4. Explain the validation strategy.

Prefer small, reviewable development steps.

When a task requires an irreversible operation, external spending, or modification of the host system, obtain explicit authorization first.

Do not silently change project requirements or previously approved decisions.

## 12. Final Development Rule

OpenHybridAI must remain local-first, hardware-independent, transparent, secure, and user-controlled.

Technical convenience must never override user authorization, data protection, or desktop stability.

---

# Português

## 1. Objetivo e Escopo

Estas instruções definem as regras de desenvolvimento que os agentes de IA devem seguir ao trabalhar no OpenHybridAI.

Todos os agentes devem respeitar a arquitetura documentada, os princípios do projeto, os requisitos de segurança e o escopo de desenvolvimento aprovado.

Antes de realizar alterações, os agentes devem consultar a documentação pertinente.

Referências principais:

- `README.md`
- `ARCHITECTURE.md`
- `ROADMAP.md`, quando disponível
- Issues relevantes e pull requests aprovados

O projeto está em estágio inicial de desenvolvimento. Funcionalidades planejadas nunca devem ser apresentadas como funcionalidades implementadas.

## 2. Princípios Fundamentais

### 2.1 Prioridade para execução local

Priorizar a execução local sempre que houver hardware e recursos de software adequados.

### 2.2 Nuvem mediante autorização explícita

A execução em nuvem é opcional e nunca deve funcionar como fallback automático irrestrito.

Solicitações pagas de nuvem exigem autorização explícita do usuário.

### 2.3 Permissões por categoria

As permissões para processamento em nuvem devem ser avaliadas por categoria de carga de trabalho.

Tarefas desconhecidas ou não classificadas não podem ser enviadas automaticamente a provedores de nuvem.

### 2.4 Proteção de gastos

Um orçamento de nuvem igual a `R$ 0` significa que a execução paga em nuvem está proibida.

Se o custo esperado não puder ser determinado com confiabilidade, não realizar o envio automático.

### 2.5 Desktop em primeiro lugar

A fluidez do desktop interativo tem prioridade sobre cargas de trabalho de IA executadas em segundo plano.

### 2.6 Independência de hardware

Evitar dependência desnecessária de fabricantes específicos.

Implementações específicas de hardware devem permanecer isoladas por meio de adaptadores apropriados.

GPUs diferentes devem ser tratadas como recursos de execução independentes, a menos que uma capacidade específica de execução compartilhada tenha sido implementada e validada.

Nunca presumir VRAM unificada ou divisão automática de modelos entre fabricantes.

## 3. Escopo e Decisões de Desenvolvimento

- Implementar somente funcionalidades dentro do escopo explicitamente aprovado.
- Não ampliar o MVP por iniciativa própria.
- Não introduzir mudanças arquiteturais sem explicar seus impactos.
- Não escolher frameworks, runtimes ou dependências importantes sem avaliar alternativas.
- Tratar integrações potenciais como candidatas até sua aprovação formal.
- Não criar estruturas desnecessárias ou implementações provisórias sem finalidade definida.
- Quando os requisitos forem ambíguos, solicitar esclarecimentos antes de assumir decisões.

## 4. Segurança e Proteção de Dados

Segurança e controle do usuário são requisitos obrigatórios.

Os agentes devem:

- Nunca expor senhas, chaves de API, tokens de acesso ou credenciais privadas.
- Nunca enviar segredos ou arquivos de configuração sensíveis ao repositório.
- Evitar registrar prompts e conteúdos sensíveis em logs.
- Minimizar a coleta e a retenção de dados pessoais.
- Nunca iniciar solicitações pagas de nuvem sem autorização explícita.
- Nunca transmitir silenciosamente dados locais do usuário a serviços externos.

Qualquer operação envolvendo dados do usuário deve respeitar as políticas documentadas de privacidade e autorização.

## 5. Segurança do Sistema e do Hardware

A descoberta e os diagnósticos iniciais do sistema devem ser somente de leitura.

Sem autorização explícita do usuário, os agentes não devem:

- Instalar ou remover drivers do sistema.
- Modificar parâmetros do kernel ou configurações do sistema.
- Alterar partições ou sistemas de arquivos.
- Formatar, apagar ou descartar blocos de dispositivos de armazenamento.
- Excluir dados do usuário.
- Executar comandos administrativos destrutivos.
- Realizar testes de estresse de hardware sem controle.
- Alterar configurações de gerenciamento de recursos que possam desestabilizar a estação de trabalho.

Preferir alterações reversíveis e explicar seus impactos antes de executar operações que afetem o sistema hospedeiro.

## 6. Limites da Arquitetura

Manter separadas, de forma lógica, as seguintes responsabilidades:

- System Discovery
- Capability Registry
- Hardware Adapters
- Workload Classifier
- Policy Engine
- Scheduler and Router
- Local Runtime Adapters
- Cloud Adapter
- Desktop Guard
- Observability

Essas são divisões arquiteturais pretendidas, não uma exigência de implementar todos os componentes imediatamente.

Os agentes devem preservar a separação entre decisões de políticas, identificação de hardware, runtimes de execução e integrações de nuvem.

Um dispositivo identificado não deve ser automaticamente considerado um backend de execução validado.

Distinguir entre:

- Capacidades identificadas
- Capacidades disponíveis
- Capacidades validadas

## 7. Padrões de Código e Qualidade

- Priorizar implementações simples, modulares e de fácil manutenção.
- Evitar complexidade desnecessária e otimizações prematuras.
- Manter alterações concentradas em um único objetivo lógico.
- Preservar funcionalidades existentes, salvo quando uma mudança for explicitamente aprovada.
- Adicionar testes adequados às funcionalidades modificadas.
- Documentar limitações relevantes e condições de falha.
- Não afirmar que testes foram aprovados sem que tenham sido realmente executados.
- Registrar comandos e resultados relevantes de validação.

Novas dependências devem possuir justificativa técnica clara.

## 8. Fluxo de Git e Contribuições

A branch principal deve permanecer protegida pelo fluxo de revisão do projeto.

Fluxo esperado:

1. Sincronizar com `main`.
2. Criar uma branch específica para funcionalidade, correção, documentação ou testes.
3. Implementar uma alteração lógica.
4. Revisar o diff.
5. Executar as validações pertinentes.
6. Criar um commit descritivo.
7. Abrir um pull request.
8. Revisar e aprovar as alterações antes do merge.

Utilizar prefixos de commits convencionais quando apropriado:

- `feat:` — Nova funcionalidade
- `fix:` — Correções
- `docs:` — Documentação
- `test:` — Testes
- `refactor:` — Reestruturação interna
- `chore:` — Manutenção

Nunca realizar push diretamente para `main` nem integrar pull requests sem autorização explícita do responsável pelo projeto.

## 9. Padrões de Documentação

Toda a documentação do projeto deve ser mantida em inglês e português.

Requisitos:

- Manter seções correspondentes nos dois idiomas.
- Preservar a consistência da terminologia técnica.
- Utilizar inglês para identificadores de código-fonte e nomes de arquivos.
- Diferenciar claramente funcionalidades implementadas, experimentais, planejadas e não suportadas.
- Documentar pré-requisitos, limitações e procedimentos de validação.
- Atualizar a documentação quando decisões arquiteturais aprovadas forem modificadas.

Nunca documentar uma funcionalidade experimental como oficialmente suportada sem validação reproduzível.

## 10. Compatibilidade e Validação de Hardware

A compatibilidade de hardware deve ser validada considerando combinações específicas de:

- Sistema operacional
- Kernel
- CPU
- GPU
- Drivers
- Runtimes de computação
- Runtimes de inferência

Um teste bem-sucedido em uma estação de trabalho não comprova compatibilidade universal.

Não presumir que dispositivos AMD e NVIDIA podem compartilhar memória ou executar cooperativamente um mesmo modelo.

O comportamento térmico, o consumo de recursos e a fluidez do desktop devem ser considerados durante as validações.

## 11. Comunicação e Execução dos Agentes

Antes de modificações significativas:

1. Explicar a alteração proposta.
2. Identificar os componentes afetados.
3. Identificar os riscos potenciais.
4. Explicar a estratégia de validação.

Priorizar pequenas etapas de desenvolvimento que possam ser revisadas.

Quando uma tarefa exigir operação irreversível, gasto externo ou modificação do sistema hospedeiro, obter autorização explícita antes de prosseguir.

Não alterar silenciosamente requisitos do projeto ou decisões previamente aprovadas.

## 12. Regra Final de Desenvolvimento

O OpenHybridAI deve permanecer local-first, independente de hardware, transparente, seguro e controlado pelo usuário.

A conveniência técnica nunca deve se sobrepor à autorização do usuário, à proteção dos dados ou à estabilidade do desktop.
