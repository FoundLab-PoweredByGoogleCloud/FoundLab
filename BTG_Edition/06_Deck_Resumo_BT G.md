# 06. Resumo Executivo (Formato Deck)

Este documento resume a estrutura da apresentação sobre a Infraestrutura de Confiança Auditável (ATI) da FoundLab, adaptada para o contexto do BTG Pactual.

---

### 1. O Paradoxo do Banco: IA Travada por Risco Regulatório

* **Problema Central:** A IA Generativa é **probabilística**, mas a conformidade regulatória (BACEN, CVM, SOX) exige prova **determinística**.
* **Conflito Legal:** Tensão entre o mandato de **retenção imutável** (SOX) e o **direito ao esquecimento** (LGPD).
* **Falha da Infra Legada:** Arquiteturas atuais baseadas em logs de texto são frágeis e inauditáveis. Modelos SaaS multi-tenant são um risco inaceitável para dados *core*.
* **Conclusão:** O bloqueador da inovação não é a falta de IA, mas a **ausência de uma infraestrutura que torne a IA auditável e segura**.

---

### 2. A Tese da FoundLab: De Probabilístico para Determinístico

* **Silogismo de Valor:** IA de valor exige dados sensíveis; dados sensíveis exigem prova irrefutável. Portanto, IA institucional = **Infraestrutura de Confiança (Umbrella) + Trilha de Prova Determinística (Veritas 2.0)**.
* **A Solução:** "Envelopar" a chamada de IA (probabilística) com uma trilha de auditoria (determinística).
* **Nova Categoria:** "Compliance-as-Infrastructure". Não somos uma RegTech que automatiza processos; construímos os **"trilhos da próxima geração"** sobre os quais as futuras aplicações de IA devem operar.

---

### 3. Arquitetura da Solução: A Trindade da Confiança

* **Modelo para BTG:** **Dedicated/VPC**, implantado como Infrastructure as Code (IaC) diretamente no projeto Google Cloud do banco.
* **Trindade da Soberania:**
    1. **Perímetro de Rede (VPC-SC):** O "muro do castelo" que bloqueia a exfiltração de dados.
    2. **Perímetro de Chaves (CMEK/HSM):** O "kill switch" regulatório. O BTG controla 100% das chaves; a destruição da chave (crypto-shredding) cumpre a LGPD.
    3. **Perímetro de Prova (Logs Soberanos):** A trilha de auditoria Veritas 2.0 é escrita no BigQuery WORM do próprio BTG, garantindo soberania e acesso direto para o regulador (BACEN Res. 4.893).

---

### 4. O Protocolo Veritas 2.0: O Cartório Técnico

* **Ativo de Auditoria:** Gera o **`DecisionID`**, um registro de decisão que armazena **metadados, não PII**.
* **Camadas de Imutabilidade (WORM):**
  * **Infraestrutura (IaC):** Tabela do BigQuery com `deletion_protection=true`.
  * **Aplicação (IAM):** Negação explícita de permissões de `UPDATE` ou `DELETE`.
* **Resultado:** Permite o **"Replay Determinístico"**, tornando a IA auditável a longo prazo.

---

### 5. Casos de Uso e ROI (Alpha Operacional)

* **Caso de Uso 1 (Eficiência): Onboarding/KYC Automatizado**
  * **Solução:** Umbrella (IA) automatiza a análise de documentos; Veritas 2.0 gera a prova de conformidade imutável.
  * **ROI (Validado):** Redução de **98.7%** no tempo de ciclo (21h → 16min) e **94%** na taxa de erro.
* **Caso de Uso 2 (Inovação): "BTG GPT" Interno Auditável**
  * **Solução:** RAG Institucional + **Zero-Persistence** (PII apenas em RAM) + Prova de Ação Veritas (metadados).
  * **Resultado:** Desbloqueia o uso de IA Generativa sobre dados sensíveis de clientes de forma segura e auditável.

---

### 6. Chamado à Ação: Prova de Conceito (PoC) em 15 Dias

* **Proposta:** Uma PoC de 15 dias focada em validação técnica e de risco.
* **Objetivo:** Gerar o primeiro **"Pacote de Evidência" (`DecisionID`)** verificável *dentro* do ambiente sandbox do BTG.
* **Entregáveis:** O Pacote de Evidência no BigQuery WORM do BTG e um "Software Verificador" para a equipe de Risco validar criptograficamente a prova.
