

## **Memorando Institucional: FoundLab \+ BTG Pactual Comitê Executivo, Private Bank, Risco e Tecnologia**

A Inteligência Artificial Generativa representa a maior alavancagem de eficiência e criação de valor para o setor financeiro na próxima década. No entanto, sua adoção em larga escala é sistematicamente bloqueada por um desafio fundamental: a incapacidade de provar conformidade regulatória (BACEN, CVM, LGPD) e manter trilhas de auditoria irrefutáveis em sistemas inerentemente probabilísticos ou de "caixa preta".

O BTG Pactual, como líder de inovação, enfrenta uma escolha: inovar rapidamente, arriscando a conformidade e a segurança; ou manter a segurança e a regulação, perdendo a janela de inovação. A FoundLab apresenta uma terceira via.

A tese da FoundLab é o "Compliance-as-Infrastructure" (Conformidade como Infraestrutura).1 A conformidade regulatória não deve ser um processo manual, reativo e custoso, mas sim uma propriedade computacional, inerente e matematicamente verificável da própria infraestrutura.

Propomos a adoção da plataforma **Umbrella \+ Veritas 2.0** como a infraestrutura de confiança padrão para todas as cargas de trabalho de IA no BTG Pactual.

* **Umbrella:** A "Fábrica de Decisão" que orquestra múltiplas IAs para analisar e agir sobre dados complexos em velocidade.1  
* **Veritas 2.0:** A "Camada de Prova" que gera uma trilha de auditoria criptográfica, imutável e soberana para cada decisão tomada, resolvendo o paradoxo regulatório.1

Esta infraestrutura servirá como base para o uso interno e otimização dos processos do BTG Pactual (uso primário) e, estrategicamente, como um serviço B2B2B de alto valor agregado para os clientes orbitais do Private Bank (Family Offices, Fundos, Gestoras) (uso secundário).

## **1\. O Cenário Institucional: O Paradoxo Regulatório da IA**

**Público-Alvo:** Risco, Compliance, Jurídico.

O BTG Pactual, como instituição financeira regulada pelo Banco Central do Brasil (BACEN) e pela Comissão de Valores Mobiliários (CVM), opera sob requisitos estritos de retenção de dados, imutabilidade de logs e trilhas de auditoria detalhadas.1

Simultaneamente, a Lei Geral de Proteção de Dados (LGPD), análoga ao GDPR europeu, impõe o "Direito ao Esquecimento", exigindo a exclusão auditável de Dados Pessoais Identificáveis (PII) sob demanda do titular.

Estes dois conjuntos de mandatos legais são, do ponto de vista de arquitetura de sistemas, fundamentalmente contraditórios.

Mandato 1: Retenção e Imutabilidade (SOX/CVM)  
Regulações como a Sarbanes-Oxley (SOX) exigem que registros de auditoria e "workpapers eletrônicos" sejam mantidos por períodos longos (ex: 7 anos) em formato WORM (Write-Once-Read-Many) — "não regravável, não apagável".1 O Departamento de Justiça dos EUA (DOJ), sob o estatuto 18 U.S.C. § 1519, torna um crime federal "contemplar" a exclusão ou alteração de registros se uma investigação puder ser iniciada, mesmo que ainda não esteja pendente.1  
Mandato 2: Exclusão e Privacidade (LGPD/GDPR)  
Regulações de privacidade exigem que a organização tenha a capacidade técnica de localizar e excluir permanentemente os PII de um indivíduo mediante solicitação.1

## **1\. O Cenário Institucional (Continuação)**

A Falha da Infraestrutura Tradicional  
Uma arquitetura de dados tradicional não pode satisfazer ambos os mandatos simultaneamente.

1. Se os logs de auditoria são verdadeiramente imutáveis (WORM) para satisfazer a CVM/SOX, a organização viola a LGPD, pois não pode executar uma exclusão (DELETE).  
2. Se os logs são mutáveis (permitindo DELETE ou UPDATE) para satisfazer a LGPD, a organização viola a CVM/SOX, pois a trilha de auditoria não é "à prova de violação" e falha em uma auditoria forense.

O Agravante da IA Generativa  
A IA Generativa torna este paradoxo um bloqueador de inovação. Para que um LLM seja útil (ex: analisar a carteira de um cliente HNW), ele precisa "ver" PIIs sensíveis. A infraestrutura tradicional não oferece garantias auditáveis de que esses PIIs não serão logados, persistidos em cache ou usados em prompts futuros, criando um passivo regulatório intratável.  
O hashing simples não é uma solução. Reguladores, como a Federal Trade Commission (FTC) dos EUA, já emitiram orientações claras de que "dados com hash *são* PII", pois o hashing é determinístico e permite o rastreamento, não constituindo anonimização.1

