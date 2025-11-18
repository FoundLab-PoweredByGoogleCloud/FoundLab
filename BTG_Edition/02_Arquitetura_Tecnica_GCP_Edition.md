# 02. Arquitetura Técnica de Soberania (Google Cloud Edition)

## Visão Geral

A arquitetura FoundLab é uma solução de **defesa em profundidade**, projetada para ser implantada como uma instância *single-tenant* (Dedicada) diretamente no perímetro soberano do BTG Pactual. A solução é entregue como **Infrastructure as Code (IaC)** e opera inteiramente dentro da VPC e do projeto Google Cloud do banco.

## A Trindade da Confiança: Soberania no Perímetro do BTG

A arquitetura de soberania é mantida pela **"Trindade da Confiança"**:

1. **Pilar 1: O Perímetro de Rede (VPC Service Controls - VPC-SC):**
    * **Função:** O "muro do castelo". O VPC-SC atua como um "firewall para as APIs do Google", criando um perímetro de segurança que bloqueia *qualquer* tentativa de exfiltração de dados no nível da infraestrutura, mitigando o risco de *insiders* ou credenciais comprometidas.

2. **Pilar 2: O Perímetro de Chaves (CMEK/HSM):**
    * **Função:** O "kill switch" regulatório. O BTG utiliza e controla 100% de suas próprias **Chaves de Criptografia Gerenciadas pelo Cliente (CMEK)**. A aplicação FoundLab recebe apenas a permissão para *usar* a chave, mas o BTG retém a *propriedade* da chave. A destruição da chave (crypto-shredding) torna os dados ilegíveis, cumprindo a LGPD sem violar a retenção WORM.

3. **Pilar 3: O Perímetro de Prova (Logs Soberanos e PSC):**
    * **Função:** O "caminho seguro" e o "cofre do cartório". O **Private Service Connect (PSC)** garante que toda a comunicação ocorra dentro da rede privada do Google. A trilha de auditoria do Veritas 2.0 é escrita *diretamente* no projeto **BigQuery WORM do próprio banco**.

## O Protocolo Veritas 2.0: O Cartório Técnico Não-Custodial (Versão TradFi)

O Veritas 2.0 gera o artefato central de auditoria: o **"Registro de Decisão" (`DecisionID`)**. O design mais crítico é que este registro armazena **metadados, não PII (Informações de Identificação Pessoal)**.

**Anatomia do Registro de Decisão (`DecisionID`):**

| Campo | Descrição | Propósito |
| :--- | :--- | :--- |
| `decision_id` | Identificador único (UUID) da decisão | Rastreabilidade |
| `actor_id` | Quem (usuário/serviço) solicitou a decisão | Responsabilidade |
| `timestamp_verified` | O *timestamp* exato da decisão | Ordem cronológica |
| `policy_version` | O hash/versão da política de risco usada | Replay Determinístico |
| `model_version` | O hash/versão do modelo de IA usado | Replay Determinístico |
| `input_hash` | O *hash* (impressão digital) dos dados de entrada (NÃO o PII) | Integridade |
| `output_hash` | O *hash* da resposta ou a decisão final | Prova do resultado |
| `jws_signature` | A assinatura criptográfica que sela o registro | Imutabilidade |

A garantia de imutabilidade **WORM (Write-Once, Read-Many)** é aplicada tecnicamente em duas camadas no BigQuery:

1. **IaC (Terraform):** Define a tabela com `deletion_protection = true`.
2. **IAM Custom Role:** Nega explicitamente as permissões `bigquery.tables.updateData` e `bigquery.tables.deleteData`, permitindo apenas `bigquery.tables.insertData`.

Este design permite o "Replay Determinístico": um auditor pode reproduzir o `DecisionID` anos depois para provar deterministicamente qual política e modelo foram usados, tornando a IA auditável a longo prazo.
