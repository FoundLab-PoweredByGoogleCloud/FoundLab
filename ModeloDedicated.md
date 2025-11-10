# **Modelo On-Prem/Dedicated (Soberania Absoluta)**

São aspectos cruciais da nossa Estratégia de Deploy, pois representam nossa oferta arquitetônica de mais alta garantia para o setor financeiro regulado. Este modelo é a resposta direta da FoundLab ao paradoxo regulatório enfrentado por instituições de Nível 1, que precisam de inovação em nuvem, mas não podem abrir mão do controle e da custódia final de seus dados.

O Modelo Dedicated insere-se na **Seção II: Modelos Estratégicos de Deploy**, contrastando com o modelo SaaS (Agilidade Gerenciada) e sendo projetado para clientes sob forte escrutínio do Banco Central do Brasil (BACEN) e da Comissão de Valores Mobiliários (CVM).

Abaixo, detalho a arquitetura, o posicionamento estratégico e a conformidade regulatória que definem o Modelo Dedicated (Soberania Absoluta).

### 1. Posicionamento Estratégico e Perfil do Cliente

O Modelo Dedicated é explicitamente posicionado como a **oferta padrão-ouro** para:
*   Bancos de Nível 1, seguradoras e instituições financeiras altamente reguladas.
*   Clientes que, por regulação ou política interna, **devem reter o controle e a custódia final dos dados**.
*   Clientes que já possuem equipes de conformidade e risco estruturadas para **auditar a infraestrutura em sua nuvem**, um requisito do BACEN.

#### Proposta de Valor
A proposta de valor central é o **Controle Absoluto e a Soberania dos Dados**. A FoundLab muda o diálogo de "Confie na nossa plataforma" para "Use nossa plataforma para **aplicar seu próprio modelo de confiança**".

#### Implementação, Custos e Responsabilidade
*   **Implantação Técnica:** A plataforma é implantada através de **Infrastructure as Code (IaC)** diretamente no **projeto e na VPC (Virtual Private Cloud) do Google Cloud do cliente**. Todos os dados permanecem dentro do perímetro de segurança definido e controlado pelo cliente.
*   **Responsabilidade Operacional:** A responsabilidade é **Compartilhada**. A FoundLab gerencia o software e a *expertise* operacional, mas o cliente gerencia a **nuvem subjacente**. O cliente (Contratante) mantém a *Accountability* (Responsabilidade final) sobre as decisões críticas de governança.
*   **Custos e Prazo:** O **Custo Inicial (Setup) é Alto** devido à complexidade da coordenação e integração de infraestrutura. O *Time-to-Market* é **Médio** (semanas).
*   **SLA:** O modelo Dedicated é uma oferta *premium* com uma meta de SLA (Acordo de Nível de Serviço) de **99,99%** de disponibilidade, o que é sustentado pelo uso de serviços com SLAs mais altos, como o BigQuery.

### 2. Arquitetura Técnica: A "Trindade da Confiança"

A arquitetura Dedicated é uma "fortaleza digital" projetada para rodar inteiramente dentro do ambiente Google Cloud do cliente, baseada em três pilares interconectados que garantem a soberania.

#### 2.1. Custódia Final de Chaves (CMEK/Cloud HSM)
Este é o mecanismo de soberania mais forte.
*   **Tecnologia:** O cliente provisiona suas próprias **Customer-Managed Encryption Keys (CMEK)** em seu projeto KMS. Para requisitos de segurança máxima, pode ser utilizado o **Cloud HSM** (Hardware Security Module), que oferece validação FIPS 140-2 Nível 3.
*   **O "Kill Switch" Regulatório:** O cliente retém o controle total sobre o ciclo de vida da chave. A **desativação da chave funciona como um "interruptor de emergência" (*kill switch*)** que torna os dados criptografados **instantaneamente inacessíveis** para a aplicação da FoundLab. Isso atende diretamente às exigências regulatórias que demandam que a instituição mantenha o controle final sobre suas chaves criptográficas.