Conclusão do Problema  
Sem uma nova arquitetura, o banco é forçado a uma escolha binária:

* **Inovação com Risco:** Usar IA Generativa e aceitar um risco regulatório massivo e não auditável.  
* **Regulação com Estagnação:** Proibir o uso de IA em dados sensíveis e perder a maior janela de eficiência da década.

## **2\. A Tese FoundLab: Arquitetura Regulatória Computável**

**Público-Alvo:** Executivos, Estratégia, Tecnologia.

A tese da FoundLab é que a conformidade não deve ser um processo reativo, mas um *ativo computacional* provável.1

A solução para o "Paradoxo Regulatório" 1 não é um software de GRC (Governança, Risco e Conformidade) ou um *checklist* manual. A solução é uma arquitetura de infraestrutura que resolve a contradição em sua origem.

A Solução Arquitetônica  
A única solução tecnicamente viável para o paradoxo é "desacoplar a existência física dos dados de sua legibilidade criptográfica".1  
A plataforma FoundLab implementa essa arquitetura da seguinte forma:

1. **Satisfazendo CVM/SOX (Retenção Imutável):** Todos os registros de auditoria e eventos de decisão são escritos em um cofre de dados WORM (implementado no BigQuery). Os dados *nunca* são fisicamente excluídos ou atualizados, garantindo uma trilha "append-only" (apenas adição).1  
2. **Satisfazendo LGPD (Direito ao Esquecimento):** Todos os dados no cofre são criptografados com Chaves de Criptografia Gerenciadas pelo Cliente (CMEK \- Customer-Managed Encryption Keys). Para atender a uma solicitação de exclusão de PII, o BTG *não* emite um DELETE (o que violaria o WORM). Em vez disso, o BTG *destrói a chave criptográfica* (um processo chamado "crypto-shredding").1

Resultado Arquitetônico  
O dado físico (agora um ciphertext ilegível) permanece no cofre WORM, satisfazendo o mandato de retenção da CVM/SOX. O PII, no entanto, torna-se permanentemente inacessível e irrecuperável, satisfazendo o "Direito ao Esquecimento" da LGPD.  
A plataforma Umbrella \+ Veritas 2.0 é a implementação ponta-a-ponta desta arquitetura, permitindo que o BTG inove com IA (Camada Umbrella) enquanto gera provas matemáticas de conformidade (Camada Veritas 2.0).

## **3\. A Visão da FoundLab: A Arquitetura da Confiança em Três Camadas**

**Público-Alvo:** Tecnologia, Arquitetura, Risco.

A solução FoundLab é uma fusão da "Fábrica de Decisão" (Umbrella) com a "Camada de Prova" (Veritas 2.0). Ela opera em três camadas de defesa em profundidade.

### **3.1. Camada 1: A "Cidadela" de Segurança Zero-Trust**

Esta é a camada onde os dados são processados. Ela se baseia no princípio da "Radical Security" (Segurança Radical): a segurança mais eficaz é a eliminação arquitetônica do risco.1

* **Zero-Persistence (Persistência Zero):** Este é o pilar central e o principal habilitador da IA. Dados sensíveis (carteiras de clientes, PIIs) são processados *exclusivamente em memória volátil (RAM)*, dentro de contêineres efêmeros (ex: Cloud Run).1 Os dados *nunca* tocam o disco. Isso permite que o BTG alimente seus LLMs com dados bancários altamente sensíveis para análise, com a garantia arquitetônica de que esses dados não serão persistidos, logados ou cacheados. O risco de vazamento de dados em repouso é *eliminado* por design.  
* **Isolamento de Carga de Trabalho (gVisor):** Os processos de IA rodam em GKE Sandbox (gVisor). O gVisor implementa um "kernel de espaço de usuário" que isola o processo do kernel do host. Isso impede "escapes" de contêiner e explorações de kernel, tratando o código de IA como "não confiável".1  
* **Perímetro de Dados Soberano (VPC-SC):** A infraestrutura opera dentro de um Perímetro de Serviço VPC (VPC Service Controls) do Google Cloud. Isso cria um "muro de castelo" digital que bloqueia *toda* tentativa de exfiltração de dados, mesmo por um *insider* mal-intencionado ou uma credencial de serviço comprometida.1  
* **Conectividade Privada (PSC):** A comunicação com os serviços de nuvem (como o cofre BigQuery WORM) ocorre via Private Service Connect, garantindo que o tráfego de dados sensíveis nunca atravesse a internet pública.1

