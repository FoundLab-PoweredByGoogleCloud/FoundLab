<p align="center">
  <img src="https://i.ibb.co/ccj8Mdky/LOGO-OK1.png" alt="FoundLab" width="180" />
</p>

<h1 align="center">FoundLab — Documentos Institucionais</h1>

<p align="center">
  Repositório público com os materiais de referência da FoundLab para bancos, PSPs e parceiros tecnológicos.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Google_for_Startups_Cloud_Program-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white" alt="Google for Startups Cloud Program" />
  <img src="https://img.shields.io/badge/Google_Cloud-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white" alt="Google Cloud" />
  <img src="https://img.shields.io/badge/ATI-Auditable_Trust_Infrastructure-111827?style=for-the-badge" alt="ATI" />
  <img src="https://img.shields.io/badge/Zero_Persistência-0F172A?style=for-the-badge" alt="Zero-Persistência" />
</p>

---

## 1. Objetivo

Este repositório concentra os **documentos oficiais** usados pela FoundLab para explicar:

- a tese de **Infraestrutura de Confiança Auditável (ATI)**;
- a plataforma **Umbrella** (processamento de documentos com auditoria);
- o protocolo **Veritas 2.0** (DecisionID + trilha WORM);
- e o **enquadramento institucional** feito para o BTG Pactual.

É um repositório de **conteúdo institucional**, não de código-fonte.

---

## 2. Arquivos disponíveis

Os arquivos atualmente publicados neste repositório são:

1. **BTG.md**  
   Enquadramento direto da FoundLab no contexto BTG.

2. **Deck Bancário para IA AuditávelV1.md**  
   Estrutura de apresentação para instituições financeiras sobre IA auditável.

3. **Deck SUPORTE Bancário para IA Auditável.md**  
   Material complementar ao deck principal, com detalhamento.

4. **Memorando Institucional_ FoundLab + BTG Pactual.md**  
   Memorando que registra o racional institucional da proposta.

5. **ModeloDedicated.md**  
   Modelo de oferta em ambiente dedicado / dentro do perímetro do cliente.

6. **Pitch FoundLab para BTG_ IA Auditávelv1.md**  
   Versão curta do pitch para envio pré-reunião.

7. **Simulação ROI FoundLab para BTG Pactual.md**  
   Documento de simulação de ganhos e redução de risco.

8. **Validação de Simulações ROI FoundLab BTG.md**  
   Complemento com premissas e validações.

9. **Uma Proposta de Infraestrutura de Confiança Auditável para o BTG Pactual.md**  
   Texto completo que amarra tese, arquitetura e caso de uso.

10. **Veritas_2.0_Whitepaper (1).md**  
    Whitepaper da camada de auditabilidade e evidência (Veritas 2.0).

> Caso novos documentos sejam adicionados, manter a mesma nomenclatura descritiva.

---

## 3. Ordem sugerida de leitura

1. **Comece pelo contexto**  
   - `Uma Proposta de Infraestrutura de Confiança Auditável para o BTG Pactual.md`

2. **Veja a versão executiva (visual)**  
   - `Deck Bancário para IA AuditávelV1.md`
   - `Deck SUPORTE Bancário para IA Auditável.md`

3. **Entenda o enquadramento institucional**  
   - `Memorando Institucional_ FoundLab + BTG Pactual.md`

4. **Consulte o modelo de entrega**  
   - `ModeloDedicated.md`

5. **Feche com os números**  
   - `Simulação ROI FoundLab para BTG Pactual.md`
   - `Validação de Simulações ROI FoundLab BTG.md`

6. **Aprofunde na auditabilidade**  
   - `Veritas_2.0_Whitepaper (1).md`

Assim qualquer stakeholder que receber o link consegue reconstruir a tese completa.

---

## 4. Escopo técnico (referência)

Todos os documentos aqui seguem a mesma linha técnica da FoundLab:

- processamento de documentos com **zero-persistência** de conteúdo sensível;
- geração de **DecisionID** para cada análise;
- trilha de auditoria em modelo **append-only / WORM**;
- implantação típica em **Google Cloud** (Cloud Run, Document AI, Vertex AI, BigQuery);
- aderência a **LGPD** e requisitos do setor financeiro.

O repositório existe justamente para deixar isso público e verificável.

---

## 5. Boas práticas de atualização

- Manter **tom formal** e alinhado ao setor financeiro.
- Priorizar **Markdown (.md)**.
- Não incluir **PII ou dados de cliente**.
- Quando houver revisão de um arquivo enviado a terceiros, usar sufixo de versão (`...v2.md`, `...2025-11.md`).

---

<p align="center">
  <sub>FoundLab · Infraestrutura de confiança auditável para o setor financeiro.</sub>
</p>