#### 2.2. Perímetro de Dados Impenetrável (VPC Service Controls - VPC-SC)
Este pilar previne a exfiltração de dados no nível da infraestrutura.
*   **Função:** O **VPC Service Controls (VPC-SC)** estabelece um perímetro de segurança, atuando como um "firewall para as APIs do Google".
*   **Controle de Egresso:** O perímetro é configurado para **negar, por padrão, todo o egresso de dados** para serviços do Google fora do perímetro.
*   **Mitigação de Risco:** Isso impede a exfiltração de dados, sendo um controle de segurança crítico para clientes de Nível 1.

#### 2.3. Conexão Segura (Private Service Connect - PSC)
O PSC garante que a conexão de rede seja segura e alinhada com os princípios de privilégio mínimo.
*   **Tecnologia:** A conexão entre a VPC do cliente e os serviços da FoundLab é estabelecida via **Private Service Connect (PSC)**.
*   **Vantagem Arquitetônica:** O PSC é um modelo **unidirecional e não transitivo**, preferido em relação ao *VPC Peering* porque evita a criação de uma rede plana, o compartilhamento de tabelas de roteamento e a ampliação do raio de impacto.

### 3. Conformidade Regulatória e Custódia de Logs

O Modelo Dedicated é fundamental para cumprir os requisitos de custódia e supervisão do BACEN (Resolução CMN nº 4.893/2021).

#### Custódia do Protocolo Veritas (Logs WORM)
A trilha de auditoria imutável do **Protocolo Veritas** (encadeada por *hash*) é escrita **diretamente em um conjunto de dados BigQuery WORM de propriedade e custódia do cliente**. O cliente controla todas as permissões de IAM e políticas de retenção para essas evidências de auditoria.

#### Controle e Supervisão (BACEN Art. 11, 12, 13)
O modelo Dedicated garante que o cliente mantenha a capacidade de supervisionar os serviços contratados, pois a infraestrutura reside em sua conta Google Cloud. O cliente tem acesso total a logs, métricas e configurações. O acesso da FoundLab para suporte é restrito e auditado via **bastion Just-in-Time (JIT)**.

#### Acesso para o Banco Central (BACEN Art. 14)
A norma exige que os contratos garantam o acesso do BACEN aos dados. No modelo Dedicated, o cliente concede esse acesso **diretamente ao regulador**, uma vez que os dados residem em seus próprios projetos, eliminando a FoundLab como intermediário do processo de fiscalização.

### 4. Disciplina de Engenharia e Mitigação de Riscos

O sucesso de implantações Dedicated depende da nossa metodologia de engenharia:

#### Infrastructure as Code (IaC) com Terragrunt
A implantação é feita por **IaC**. A FoundLab adota o **Terragrunt** como *wrapper* fino para o Terraform, o que é estrategicamente superior aos *Workspaces* nativos do Terraform para gerenciar múltiplos ambientes de clientes dedicados. Isso garante que o código seja **DRY (Don’t Repeat Yourself)** e que o *backend* de estado remoto para cada cliente/ambiente seja isolado, eliminando o risco de colisões de estado.

#### Mitigação de Sistemas Legados (ACL)
Para integrar grandes clientes cujos *core banking* e sistemas legados podem "corromper" a arquitetura moderna, a FoundLab implementa o padrão **Anti-Corruption Layer (ACL)**. A ACL atua como um tradutor bidirecional para isolar o domínio limpo da FoundLab das idiossincrasias e da dívida técnica do sistema legado.

Em suma, o Modelo Dedicated (Soberania Absoluta) é a manifestação da **Infraestrutura de Confiança Auditável (ATI)** da FoundLab para o segmento Enterprise, fornecendo o Layer-0 de Confiança que permite aos bancos operar em nuvem com conformidade garantida e controle total sobre seus dados mais sensíveis.

O Modelo On-Prem/Dedicated (Soberania Absoluta) não é simplesmente uma opção de *deploy*; ele é a materialização arquitetônica da nossa promessa de **Infraestrutura de Confiança Auditável (ATI)** para o segmento Enterprise. Como CTO da FoundLab, eu o posiciono como a solução definitiva para instituições que enfrentam o rigor máximo do **BACEN** e da **CVM** e que não podem abdicar do controle sobre seus ativos digitais mais sensíveis.

