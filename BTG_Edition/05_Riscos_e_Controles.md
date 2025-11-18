# 05. Análise de Riscos e Controles Técnicos

A arquitetura da FoundLab é projetada para mitigar riscos de IA, conformidade e segurança em múltiplas camadas. Esta seção detalha os riscos inerentes à adoção de IA em ambientes regulados e os controles técnicos específicos implementados pela plataforma.

## Risco 1: Alucinação e Decoerência do Modelo de IA

* **Descrição do Risco:** Modelos de Linguagem Grandes (LLMs) são probabilísticos e podem gerar respostas factualmente incorretas, inconsistentes ou que violam políticas internas ("alucinações"). Uma decisão de negócio baseada em uma alucinação pode levar a perdas financeiras e violações de conformidade.

* **Controle FoundLab (Guardian AI / Schema Enforcer):**
  * **Validação de Schema Rígida:** A arquitetura força o LLM a colapsar sua resposta em um **schema JSON determinístico e pré-definido**. Se o modelo gerar uma resposta que viole o contrato de dados (ex: um campo obrigatório ausente, um tipo de dado incorreto), a transação falha de forma segura (*fail-safe*) com um erro 422 (Unprocessable Entity).
  * **Critic-Loop:** Um agente de IA "crítico" revisa as saídas das IAs "analistas" para detectar inconsistências lógicas *antes* que a decisão final seja retornada, adicionando uma camada de verificação semântica.
  * **Impacto:** Este controle impede que dados corrompidos ou "alucinados" entrem nos sistemas de produção do BTG, transformando a IA de uma "caixa-preta" em um sistema com saídas previsíveis e estruturadas.

## Risco 2: Falha de Auditoria e Não-Repúdio

* **Descrição do Risco:** Reguladores (BACEN, CVM, SOX) exigem uma trilha de auditoria irrefutável. Logs de texto tradicionais são frágeis, alteráveis e não fornecem prova de "não-repúdio" (a garantia de que uma parte não pode negar ter executado uma ação).

* **Controle FoundLab (Protocolo Veritas 2.0):**
  * **Prova Matemática Imutável:** Cada decisão gera um `DecisionID` com um pacote de evidências (metadados não-PII) que é selado com uma **assinatura criptográfica (JWS)**.
  * **Cofre WORM (Write-Once, Read-Many):** A prova assinada é persistida em uma tabela do **Google BigQuery** configurada em modo WORM, com políticas de IAM que negam explicitamente qualquer alteração ou exclusão.
  * **Impacto:** A auditoria deixa de ser um exercício forense reativo e se torna uma **verificação matemática instantânea**. O BTG pode provar deterministicamente qual política, modelo e dados foram usados para cada decisão, satisfazendo os mais altos requisitos de auditoria.

## Risco 3: Vazamento e Exfiltração de Dados (LGPD)

* **Descrição do Risco:** O uso de IA em dados sensíveis de clientes cria um passivo tóxico. A persistência de prompts ou respostas contendo PII em logs ou caches viola o princípio de minimização de dados da LGPD e cria um alvo para ataques.

* **Controle FoundLab (Zero-Persistence e Perímetro Soberano):**
  * **Zero-Persistence:** Dados sensíveis são processados **exclusivamente em memória volátil (RAM)**. Eles nunca tocam o disco na camada de aplicação, eliminando arquitetonicamente o risco de vazamento de banco de dados.
  * **Perímetro Soberano (VPC Service Controls):** A infraestrutura opera dentro de um perímetro de segurança no GCP do BTG que **bloqueia por padrão toda a tentativa de exfiltração de dados**, mesmo por um *insider* ou uma credencial comprometida.
  * **Impacto:** A plataforma oferece conformidade nativa com a LGPD e protege o ativo mais valioso do banco: os dados de seus clientes.

## Risco 4: Paradoxo Regulatório (SOX vs. LGPD)

* **Descrição do Risco:** O conflito arquitetônico entre o mandato de "nunca apagar" (SOX) e "apagar sob demanda" (LGPD) paralisa a inovação.

* **Controle FoundLab (Desacoplamento Criptográfico):**
  * **Crypto-Shredding via CMEK:** A solução desacopla a existência física dos dados de sua legibilidade. Para cumprir a LGPD, o BTG **destrói a chave de criptografia (CMEK)**. O dado cifrado permanece fisicamente no cofre WORM (cumprindo SOX), mas torna-se "lixo criptográfico" permanentemente ilegível (cumprindo LGPD).
  * **Impacto:** Transforma um problema jurídico insolúvel em uma solução de engenharia de chaves, onde o controle do "kill switch" regulatório está 100% nas mãos do BTG.
