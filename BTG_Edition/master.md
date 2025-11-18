# Proposta de Infraestrutura de Confiança Auditável (ATI)
## Edição BTG Pactual

**PARA:** Comitê Executivo, Risco, Compliance e Tecnologia do BTG Pactual
**DE:** FoundLab
**ASSUNTO:** Adoção da Infraestrutura de Confiança Auditável (ATI) como camada de infraestrutura padrão para Governança de IA e Conformidade Regulatória (SOX, LGPD, BACEN).

---

### 1. Sumário Executivo

A FoundLab propõe a adoção da **Infraestrutura de Confiança Auditável (ATI)**, uma camada de infraestrutura nativa do Google Cloud (GCP) projetada para resolver o "Paradoxo Regulatório" (LGPD vs. SOX/BACEN). Nossa arquitetura permite que o BTG Pactual inove com IA Generativa sobre dados sensíveis, garantindo conformidade determinística.

A solução, implantada 100% dentro da VPC do BTG (**Modelo de Soberania Absoluta**), utiliza **Zero-Persistence** (processamento de PII em RAM) e o **Protocolo Veritas 2.0** (provas imutáveis em BigQuery WORM). Isso transforma o compliance de um custo operacional em um "Alfa Operacional" mensurável, validado por uma **redução de 98,7%** no tempo de ciclo de processos de conformidade (de 21 horas para 16 minutos) em um caso de uso análogo no ecossistema Necton/BTG.

---

### 2. A Narrativa Central: Resolvendo o Paradoxo Regulatório

A inovação em IA no setor financeiro está bloqueada por um conflito legal e arquitetônico:

1.  **Mandato de Retenção (SOX, BACEN 4.893, CVM):** Exige que trilhas de auditoria sejam mantidas em formato imutável (WORM) por 5-10 anos. A regra é: **"NUNCA EXCLUIR"**.
2.  **Mandato de Privacidade (LGPD):** Concede o "Direito ao Esquecimento", exigindo que dados pessoais (PII) sejam permanentemente excluídos sob demanda. A regra é: **"EXCLUIR SOB DEMANDA"**.

Um sistema tradicional não pode obedecer a ambas as regras. A IA Generativa, que é *probabilística*, agrava o problema, pois a auditoria exige prova *determinística*.

#### 2.1. A Falha das Abordagens Atuais (Tese 1.0)

A tentativa comum de "anonimizar" PII em logs de auditoria através de *hashing* é legalmente falha e cria um risco duplo:

* **Violação da Privacidade (FTC/LGPD):** Reguladores como a Federal Trade Commission (FTC) dos EUA são explícitos: "hashing não é anonimização". Como o *hash* é determinístico, ele permite o rastreamento e ainda é considerado PII.
* **Violação da Auditoria (SOX/BACEN):** Um *hash* é inútil para um auditor, que precisa dos dados originais para verificar a lógica de negócios. Processar PII em RAM e descartá-lo ("Zero-Persistence 1.0") é funcionalmente "destruição de evidência como serviço", uma violação do mandato de retenção (DOJ 18 U.S.C. § 1519).

#### 2.2. A Tese FoundLab: "Compliance-as-Infrastructure"

A conformidade não deve ser uma aplicação reativa ("trem"), mas uma propriedade da infraestrutura subjacente ("trilhos"). Nossa plataforma, **ATI**, é essa camada de infraestrutura.

#### 2.3. A Solução (ATI): Desacoplamento Criptográfico

A arquitetura ATI resolve o paradoxo ao desacoplar a *existência física* do dado de sua *legibilidade criptográfica*.

1.  **Princípio de Zero-Persistence (ZPA):** Dados sensíveis (PII) são processados exclusivamente em memória volátil (RAM) dentro de contêineres *serverless* (ex: Cloud Run). O PII nunca toca em disco na camada de aplicação, garantindo a minimização de dados da LGPD.
2.  **Protocolo Veritas 2.0 (Prova WORM):** A *prova* da transação (metadados não-PII, como `decision_id`, `timestamp`, `policy_version` e *hashes* de evidência) é selada e registrada em um cofre de dados imutável (**Google BigQuery WORM**) para cumprir os mandatos de retenção SOX/BACEN.
3.  **A Solução do Paradoxo (Crypto-Shredding):** Para habilitar o "Direito ao Esquecimento" da LGPD sem violar a retenção WORM da SOX, todo o cofre de dados é criptografado com uma **Chave de Criptografia Gerenciada pelo Cliente (CMEK)**, controlada 100% pelo BTG.
    * **Para Reter (SOX):** O dado físico (criptografado) permanece no cofre WORM.
    * **Para Esquecer (LGPD):** O BTG Pactual **destrói a chave CMEK**. O dado físico permanece (cumprindo SOX), mas torna-se "lixo criptográfico" permanentemente ilegível, cumprindo o mandato de exclusão da LGPD.