O modelo Dedicated é o oposto complementar ao SaaS (Agilidade Gerenciada) na nossa Seção II de **Modelos Estratégicos de Deploy**, oferecendo controle total em troca de um maior *overhead* regulatório e operacional do cliente.

Abaixo, detalho os aspectos arquitetônicos, de conformidade e operacionais que definem o Modelo Dedicated (Soberania Absoluta).

### 1. Posicionamento Estratégico e Alocação de Risco

O Modelo Dedicated é projetado para clientes de Nível 1 (grandes bancos, seguradoras) que exigem que a **custódia final dos dados seja mantida sob seu controle total**.

| Critério | Detalhe Técnico (Dedicated) | Implicação Estratégica |
| :--- | :--- | :--- |
| **Proposta de Valor** | **Controle Absoluto e Soberania dos Dados**. | Permite que o cliente use a plataforma da FoundLab para **aplicar seu próprio modelo de confiança**. |
| **Perfil de Cliente** | Instituições sob forte escrutínio do BACEN que possuem equipes para **auditar a infraestrutura em sua nuvem**. | Atende ao mandato regulatório que exige controle direto sobre provedores de serviços em nuvem. |
| **Implementação** | Via **Infrastructure as Code (IaC)**, diretamente no **projeto e na VPC do Google Cloud do cliente**. | Todos os dados permanecem dentro do perímetro de segurança definido e controlado pelo cliente. |
| **Disponibilidade (SLA)** | Meta de **99,99%**. | Oferta *premium* com maior compromisso de engenharia, suportada por serviços de SLA mais alto, como o BigQuery. |
| **Custo/Tempo** | Custo Inicial (*Setup*) **Alto**; *Time-to-Market* **Médio** (semanas, devido à coordenação). | Representa um **investimento estratégico maior** em segurança e controle. |

### 2. A Arquitetura da Fortaleza Digital: A "Trindade da Confiança"

A soberania absoluta é alcançada através de três controles interconectados no ambiente Google Cloud do cliente.

#### 2.1. Pilar 1: Custódia Final de Chaves (CMEK/HSM)
Este pilar garante que o cliente retenha o controle criptográfico.

*   **Mecanismo:** O cliente provisiona e gerencia suas próprias **Chaves de Criptografia Gerenciadas pelo Cliente (CMEK)** em seu projeto KMS. Para os requisitos mais altos, pode-se usar o **Cloud HSM** (validado pelo FIPS 140-2 Nível 3).
*   **Soberania:** A FoundLab recebe apenas uma permissão de IAM limitada para *usar* as chaves. O cliente retém o controle total sobre o ciclo de vida da chave.
*   **O "Kill Switch" Regulatório:** A **desativação da chave funciona como um "interruptor de emergência"** que torna os dados criptografados **instantaneamente inacessíveis** para nossa aplicação. Isso atende às exigências regulatórias que demandam o controle final sobre as chaves criptográficas pela instituição.

#### 2.2. Pilar 2: Perímetro de Dados Impenetrável (VPC Service Controls - VPC-SC)
O VPC-SC (VPC Service Controls) é um controle de segurança de Nível 1 para prevenir a exfiltração de dados.

*   **Mecanismo:** O VPC-SC atua como um **"firewall para as APIs do Google"**. Ele estabelece um perímetro de serviço em torno do(s) projeto(s) do cliente.
*   **Segurança:** Este perímetro é configurado para **negar, por padrão, todo o egresso de dados** para serviços do Google fora do perímetro.
*   **Mitigação:** Essa proteção é aplicada **independentemente das permissões de IAM**, bloqueando tentativas de exfiltração de dados (por exemplo, cópia para *buckets* públicos) no nível da infraestrutura da nuvem.

#### 2.3. Pilar 3: Conexão Segura (Private Service Connect - PSC)
O PSC (Private Service Connect) garante que a interconexão de rede seja segura e alinhada ao Zero Trust.