## **3\. A Visão da FoundLab (Continuação)**

### **3.2. Camada 2: A Fábrica de Decisão (Umbrella)**

Esta é a camada onde as decisões são tomadas. O Umbrella é o pipeline operacional de sete estágios que transforma dados brutos e não estruturados (contratos, balanços) em decisões estruturadas e auditáveis.1

* **Pipeline:** Ingestão \-\> Parsing \-\> Extração \-\> Validação \-\> Scoring \-\> Geração de Evidência \-\> Output.  
* **Cognitive Orchestrator (Orquestrador Cognitivo):** Este é o "cérebro" do Umbrella. A plataforma não depende de um único modelo de IA (evitando *lock-in*). O Orquestrador atua como um roteador inteligente, selecionando dinamicamente o melhor motor para a tarefa (ex: Google Gemini para análise complexa, NVIDIA NIMs para alta performance).1  
* **AI Flywheel e Critic-Loop (Ciclo de Feedback):** O sistema é projetado para melhoria contínua e auditável.1 Quando um analista de compliance do BTG (Humano) discorda de uma decisão da IA (ex: aprovação de KYC), ele gera um Human\_Override. O sistema FoundLab captura esse *override* não como um log de texto, mas como um *evento criptográfico* (uma nova Credencial Verificável) que é encadeado à decisão original da IA. Este evento serve a dois propósitos críticos:  
  1. **Risco:** Cria uma trilha de auditoria de responsabilidade inquebrável (HITL \- Human-in-the-Loop).  
  2. **Tecnologia:** É usado como um "sinal de treinamento de alto valor" para realimentar e refinar os modelos de IA, criando um fosso de dados competitivo.1

## **3\. A Visão da FoundLab (Continuação)**

### **3.3. Camada 3: A Camada de Prova (Veritas 2.0)**

Esta é a camada onde as decisões são *provadas*. É o pilar da "Absolute Auditability" (Auditabilidade Absoluta).1 O Veritas 2.0 transforma logs de texto frágeis, que são inerentemente mutáveis, em provas matemáticas irrefutáveis.

* **O Cofre WORM (Write-Once-Read-Many):**  
  * **Mecanismo:** Implementado no Google BigQuery.  
  * **Garantia IaC (Infra-as-Code):** A tabela é configurada com deletion\_protection \= true via Terragrunt. Isso impede que qualquer administrador ou pipeline de CI/CD exclua a tabela no nível da infraestrutura.1  
  * **Garantia IAM (Identidade):** O serviço que escreve no cofre usa uma função IAM personalizada (Veritas-WORM-Ingestor). Esta função possui *apenas* a permissão bigquery.tables.insertData e *nega explicitamente* as permissões bigquery.tables.updateData e bigquery.tables.deleteData. Isso impõe o "append-only" no nível da aplicação, tornando o log à prova de violação.1  
* **A Trilha de Veracidade Criptográfica (W3C VCs):**  
  * **O Problema:** Um log WORM prova que *algo* foi salvo, mas não prova *quem* o salvou ou que o *conteúdo* é verdadeiro.  
  * **A Solução (Notário Digital):** A FoundLab não salva logs de texto. Para cada decisão (ex: "Score de Risco de Onboarding: 75"), o sistema emite uma **Credencial Verificável (VC)** do padrão W3C.1  
  * **Identidade do Emissor (did:web):** A VC é assinada criptograficamente por um *emissor* (o sistema de compliance do BTG) cuja identidade é provada pelo próprio domínio do BTG (ex: did:web:compliance.btgpactual.com). Isso vincula matematicamente a decisão à instituição.1  
  * **Revogação sem Exclusão (StatusList2021):** Se uma decisão for anulada (ex: aprovada por engano), ela não é excluída (o que violaria a SOX/CVM). Em vez disso, o sistema marca o *bit* daquela VC como "revogado" em uma lista de status pública (StatusList2021), preservando a trilha histórica completa para auditores.1

## **4\. Stack Técnico Institucional: Plugável ao GCP-BTG**

**Público-Alvo:** Arquitetura, Tecnologia, Operações de Nuvem.

