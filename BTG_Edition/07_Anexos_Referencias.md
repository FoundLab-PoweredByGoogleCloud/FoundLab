# 07. Glossário Técnico e Anexos

Este documento serve como um glossário e repositório para os conceitos técnicos e arquitetônicos fundamentais da plataforma FoundLab.

---

### 1. Conceitos Fundamentais

* **Infraestrutura de Confiança Auditável (ATI) / Auditable Trust Infrastructure:** A tese central da FoundLab. Refere-se a uma arquitetura onde a conformidade e a confiança não são processos manuais, mas propriedades matemáticas e determinísticas da própria infraestrutura.

* **Zero-Persistence:** Um princípio de segurança radical onde dados sensíveis (PII) são processados **exclusivamente em memória volátil (RAM)** e nunca são armazenados em disco (*data-at-rest*) na camada de aplicação. Isso elimina arquitetonicamente a classe de risco de vazamentos de banco de dados.

* **Protocolo Veritas 2.0:** A camada de prova da FoundLab. Transforma logs de texto frágeis em provas matemáticas irrefutáveis. Seus principais componentes são o `DecisionID` e o cofre WORM.

* **DecisionID / Registro de Decisão:** O artefato de auditoria gerado pelo Veritas 2.0. É um registro de **metadados não-PII** que contém informações como `actor_id`, `timestamp`, `policy_version`, `model_version` e hashes criptográficos dos inputs e outputs. Ele é assinado digitalmente para garantir não-repúdio e integridade.

* **WORM (Write-Once, Read-Many):** Um modelo de armazenamento de dados que garante a imutabilidade. Uma vez que os dados são escritos, eles não podem ser alterados ou excluídos por um período definido. É um requisito fundamental de regulamentações como a SOX e a CVM para trilhas de auditoria. A FoundLab implementa o WORM no Google BigQuery através de controles de IaC e IAM.

---

### 2. Arquitetura de Soberania (Google Cloud)

* **Modelo Dedicated/VPC:** O modelo de implantação proposto para o BTG Pactual, onde a infraestrutura da FoundLab é instalada como **Infrastructure as Code (IaC)** diretamente no projeto e na VPC do Google Cloud do banco, garantindo soberania total sobre dados, chaves e logs.

* **VPC Service Controls (VPC-SC):** Um "firewall para as APIs do Google" que cria um perímetro de segurança em nível de nuvem. Ele bloqueia por padrão qualquer tentativa de exfiltração de dados para fora do perímetro definido pelo BTG, mitigando riscos de *insiders* ou credenciais comprometidas.

* **CMEK (Customer-Managed Encryption Keys):** Chaves de criptografia controladas 100% pelo cliente (BTG) no Google Cloud KMS ou HSM. O BTG retém a propriedade e o controle sobre o ciclo de vida da chave.

* **Crypto-Shredding:** A solução técnica para o paradoxo regulatório (SOX vs. LGPD). Em vez de excluir um registro físico (o que violaria o WORM), o BTG **destrói a chave CMEK**. O dado cifrado permanece no cofre WORM, mas torna-se permanentemente ilegível, cumprindo funcionalmente o "direito ao esquecimento" da LGPD.

---

### 3. Inteligência Artificial e Controles

* **RAG (Retrieval-Augmented Generation):** Uma técnica de IA que "ancora" um modelo de linguagem em um conjunto de dados específico e autorizado. O **RAG Institucional** da FoundLab garante que a IA opere apenas com fontes de dados internas e vetadas pelo BTG, aumentando a precisão e a segurança.

* **Schema Enforcer / Guardian AI:** Um controle técnico que força a saída do modelo de IA a aderir a um **schema JSON determinístico**. Se o modelo "alucinar" ou gerar uma resposta malformada, a transação falha de forma segura (*fail-safe*), protegendo os sistemas de produção.

* **AI Flywheel:** Um ciclo de aprendizado contínuo. Quando um humano corrige uma decisão da IA (`HUMAN_OVERRIDE`), esse evento é registrado criptograficamente no Veritas e usado como *ground truth* para retreinar e aprimorar o modelo de forma auditável.