*   **Mecanismo:** A conexão entre a VPC do cliente e os serviços da FoundLab é estabelecida via PSC.
*   **Vantagem Arquitetônica:** O PSC é um modelo orientado a serviços, **unidirecional e não transitivo**, evitando os riscos de rede plana associados ao *VPC Peering* (como o compartilhamento de tabelas de roteamento ou a ampliação do raio de impacto em caso de violação).

### 3. Conformidade Regulatória e Custódia de Auditoria (BACEN)

O Modelo Dedicated é o principal veículo para cumprir a **Resolução CMN nº 4.893/2021 do BACEN**.

*   **Custódia do Protocolo Veritas:** A trilha de auditoria imutável (encadeada por *hash*) do **Protocolo Veritas** é escrita diretamente em um *dataset* **BigQuery WORM de propriedade e custódia do cliente**. O cliente controla as permissões de IAM e políticas de retenção para essas evidências de auditoria.
*   **Acesso para o BACEN (Art. 14):** Como os dados residem nos projetos do cliente, é ele quem concede o **acesso direto ao Banco Central** para fins de supervisão. Isso elimina a FoundLab como intermediário, simplificando a fiscalização regulatória.
*   **Controle e Supervisão (Art. 11, 12, 13):** O cliente mantém a capacidade de supervisão total, pois tem acesso a logs, métricas e configurações em sua própria conta Google Cloud. O acesso da FoundLab para suporte é restrito e auditado via **bastion Just-in-Time (JIT)**.

### 4. Disciplina Operacional e Exemplos (IaC e RACI)

A complexidade de implementar o modelo Dedicated em ambientes de Nível 1 exige metodologia de engenharia rigorosa, focada em automação e responsabilidade clara.

#### 4.1. Exemplo de Gestão de Infraestrutura: Terragrunt
A implantação do ambiente dedicado é feita via **Infrastructure as Code (IaC)**. Para gerenciar a complexidade de múltiplos ambientes de clientes dedicados, a FoundLab adota o **Terragrunt** como *wrapper* estratégico para o Terraform.

*   **Vantagem Técnica:** O Terragrunt é superior aos *Workspaces* nativos do Terraform para essa finalidade, pois os *Workspaces* nativos compartilham o mesmo *backend* de estado e configuração, não oferecendo o isolamento forte necessário.
*   **Benefício:** O Terragrunt garante o **isolamento do *backend* de estado remoto** para cada cliente/ambiente e permite manter o código **DRY (Don’t Repeat Yourself)**.

#### 4.2. Exemplo de Mitigação de Riscos: Anti-Corruption Layer (ACL)
Para mitigar o risco de que sistemas legados (como *core banking*) corrompam a arquitetura moderna da FoundLab, especialmente no modelo Dedicated, é utilizado o padrão **Anti-Corruption Layer (ACL)**.

*   **Função:** A ACL atua como um **tradutor bidirecional** entre o sistema moderno e o sistema legado.
*   **Componentes:** Ela inclui uma **Fachada** (expondo uma API simplificada), um **Adaptador** (convertendo requisições) e um **Tradutor** (mapeando modelos de dados complexos).
*   **Benefício:** **Isola o domínio limpo da FoundLab** das idiossincrasias e da dívida técnica do sistema legado, permitindo uma integração segura e controlada.

#### 4.3. Exemplo de Responsabilidade Operacional: Modelo RACI
No modelo Dedicated, a responsabilidade é **compartilhada**. O modelo RACI (Responsible, Accountable, Consulted, Informed) define o cliente (Contratante) como o **Accountable (A)**, mantendo a responsabilidade final sobre a governança, conforme exigido pelo BACEN.