A arquitetura FoundLab não é um SaaS de nuvem terceira; é uma infraestrutura não-custodial projetada para ser implantada *dentro* do perímetro GCP soberano do BTG Pactual, conectando-se aos serviços e políticas de segurança existentes. A FoundLab não tem acesso aos dados, PIIs ou chaves criptográficas (CMEK) do BTG.

A arquitetura proposta garante 100% de soberania dos dados para o banco.

**Diagrama de Arquitetura de Alto Nível (Proposta)**

Snippet de código

graph TD  
    subgraph Perímetro Soberano GCP-BTG (VPC-SC)  
        direction LR  
        subgraph Cluster GKE (gVisor Sandbox)  
            A\[API Gateway (Ingestão)\] \--\> B(Umbrella: Cognitive Orchestrator)  
            B \--\> C  
            C \--\> D{Decisão Gerada}  
        end  
          
        subgraph Veritas 2.0 (Camada de Prova)  
            D \--\> E\[ACL: Emissor de VC (did:web)\]  
            E \-- Insere VC (IAM Append-Only) \--\> F  
            F \-- Criptografado com \--\> G(Cloud KMS: Chave CMEK)  
        end  
          
        subgraph Acesso de Auditoria  
           H(Auditor/Regulador) \-- Verifica Prova \--\> E  
           H \-- Consulta Log Imutável \--\> F  
        end

        subgraph Gestão de Risco (Kill Switch)  
            I(Time de Compliance BTG) \-- Destrói Chave (LGPD) \--\> G  
        end  
    end  
      
    style F fill:\#0f172a,color:\#fff  
    style G fill:\#b91c1c,color:\#fff  
    style I fill:\#b91c1c,color:\#fff  
    style E fill:\#06b6d4,color:\#fff

## **4\. Stack Técnico Institucional (Continuação)**

Tabela: Mapeamento de Componentes (Arquitetura para GCP)  
O valor da arquitetura não reside nos serviços individuais, mas na configuração sinérgica deles para atender aos requisitos legais contraditórios. A sinergia entre o Cofre WORM e o Kill Switch CMEK é o que cria a vantagem arquitetônica.1

| Capacidade Arquitetural (Veritas 2.0) | Componente GCP Utilizado | Propósito e Requisito Regulatório Atendido | Ref. |
| :---- | :---- | :---- | :---- |
| **Processamento Efêmero (Zero-Persistence)** | Google Cloud Run / GKE (RAM-only) | Processa PII em memória volátil, sem persistência em disco. Elimina risco de vazamento de dados em repouso. | 1 |
| **Isolamento de Carga de Trabalho (Sandbox)** | GKE Sandbox (gVisor) | Isola código de IA "não confiável" do kernel do host, prevenindo *container escape*. | 1 |
| **Firewall de Microsserviços** | Cilium (eBPF) | Aplica políticas de rede L7 baseadas em identidade (SPIFFE) em vez de IP. Bloqueia movimento lateral. | 1 |
| **Perímetro de Dados Soberano** | VPC Service Controls (VPC-SC) | Impede a exfiltração de dados do cofre WORM, mesmo com credenciais roubadas. | 1 |
| **Cofre de Auditoria Imutável (WORM)** | BigQuery (com deletion\_protection) | Armazena VCs de auditoria. Impede a exclusão física de registros (Mandato SOX/CVM). | 1 |
| **Controle de Acesso "Append-Only"** | IAM Custom Role (Veritas-WORM-Ingestor) | Impede UPDATE ou DELETE no nível da aplicação. Garante integridade do log. | 1 |
| **Identidade Criptográfica do Emissor** | did:web (Hospedado no GCS/Cloud CDN) | Publica a chave pública do BTG (did.json) para que auditores possam verificar as VCs. | 1 |
| **"Kill Switch" Regulatório (Crypto-Shred)** | Cloud KMS (Customer-Managed Keys \- CMEK) | Permite "apagar" PII (destruindo a chave) sem violar o WORM (satisfazendo LGPD \+ SOX). | 1 |

## **5\. Proposição de Valor por Público-Alvo Interno**

**Público-Alvo:** Risco & Compliance

A plataforma Veritas 2.0 substitui a "confiança" baseada em processos por "prova" matemática.