Este mecanismo transforma um dilema legal insolúvel em um processo de engenharia de chaves controlado pelo banco.

---

### 3. A Arquitetura Técnica (GCP Edition)

Propomos exclusivamente o **Modelo de Soberania Absoluta (Dedicated)**: a plataforma ATI é implantada via Infraestrutura-como-Código (IaC) diretamente dentro do projeto e da VPC do Google Cloud do BTG Pactual.

Esta arquitetura garante 100% de soberania de dados e atende diretamente à Resolução BACEN 4.893 através da "Trindade da Confiança" do GCP:

1.  **Soberania de Perímetro (VPC Service Controls - VPC-SC):**
    * **O que é:** Um "firewall para as APIs do Google" que cria um perímetro de segurança.
    * **Função:** Bloqueia arquitetonicamente *toda* tentativa de exfiltração de dados (ex: uma cópia do BigQuery para um *bucket* público), mesmo por credenciais comprometidas.

2.  **Soberania de Chaves (Cloud KMS - CMEK/HSM):**
    * **O que é:** Gerenciamento de Chaves de Criptografia pelo Cliente (HSM FIPS 140-2 Nível 3 opcional).
    * **Função:** O BTG Pactual detém 100% de controle sobre as chaves que criptografam o cofre de auditoria. A destruição da chave é o "Kill Switch" regulatório que habilita o *Crypto-Shredding*.

3.  **Soberania de Conexão (Private Service Connect - PSC):**
    * **O que é:** Conectividade de serviço privada e unidirecional.
    * **Função:** Garante que todo o tráfego de dados entre as aplicações e o cofre BigQuery WORM ocorra inteiramente na rede *backbone* privada do Google, nunca atravessando a internet pública.

#### 3.1. Componentes da Camada de Prova (Protocolo Veritas)

A imutabilidade WORM (Write-Once, Read-Many) é aplicada por engenharia, não por política:

* **Cofre WORM (BigQuery):** O *dataset* de auditoria é configurado com `deletion_protection=true` via IaC (Terraform/Terragrunt).
* **Controle de Acesso (IAM):** Uma Função de IAM personalizada (`Veritas-WORM-Ingestor`) é aplicada ao serviço de ingestão. Esta função permite *apenas* `bigquery.tables.insertData` (anexar) e **nega explicitamente** `bigquery.tables.updateData` e `bigquery.tables.deleteData`.

#### 3.2. Componentes da Camada de Decisão (Plataforma Umbrella)

A "Fábrica de Decisão" que orquestra a IA de forma auditável:

* **Compute:** Contêineres *serverless* e *stateless* (Cloud Run ou GKE com gVisor) para executar o processamento Zero-Persistence.
* **IA e RAG:** O orquestrador usa modelos do **Vertex AI (Gemini)** ancorados em dados internos via **Vertex AI Vector Search** (RAG) para garantir relevância e mitigar alucinações.
* **Controle de IA:** O "Guardian AI" força as saídas do modelo a aderirem a um *schema* JSON predefinido, garantindo respostas estruturadas e prevenindo "alucinação estrutural".

---

### 4. O Caso de Uso Primário para BTG

A plataforma ATI desbloqueia imediatamente dois vetores de valor:

1.  **Eficiência: Automação de Compliance (KYC/Onboarding)**
    * **Dor:** Processos manuais de onboarding, análise documental e KYC são lentos, caros e propensos a erros.
    * **Solução:** A plataforma **Umbrella** usa IA (Document AI, Gemini) para analisar e extrair dados de documentos complexos. O **Veritas 2.0** registra cada etapa e decisão como uma prova imutável no cofre WORM, gerando o "Alfa Operacional".