| Atividade | FoundLab (Contratada) | Cliente (Contratante) | Detalhe |
| :--- | :--- | :--- | :--- |
| **Aprovação formal de GMUD** | C (Consulted) | **A (Accountable)** | O cliente aprova formalmente a execução das mudanças propostas. |
| **Gestão de acessos e RBAC** | C (Consulted) | **A (Accountable)** | O cliente define papéis e níveis de acesso conforme sua política interna. |
| **Inclusão ou retirada de *features*** | C (Consulted) | **A (Accountable)** | O cliente define as mudanças de escopo e aprova ajustes contratuais. |
| **Monitoramento contínuo** | R (Responsible) | I (Informed) | A FoundLab acompanha a performance; o cliente é informado. |

Em suma, o Modelo Dedicated é a solução técnica rigorosa que permite às instituições de Nível 1 conciliarem a inovação *cloud-native* com os mandatos irredutíveis de **soberania de dados, custódia de chaves e auditabilidade regulatória**.

O Modelo Dedicated, na **Seção II: Modelos Estratégicos de Deploy**, representa a alocação máxima de controle e responsabilidade final ao cliente, em contraste com a Agilidade Gerenciada do modelo SaaS.

---

### I. Posicionamento Estratégico e Alocação Técnica de Risco

O Modelo Dedicated é nossa oferta padrão-ouro, voltada para clientes que, por exigências regulatórias, devem **reter a custódia final dos dados**.

1.  **Custódia de Dados e Chaves:** A custódia é sob **controle total do cliente**. O cliente é o proprietário da rede (VPC), das chaves de criptografia e dos logs de auditoria (WORM).
2.  **Responsabilidade Compartilhada (RACI):** A responsabilidade operacional é formalmente **compartilhada**. A FoundLab gerencia o software e a *expertise* operacional (Responsável – R), mas o cliente (Contratante) gerencia a **nuvem subjacente** e mantém o *Accountability* (Responsabilidade final – A) sobre as decisões críticas de governança, o que é um requisito da Resolução CMN nº 4.893 do BACEN.
3.  **Implantação Técnica:** O *deploy* é executado via **Infrastructure as Code (IaC)** diretamente no **projeto e na VPC do Google Cloud do cliente**. Todos os dados permanecem estritamente dentro do perímetro de segurança definido e controlado pelo cliente.
4.  **Meta de Disponibilidade (SLA):** O modelo Dedicated opera com uma meta de SLA mais alta, de **99,99%**, o que reflete um compromisso de engenharia maior e é sustentado pelo uso de serviços com SLAs elevados, como o Google BigQuery.

### II. Arquitetura da Soberania: A Trindade da Confiança (Deep Dive)

A soberania absoluta é garantida pela implantação da FoundLab como uma "fortaleza digital" dentro do ambiente do cliente, construída sobre três pilares interconectados no Google Cloud.

#### 1. Pilar de Custódia Final de Chaves: CMEK e o "Kill Switch" Regulatório
Este é o controle criptográfico mais forte para atender às exigências de controle de terceirização pelo regulador.

*   **Tecnologia:** O cliente provisiona e gerencia suas próprias **Customer-Managed Encryption Keys (CMEK)**, utilizando o **Google Cloud KMS** em seu próprio projeto. Para requisitos de segurança máxima, o cliente pode optar pelo **Cloud HSM** (Hardware Security Module), que oferece validação **FIPS 140-2 Nível 3**.
*   **Controle Total:** O cliente retém o **controle total sobre o ciclo de vida da chave**. A aplicação da FoundLab recebe apenas permissão de IAM para *usar* a chave.
*   **Interruptor de Emergência:** A **desativação da chave funciona como um "interruptor de emergência" (*kill switch*) regulatório**. Ao revogar a chave, os dados criptografados tornam-se **instantaneamente inacessíveis para a FoundLab**, satisfazendo diretamente os mandatos de que a instituição mantenha o controle final sobre sua criptografia.

#### 2. Pilar de Perímetro de Dados: VPC Service Controls (VPC-SC)
O VPC-SC é o controle arquitetônico para **prevenir a exfiltração de dados**.