1. **Auditoria Não-Repudiável:** A trilha de auditoria (VCs) é assinada criptograficamente. Um DecisionID (ID de Decisão) 1 pode ser rastreado até sua prova matemática, que é irrefutável e vinculada à identidade do emissor (BTG).1  
2. **Solução do Paradoxo Regulatório:** Pela primeira vez, o BTG pode cumprir simultaneamente os mandatos de retenção (SOX/CVM) e os mandatos de exclusão (LGPD) através da arquitetura WORM \+ CMEK.1  
3. **Forense Imediata:** A forense pós-evento 1 deixa de ser uma escavação de logs de texto e passa a ser um "replay criptográfico" da cadeia de VCs, verificando matematicamente a sequência de eventos.1

**Público-Alvo:** Tecnologia & Arquitetura

A plataforma é uma infraestrutura de *confiança* que se integra ao stack GCP existente, não uma *aplicação* que o substitui.

1. **Soberania e Não-Custódia:** A infraestrutura é *plugável* e roda inteiramente dentro do perímetro VPC-SC do BTG. A FoundLab não tem acesso aos dados, PII ou chaves (CMEK). O BTG mantém 100% de soberania.  
2. **Arquitetura Zero-Trust:** A implementação de gVisor, Cilium e VPC-SC representa uma arquitetura de defesa em profundidade que excede os padrões de segurança atuais.1  
3. **Padrões Abertos (W3C):** A camada de prova (VCs, DIDs) usa padrões abertos do W3C, garantindo interoperabilidade e evitando *vendor lock-in*.1

**Público-Alvo:** Produto & Negócio

A infraestrutura desbloqueia a inovação em IA, permitindo o desenvolvimento de produtos que antes eram bloqueados por risco regulatório.

1. **Velocidade de Inovação:** Permite que as áreas de negócio usem IA Generativa com dados reais e sensíveis (via Zero-Persistence), acelerando o *time-to-market* de novos produtos.  
2. **Alfa Operacional:** A automação de processos complexos (como onboarding) gera ganhos de eficiência comprovados, reduzindo o tempo de processamento em ordens de magnitude.1  
3. **Nova Fonte de Receita:** Habilita o modelo B2B2B, transformando a infraestrutura de compliance do BTG em um produto de confiança para seus clientes orbitais.

## **6\. Casos de Uso Imediatos no Ecossistema BTG Pactual**

**Público-Alvo:** Produto, Negócio, Private Bank.

A seguir, detalhamos a aplicação da arquitetura Umbrella \+ Veritas 2.0 em cenários reais do BTG Pactual.

**1\. Onboarding PJ/HNW Sensível**

* **Processo:** A plataforma Umbrella (IA) ingere e analisa documentos complexos de onboarding (contratos sociais, balanços patrimoniais, comprovantes de renda).1  
* **Evidência (Veritas):** O sistema emite uma VerifiableCredential (VC) para a decisão de risco. Ex: "Score de Risco KYC: 85", "Decisão: APROVADO", "Baseado em:".  
* **Valor:** A decisão de aprovação, que hoje depende de um *checklist* manual, passa a ter uma prova criptográfica imutável 1 armazenada no cofre WORM, pronta para auditoria do BACEN.

**2\. Chat Interno (Assessor) com Dado Bancário**

* **Processo:** Um assessor do Private Bank usa um LLM interno (ex: "BTG GPT") para simular uma alocação de carteira. A consulta *requer* dados sensíveis do cliente (posição, PII).  
* **Evidência (Veritas):** A arquitetura **Zero-Persistence** 1 garante que os dados do cliente são processados em RAM e destruídos após a resposta. O Veritas *não* salva a conversa (o PII), mas emite uma VC para a *instrução* e os metadados. Ex: "Assessor solicitou simulação para cliente".  
* **Valor:** Permite o uso seguro de LLMs com dados de clientes, criando uma trilha de auditoria das *ações* sem reter os *dados* sensíveis, resolvendo o risco de conformidade da IA.

## **6\. Casos de Uso Imediatos (Continuação)**

**3\. Parsing Documental (Fundos) com Score de Risco**

* **Processo:** O time de análise de fundos submete lâminas, regulamentos e prospectos de fundos externos. O Umbrella (IA) extrai cláusulas de risco (ex: alavancagem, liquidez) e gera um *score* de compliance.1  
* **Evidência (Veritas):** O sistema emite uma VC que atesta: "IA \[Gemini\] analisou Documento \[Hash\] e atribuiu Score \[X\] com base nas Cláusulas".  
* **Valor:** Cria o "Alfa Operacional" 1, reduzindo o tempo de análise de diligência de horas para minutos, com prova criptográfica da análise realizada.

**4\. Compliance Contínuo (Regra como Código)**

