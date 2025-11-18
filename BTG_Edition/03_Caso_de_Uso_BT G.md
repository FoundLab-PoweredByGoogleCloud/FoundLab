# 03. Casos de Uso Estratégicos para o BTG Pactual

A plataforma FoundLab não é apenas uma infraestrutura de conformidade, mas um habilitador de valor de negócio, entregando "alpha operacional" e "alpha de inovação".

## Caso de Uso 1: Onboarding/KYC Automatizado (Alpha de Eficiência)

* **Dor:** Processos de Onboarding e KYC (Know Your Customer) são manuais, lentos, caros e propensos a erros humanos, além de gerarem trilhas de auditoria frágeis baseadas em logs de texto e planilhas.

* **Solução:**
    1. **Umbrella (Orquestrador de IA):** Automatiza a análise de documentos complexos de onboarding (contratos sociais, balanços) usando ferramentas como o Google Document AI.
    2. **Veritas 2.0 (Camada de Prova):** Gera uma prova de conformidade imutável (`DecisionID`) para cada etapa do processo, armazenada no cofre WORM do BTG.

* **ROI (Validado no ecossistema BTG):**
  * **Tempo de Ciclo:** Redução de **21 horas para 16 minutos** (98.7%).
  * **Taxa de Erro:** Redução de **42% para 2.5%** (94%).
  * **Prova de Auditoria:** Migração de logs frágeis para um registro `DecisionID` imutável e deterministicamente defensável.

## Caso de Uso 2: "BTG GPT" Interno Auditável (Alpha de Inovação)

* **Dor:** O BTG deseja habilitar seus assessores e equipes internas com IA Generativa para analisar dados de clientes (ex: "simule uma alocação de carteira para o cliente X"). Isso é atualmente bloqueado pelo risco de vazar PII para o provedor do modelo e pela falta de uma trilha de auditoria.

* **Solução (A "Tríade" de IA Segura):**
    1. **Umbrella (RAG Institucional):** Busca os dados do cliente (PII) *internamente* e os injeta no *prompt* da IA, garantindo que o modelo opere apenas com dados vetados pelo banco.
    2. **Zero-Persistence:** A consulta + dados PII existem *apenas em memória volátil (RAM)* durante a chamada à API do modelo. O PII nunca é logado pelo provedor ou pela infraestrutura da FoundLab.
    3. **Veritas 2.0 (Prova de Ação):** O sistema *não* registra a conversa (o PII). Ele registra a *ação* (metadados): "Assessor `[actor_id]` executou simulação para cliente `[input_hash]` às `[timestamp]` usando política `[policy_version]`".

* **Resultado:** Este caso de uso desbloqueia a inovação que hoje está parada nas mesas de Risco e Jurídico. Permite que o BTG Pactual use IA Generativa com segurança sobre seus dados mais sensíveis, mantendo uma trilha de auditoria completa da *ação* do assessor, em conformidade com CVM e BACEN, sem reter PII nos logs de auditoria.