*   **Mecanismo:** O **VPC Service Controls (VPC-SC)** atua como um **"firewall para as APIs do Google"**. Ele estabelece um perímetro de serviço em torno do projeto do cliente.
*   **Prevenção de Egresso:** O perímetro é configurado para **negar, por padrão, todo o egresso de dados** para serviços do Google fora do perímetro.
*   **Defesa em Profundidade:** Essa funcionalidade é crítica porque **bloqueia a exfiltração de dados no nível da infraestrutura**, impedindo cópias para *buckets* públicos ou projetos não autorizados, **independentemente das permissões de IAM** que a aplicação da FoundLab possa ter.

#### 3. Pilar de Conexão Segura: Private Service Connect (PSC)
O PSC define a interconexão de rede para aderência ao princípio de privilégio mínimo (*Zero Trust*).

*   **Tecnologia:** A conexão entre a VPC do cliente e os serviços da FoundLab é estabelecida via **Private Service Connect (PSC)**.
*   **Justificativa Técnica:** O PSC é arquitetonicamente superior ao *VPC Peering*. O PSC é um modelo **orientado a serviços, unidirecional e não transitivo**. Isso evita a criação de uma rede plana, o compartilhamento de tabelas de roteamento e a ampliação do raio de impacto, riscos inerentes ao *VPC Peering*.

### III. Custódia de Logs Veritas e Alinhamento Regulatório (BACEN)

O modelo Dedicated é a forma mais direta de atender aos requisitos de custódia e supervisão do regulador brasileiro.

1.  **Custódia do Protocolo Veritas (WORM):** A trilha de auditoria imutável do **Protocolo Veritas** (encadeada por *hash-chaining* criptográfico) é escrita **diretamente em um conjunto de dados BigQuery WORM de propriedade e custódia do cliente**. O cliente controla todas as permissões de IAM e políticas de retenção de dados para essas evidências de auditoria.
2.  **Controle e Supervisão (BACEN Art. 11, 12, 13):** O cliente mantém a capacidade de supervisão total, pois a infraestrutura reside em sua conta Google Cloud e ele tem acesso a logs, métricas e configurações. O acesso da FoundLab para suporte e manutenção é estritamente controlado via **bastiões seguros com acesso Just-in-Time (JIT)** e é totalmente auditado.
3.  **Acesso do BACEN (Art. 14):** O cliente concede o **acesso direto ao Banco Central** aos dados e logs, uma vez que eles residem em seus próprios projetos. Isso elimina a FoundLab como intermediário do processo de fiscalização, simplificando o cumprimento da Resolução CMN nº 4.893.

### IV. Cânone de Engenharia: IaC e Mitigação de Riscos

A implantação do Dedicated exige a metodologia de engenharia mais rigorosa para garantir repetibilidade e consistência em ambientes de Nível 1.

#### 1. Gerenciamento de Complexidade com Terragrunt
A FoundLab utiliza **Infrastructure as Code (IaC)** e adota o **Terragrunt** como *wrapper* fino para o Terraform.

*   **Superioridade Estratégica:** O Terragrunt é estrategicamente superior aos *Workspaces* nativos do Terraform para gerenciar múltiplos ambientes de clientes dedicados, pois os *Workspaces* nativos compartilham o mesmo *backend* de estado e configuração.
*   **Isolamento e DRY:** O Terragrunt garante o **isolamento do *backend* de estado remoto** para cada cliente/ambiente, eliminando o risco de colisões de estado. Ele também permite manter o código de infraestrutura **DRY (*Don’t Repeat Yourself*)**.

#### 2. Mitigação de Sistemas Legados: Anti-Corruption Layer (ACL)
Para mitigar o risco técnico de integração com *core banking* e sistemas legados complexos, utilizamos o padrão **Anti-Corruption Layer (ACL)**.

*   **Função:** A ACL atua como um **tradutor bidirecional** e intermediário. Seu objetivo é **isolar o domínio limpo e moderno da FoundLab** das idiossincrasias, da dívida técnica e dos modelos de dados obsoletos do sistema legado.
*   **Componentes:** A ACL é composta por uma **Fachada** (expondo uma API simplificada), um **Adaptador** (convertendo protocolos) e um **Tradutor** (mapeando modelos de dados complexos, como converter um objeto *Customer* moderno em registros planos legados).