* **Processo:** Uma nova regra da CVM é traduzida para o "Cognitive Orchestrator" 1 como uma política computável. O sistema reavalia posições de carteira de clientes *em batch* durante a noite.  
* **Evidência (Veritas):** O sistema emite VCs apenas para as *exceções* detectadas. Ex: "Carteira marcada por violar Regra \[CVM-XXX\] com Exposição \[Valor\]".  
* **Valor:** Transforma o compliance de um processo reativo (auditoria semestral) para um monitoramento proativo, contínuo e em tempo real.

**5\. Forense Pós-Evento com Replay Criptográfico**

* **Processo:** Ocorre um incidente de mercado ou uma reclamação de cliente. O auditor de Risco precisa reconstruir os eventos.  
* **Evidência (Veritas):** O auditor *não* lê logs de texto (que podem ser falhos ou incompletos). Ele executa um *software Verificador* 1 que consome a cadeia de VCs do cofre WORM.  
* **Valor:** O auditor pode provar matematicamente a sequência de eventos, quem tomou qual decisão (IA ou humana) e se uma decisão foi revogada (StatusList2021), reduzindo o tempo de forense de semanas para horas.1

## **7\. Prova de Conceito (PoC) Institucional: 15 Dias para a Evidência**

**Público-Alvo:** Todos.

Propomos uma Prova de Conceito (PoC) de 15 dias, focada e objetiva, para validar a arquitetura e suas garantias de conformidade dentro do ambiente BTG Pactual.

O objetivo desta PoC *não* é um relatório de performance, mas a entrega de um **Artefato Criptográfico Verificável** — a prova tangível da trilha de auditoria.

**Escopo Técnico**

1. **Ambiente:** Deploy da infraestrutura (GKE Sandbox, BigQuery WORM, ACL) dentro de um projeto GCP *sandbox* dedicado do BTG, protegido por políticas de VPC-SC e IAM customizadas.  
2. **Dados:** Utilização de 1-2 casos de uso selecionados (recomendado: Onboarding PJ) com dados simulados ou regulados fornecidos pelo BTG.  
3. **Fluxo:** O BTG submete os dados via API. A plataforma Umbrella \+ Veritas 2.0 processa os dados (em modo Zero-Persistence), orquestra a análise de IA, gera as Credenciais Verificáveis (VCs) assinadas e as armazena no Cofre WORM (BigQuery).

## **7\. Prova de Conceito (PoC) (Continuação)**

**Entregáveis-Chave (Prontos para o Time de Risco)**

Ao final dos 15 dias, a FoundLab entregará:

1. **O "Pacote de Evidência":** Um conjunto de Credenciais Verificáveis (em formato JWT/JSON) geradas e assinadas, residindo de forma imutável no cofre BigQuery WORM do BTG.  
2. **O "Software Verificador":** Um script ou aplicação simples (baseado em padrões W3C abertos) entregue ao time de Risco e Compliance do BTG.  
3. **O "Momento da Prova":** O time de Risco/Compliance do BTG, de forma *independente* e *offline*, executará o Verificador contra o Pacote de Evidência. Esta ação irá *provar matematicamente* a autenticidade (via assinatura did:web) e a integridade (via hash) de cada decisão gerada pela IA durante a PoC.  
4. **Relatório de Métricas:** Medição de latência (tempo de processamento e geração de prova), precisão do modelo de IA (vs. gabarito) e *throughput* da geração de evidências.

## **8\. O Modelo de Ativação (B2B2B) via Clientes Orbitais**

**Público-Alvo:** Private Bank, Estratégia, Negócios.

Além do uso interno, a arquitetura Veritas 2.0 habilita um modelo de negócio secundário estratégico, posicionando o BTG Pactual como um *hub* de confiança para seu ecossistema.

Tese Comercial  
Os clientes orbitais do BTG Private Bank (Family Offices, Fundos de Investimento, Gestoras de Patrimônio, Clientes HNW) enfrentam exatamente os mesmos desafios regulatórios (CVM, LGPD) e a mesma pressão por inovação com IA. No entanto, eles não possuem a escala, o capital ou o expertise técnico para construir uma infraestrutura de confiança de nível institucional como o Veritas 2.0.  
A Proposta: BTG como "Integrador e Curador da Confiança"  
O BTG Pactual, após adotar o Veritas 2.0 internamente (Uso Primário), pode oferecer esta mesma infraestrutura como um serviço (B2B2B) aos seus clientes orbitais.  
Ativação Arquitetônica (via VCs)  
Este modelo é nativamente habilitado pela arquitetura de Credenciais Verificáveis 1:

1. Um Family Office (cliente orbital) utiliza a infraestrutura (disponibilizada via API pelo BTG) para processar seu próprio onboarding ou análise de risco.  
2. O sistema emite uma VC (a prova de compliance) que pode ser *co-assinada*:  
   * **Emissor:** did:web:family-office-xyz.com  
   * **Atestador (Opcional):** did:web:compliance.btgpactual.com

## **8\. O Modelo de Ativação (B2B2B) (Continuação)**

Valor para o Cliente Orbital (Family Office / Fundo)  
O cliente orbital ganha acesso sob demanda (modelo híbrido 1\) a uma infraestrutura de IA e compliance de nível tier-1 que ele jamais poderia construir ou manter sozinho. Isso reduz seu custo de compliance e seu risco operacional.  
**Valor Estratégico para o BTG Pactual**

1. **Nova Fonte de Receita (IaaS):** O BTG monetiza o acesso à sua infraestrutura de confiança (modelo de infraestrutura dedicada 1) como um serviço de *Trust-Infrastructure-as-a-Service (IaaS)*.  
2. **Reforço do Ecossistema (Diligência):** Ao "puxar" seus clientes orbitais para seu perímetro de compliance, o BTG aumenta drasticamente a transparência e a diligência em toda a sua cadeia de valor, reduzindo o risco sistêmico de todo o ecossistema.  
3. **Diferenciação Estratégica:** O Private Bank deixa de oferecer apenas produtos financeiros e passa a oferecer "Conformidade como Ativo". Isso transforma um centro de custo (compliance) em um diferencial competitivo e um produto de alto valor agregado.

## **9\. ROI Estratégico: O Alfa Operacional (ROI\_E)**

**Público-Alvo:** Executivos, Negócios, Finanças.

A IA tradicional promete Retorno Sobre Investimento (ROI). A plataforma FoundLab entrega **ROI\_E** – um Retorno Sobre Investimento lastreado em *Evidência* criptográfica.

**A Fórmula: ROI\_E \= (Ganhos de Automação) \+ (Redução de Risco Custo) / Evidência (DecisionID \+ VC)**

O lastro (a Evidência) é o que garante que os ganhos de automação (velocidade, redução de FTE) não sejam anulados por uma única multa regulatória ou falha de auditoria. A prova 1 é o colateral que garante o ROI.

Prova de Conceito (Case: Elite Capital)  
A FoundLab já validou esta tese em produção. Nossa implementação na Elite Capital, uma gestora de patrimônio que opera na infraestrutura NECTON/BTG Pactual, transformou seu passivo regulatório em "Alfa Operacional".1  
**O Resultado:** O processo de análise regulatória e scoring de portfólio, que consumia **21 horas** de trabalho manual, foi reduzido para **16 minutos** de processamento automatizado, auditável e com geração de prova criptográfica.1

Isso representa uma redução de **98.7%** no tempo de processamento.1

## **9\. ROI Estratégico (Continuação)**

Tabela: Métricas de Alfa Operacional (Base Elite Capital)  
Esta tabela, baseada em dados de implementação 1, quantifica o impacto da automação auditável em métricas críticas de negócio e risco. O valor não está apenas em "ser mais rápido" (redução de tempo), mas em "ser mais preciso" (redução de erro) e "ter mais cobertura de auditoria" (prova).

| Métrica | Descrição | Valor (Antes da FoundLab) | Valor (Depois da FoundLab) | Impacto |
| :---- | :---- | :---- | :---- | :---- |
| **Tempo / Processo Regulatório** | Tempo médio para gerar relatório regulatório | 8 Horas | 2 Horas | 75% Redução |
| **Tempo / Scoring de Portfólio** | Tempo médio para gerar score de risco da carteira | 45 Minutos | 5 Minutos | 88.9% Redução |
| **Precisão das Decisões (IA)** | Percentual de decisões corretas (IA vs. Humano) | 85% | 98% | \+15.3% Aumento |
| **Taxa de Erro (Manual)** | Erros de cálculo ou inconsistências (Manual) | 15 / Mês | 2 / Mês | 86.7% Redução |
| **Cobertura de Auditoria** | Percentual de operações auditáveis com prova | 60% | 95% | \+58.3% Aumento |
| **Conformidade de Regras** | % de políticas aplicadas corretamente | 70% | 99% | \+41.4% Aumento |
| Resultado Composto 1 | Cálculo do case 1 | *\~21 Horas* | *\~16 Minutos* | **98.7% Redução** |