2.  **Inovação: "BTG GPT" Interno e Auditável (Private Bank)**
    * **Dor:** A impossibilidade de usar IA Generativa em dados sensíveis de clientes (ex: PII, posições de carteira) devido ao risco de vazamento de dados e falta de auditabilidade.
    * **Solução:**
        * O **Umbrella (RAG)** ancora a consulta do assessor (ex: "Simular alocação para Cliente X") com dados *apenas* de fontes internas autorizadas.
        * O **Zero-Persistence** garante que o PII da consulta exista *apenas em RAM* durante a inferência (milissegundos) e nunca seja logado.
        * O **Veritas 2.0** *não* salva a conversa (o PII). Ele salva a *prova da ação* (os metadados: `assessor_id`, `timestamp`, `policy_version`, `model_version` usados), criando uma trilha de auditoria determinística sem criar um passivo de PII.

---

### 5. Simulações e Benefícios Mensuráveis

A tese de "Alfa Operacional" é validada por um caso de uso em produção no ecossistema Necton/BTG (Elite Capital).

* **Métrica de Eficiência:** Redução de **98,7%** no tempo de ciclo de conformidade (de **21 horas** de trabalho manual para **16 minutos** de processamento ATI).
* **Métrica de Risco:** Redução de **94%** na taxa de erro manual (de 42% para 2,5%).

#### 5.1. Análise de ROI Quantificada

Baseado nessas métricas, o benefício financeiro de automatizar um processo de conformidade manual é de **R$ 4.409,50 por processo** (R$ 3.109,50 em economia de tempo de analista sênior + R$ 1.300,00 em mitigação de risco de erro).

| Métrica Financeira | Cenário 10.000 Processos/Mês | Cenário 50.000 Processos/Mês |
| :--- | :--- | :--- |
| Modelo de Custo | Hybrid (Variável) | **Dedicated (Fixo)** |
| Custo Anualizado (TCO) | R$ 1,20 Milhões | R$ 2,84 Milhões |
| Retorno Anual (Benefícios) | R$ 529,14 Milhões | **R$ 2,645 Bilhões** |
| **Custo Efetivo por Processo** | **R$ 9,97** | **R$ 4,16** |
| **VPL (3 Anos, 12% WACC)** | **R$ 1,268 Bilhões** | **R$ 6,346 Bilhões** |
| **Payback** | **< 1 Mês** | **< 1 Mês** |

**Conclusão Financeira:** O investimento se paga quase instantaneamente. O modelo "Dedicated" (Soberania Absoluta) desbloqueia uma alavancagem operacional massiva, reduzindo o custo por processo em 73,5% em escala institucional.

---

### 6. Riscos & Controles

A adoção da arquitetura ATI mitiga riscos legais catastróficos, mas introduz novos riscos operacionais que exigem controles rigorosos:

* **Risco Operacional: Gerenciamento da Chave Mestra (CMEK)**
    * **Descrição:** O "Kill Switch" (CMEK) é a maior força da solução, mas também seu maior risco. Se a chave CMEK for perdida ou destruída acidentalmente, os dados de auditoria criptografados tornam-se **irrecuperáveis**.
    * **Controle Recomendado:** A posse e o ciclo de vida da chave CMEK (preferencialmente em **Cloud HSM**) devem ser de propriedade exclusiva da equipe de **Risco e Compliance** do BTG, estritamente segregada das equipes de Engenharia/DevOps.

* **Risco Operacional: Integridade da Prova Criptográfica**
    * **Descrição:** A capacidade de verificar as assinaturas criptográficas das provas de auditoria depende da integridade dos metadados de chaves públicas do BTG.
    * **Controle Recomendado:** Esses metadados de verificação devem ser tratados como infraestrutura de Nível 0, com os mesmos controles rigorosos dos certificados TLS raiz do banco.

---

### 7. Documentos Anexos (Referência)

* `ATI_Whitepaper_Técnico.md` (Baseado em Veritas_2.0_Whitepaper.md)
* `ATI_Modelo_Soberania_GCP.md` (Baseado em ModeloDedicated.md)
* `ATI_Simulação_ROI_Detalhada.md` (Baseado em Simulação ROI FoundLab para BTG Pactual.md)
* `ATI_Plano_PoC_15_Dias.md` (Baseado em Deck Bancário para IA AuditávelV1.md)