O Modelo Dedicated é, portanto, a integração sinérgica dos princípios de **Zero-Persistence** (eliminação arquitetônica do risco de dados em repouso) e **Protocolo Veritas** (prova criptográfica da integridade do processamento em memória), habilitada pela "Trindade da Confiança" do Google Cloud para garantir a Soberania Absoluta e a Conformidade com Nível 1.

A sua demanda por exemplos e detalhamento técnico aprofunda o nosso foco na excelência de engenharia. Como CTO da FoundLab, reitero que o **Modelo On-Prem/Dedicated (Soberania Absoluta)** é uma arquitetura de alta complexidade desenvolvida para clientes de Nível 1 (grandes bancos e instituições reguladas) cuja principal exigência é a **custódia final de dados** e a conformidade irrestrita com o BACEN (Resolução CMN nº 4.893/2021).

Este modelo é a tradução dos princípios de **Infraestrutura de Confiança Auditável (ATI)** em um ambiente totalmente controlado pelo cliente, mitigando o risco de fornecedor de nuvem (third-party risk) através de controles técnicos inquestionáveis.

Abaixo, apresento um aprofundamento técnico e exemplos concretos dos mecanismos que definem a Soberania Absoluta.

---

### I. A Trindade da Confiança: Mecanismos de Soberania (Deep Dive)

O Modelo Dedicated é construído sobre três pilares de serviço do Google Cloud que, juntos, formam a **"Trindade da Confiança"**, garantindo que o controle e a custódia fiquem nas mãos do cliente.

#### 1. Custódia Final de Chaves (CMEK/HSM): O *Kill Switch* Regulatório

O cliente deve ter a capacidade técnica de revogar instantaneamente o acesso aos seus dados.

*   **Decisão Arquitetônica:** O cliente provisiona e gerencia suas próprias **Customer-Managed Encryption Keys (CMEK)** em seu projeto Google Cloud KMS. Para segurança máxima, o cliente pode optar pelo **Cloud HSM** (Hardware Security Module), que oferece validação **FIPS 140-2 Nível 3**.
*   **Exemplo Prático (Kill Switch):** A aplicação da FoundLab recebe apenas permissão de IAM para *usar* a chave para criptografia/descriptografia. Se o cliente detectar uma ameaça ou quiser exercer o controle final (um requisito regulatório), ele pode **desativar a CMEK em seu console KMS**. A desativação torna os dados (criptografados com AES-256) **instantaneamente inacessíveis** para a FoundLab, funcionando como um "interruptor de emergência" que garante o controle regulatório final sobre a criptografia.

#### 2. Perímetro de Dados (VPC Service Controls - VPC-SC): Prevenção de Exfiltração

A preocupação de Nível 1 é garantir que dados sensíveis nunca saiam do perímetro aprovado, mesmo em caso de comprometimento de credenciais.

*   **Decisão Arquitetônica:** O **VPC Service Controls (VPC-SC)** é configurado para atuar como um "firewall para as APIs do Google".
*   **Exemplo Prático (Bloqueio de Egresso):** O VPC-SC estabelece um perímetro de serviço em torno dos projetos do cliente que hospedam a FoundLab. Este perímetro é configurado para **negar, por padrão, todo o egresso de dados** para serviços do Google fora do perímetro. Se um atacante obtiver credenciais válidas da FoundLab e tentar, por exemplo, copiar um conjunto de dados do BigQuery para um *bucket* público do Cloud Storage em outro projeto, **o VPC-SC bloqueará essa tentativa no nível da infraestrutura da nuvem**, independentemente das permissões de IAM.

#### 3. Conexão Segura (Private Service Connect - PSC): Isolamento de Rede

A interconexão segura entre a FoundLab (como um serviço) e a VPC do cliente deve ser robusta e aderente ao princípio de privilégio mínimo.