## **10\. Próximos Passos: Ativação da Prova de Conceito**

**Público-Alvo:** Todos.

Para validar as teses de arquitetura, conformidade e performance, propomos a execução imediata da Prova de Conceito (PoC) de 15 dias.

O objetivo é validar as métricas de latência, precisão e, crucialmente, a geração e verificação da **Evidência Criptográfica (VC)**.

**Ações Imediatas Propostas:**

1. **Sessão de Arquitetura (D+1):** Reunião de 1 dia entre os arquitetos de nuvem da FoundLab e do BTG Pactual para definir o escopo da *sandbox* GCP e os requisitos de VPC-SC, IAM e CMEK.  
2. **Definição do Caso de Uso (D+2):** Seleção conjunta (Times de Negócio BTG \+ FoundLab) de 1-2 casos de uso para a PoC (recomendado: Onboarding PJ ou Análise de Fundos).  
3. **Provisionamento e Deploy (D+3 a D+5):** FoundLab (via Terragrunt/IaC) provisiona a infraestrutura *dentro* do projeto GCP sandbox do BTG.  
4. **Execução e Geração de Provas (D+6 a D+13):** Execução dos *jobs* de IA com dados simulados; geração do "Pacote de Evidência" (VCs) e armazenamento no cofre WORM.  
5. **Apresentação dos Resultados (D+15):** Sessão de validação, onde o time de Risco do BTG utilizará o "Software Verificador" para validar as provas criptográficas geradas.

**Equipes Requeridas:**

* **BTG Pactual:** Patrocínio executivo, Arquiteto de Nuvem (GCP), Líder de Risco/Compliance, Líder de Produto (Private Bank).  
* **FoundLab:** Arquiteto de Soluções, Engenheiro de Infraestrutura.

## **Apêndice: O Ciclo de Vida de uma Decisão Auditável (Veritas 2.0)**

**Público-Alvo:** Técnico, Risco, Arquitetura.

Este diagrama de sequência 1 resume o ciclo de vida completo de uma decisão, desde a ingestão até a resolução do paradoxo regulatório (exclusão).

Snippet de código

sequenceDiagram  
    participant Sistema as Sistema Legado (Ex: Onboarding)  
    participant ACL as ACL (Emissor Veritas / gVisor)  
    participant BQ as Cofre WORM (BigQuery)  
    participant KMS as Cloud KMS (Chave CMEK)  
    participant Auditor as Auditor / Regulador  
    participant Compliance as Time de Compliance BTG

    %% Fase 1: Emissão e Armazenamento (SOX/CVM) %%  
    Sistema-\>\>ACL: 1\. Envia dados (Ex: Onboarding PJ)  
    ACL-\>\>ACL: 2\. Processa em RAM (Zero-Persistence)  
    ACL-\>\>ACL: 3\. Gera VC assinada (did:web)  
    ACL-\>\>KMS: 4\. Solicita criptografia  
    KMS--\>\>ACL: Chave fornecida  
    ACL-\>\>BQ: 5\. Escreve VC criptografada (IAM Append-Only)  
    Note over BQ: Registro agora é WORM (SOX/CVM)  
      
    %% Fase 2: Auditoria / Verificação %%  
    Auditor-\>\>BQ: 6\. Recupera VC Criptografada  
    Auditor-\>\>KMS: 7\. Solicita descriptografia  
    KMS--\>\>Auditor: Dados descriptografados  
    Auditor-\>\>Auditor: 8\. Verifica assinatura (via did:web)  
    Note over Auditor: "Prova matemática de integridade e não-negação"

    %% Fase 3: Exclusão (Paradoxo Regulatório \- LGPD) %%  
    Compliance-\>\>KMS: 9\. Ordem de Exclusão (LGPD)  
    KMS-\>\>KMS: 10\. Destrói Chave CMEK (Crypto-Shred)  
      
    %% Fase 4: Pós-Exclusão %%  
    Auditor-\>\>BQ: 11\. Tenta ler o mesmo registro  
    BQ--\>\>Auditor: VC Criptografada  
    Auditor-\>\>KMS: 12\. Solicita descriptografia  
    KMS--\>\>Auditor: 13\. Nega (Chave destruída)  
    Note over Auditor: "Dado é permanentemente ilegível (LGPD),\<br\>mas o blob físico persiste (SOX/CVM)."