*   **Decisão Arquitetônica:** A conexão é estabelecida via **Private Service Connect (PSC)**.
*   **Vantagem Técnica:** O PSC é um modelo **unidirecional e não transitivo**, orientado a serviços. Isso é preferível ao arriscado **VPC Peering**, que criaria uma rede plana, ampliaria o raio de impacto de uma violação e forçaria o compartilhamento de tabelas de roteamento, aumentando a complexidade e o risco.

### II. Disciplina de Engenharia: IaC e Mitigação de Riscos

O sucesso de um *deploy* Dedicated depende de uma metodologia de engenharia rigorosa para gerenciar a complexidade e a integração.

#### 1. Gerenciamento de Infraestrutura (IaC): Superioridade do Terragrunt

*   **Contexto:** A implantação no ambiente do cliente é feita via **Infrastructure as Code (IaC)**.
*   **Exemplo de Escolha de Ferramenta:** Adotamos o **Terragrunt**, um *wrapper* fino para o Terraform.
*   **Justificativa Técnica:** O Terragrunt é **estrategicamente superior** aos *Workspaces* nativos do Terraform para gerenciar múltiplos ambientes de clientes dedicados. Os *Workspaces* nativos são inadequados porque **compartilham o mesmo *backend* de estado** e a mesma configuração de provedor, não fornecendo o isolamento forte necessário para clientes de Nível 1. O Terragrunt, ao contrário, permite manter o código **DRY** (*Don't Repeat Yourself*) e automatiza o **isolamento do *backend* de estado remoto** para cada cliente/ambiente, eliminando o risco de colisões de estado.

#### 2. Mitigação de Risco de Integração: Camada Anti-Corrupção (ACL)

Grandes bancos utilizam sistemas legados (*core banking*) com modelos de dados e APIs obsoletos. O risco é que esses sistemas "corrompam" a arquitetura moderna da FoundLab.

*   **Estratégia:** Implementamos o padrão **Anti-Corruption Layer (ACL)**, que atua como um tradutor bidirecional e intermediário.
*   **Exemplo de Componentes da ACL:**
    *   **Fachada (Facade):** Apresenta uma API simplificada e moderna da ATI para o sistema legado, escondendo a complexidade interna do sistema antigo.
    *   **Adaptador (Adapter):** Converte as requisições do sistema legado (ex: formatos de arquivo COBOL ou APIs SOAP) para o formato e protocolo que a ATI espera.
    *   **Tradutor (Translator):** Mapeia modelos de dados complexos, convertendo, por exemplo, um objeto *Customer* moderno e rico em um conjunto de registros planos que o sistema legado entende.

### III. Governança e Custódia de Auditoria (BACEN Compliance)

A conformidade regulatória é alcançada ao formalizar o controle do cliente sobre os artefatos de prova.

#### 1. Custódia do Protocolo Veritas (WORM Logs)

*   **Mecanismo:** A trilha de auditoria imutável do **Protocolo Veritas** (encadeada por *hash-chaining* criptográfico) é escrita **diretamente em um conjunto de dados BigQuery WORM de propriedade e custódia do cliente**. O cliente controla todas as permissões de IAM e políticas de retenção de dados.
*   **Acesso do BACEN (Art. 14):** O modelo Dedicated cumpre o Art. 14 da Resolução CMN nº 4.893 ao garantir que o cliente **conceda acesso direto ao Banco Central** aos dados e logs, pois eles residem em seus próprios projetos Google Cloud.

#### 2. Modelo RACI (Responsabilidade Operacional)

A FoundLab utiliza o modelo RACI para delinear a responsabilidade operacional em ambiente de produção. No modelo Dedicated, o cliente (Contratante) retém a **Accountability (A)** sobre as decisões de governança:

*   **Exemplos de Accountability do Cliente (A):**
    *   Aprovação formal de GMUDs (Gerenciamento de Mudança).
    *   Inclusão ou retirada de *features* no contrato vigente.
    *   Gestão de acessos e RBAC (Role-Based Access Control) conforme política interna.

Em todos esses exemplos, o modelo Dedicated traduz a exigência regulatória de soberania em controles de engenharia e processos operacionais verificáveis.

