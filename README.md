# Chatbot de Qualificação de Leads – Faculdade Zaffalon

#### Desafio Técnico – Vaga Desenvolvedor de Chatbots
---
### Desenvolvido por Gabriel Zaffalon Ferreira Rocha
---

## 🧭 Sumário

* [Plataforma Usada e Justificativa](#plataforma-usada-e-justificativa)
* [Como Testar o Chatbot](#como-testar-o-chatbot)
* [Onde Alterar Configurações (Catálogo e Descontos)](#onde-alterar-configurações-catálogo-e-descontos)
* [Endpoints e Payload Usados](#endpoints-e-payload-usados)
* [Casos de Teste (5 Cenários)](#casos-de-teste-5-cenários)

---

## 🤖 Plataforma Usada e Justificativa

**Plataforma:** [Blip](https://www.blip.ai/) (Take Blip)

**Motivo da escolha:**
O Blip foi escolhido por ser uma plataforma *low-code* robusta com a qual possuo familiaridade. Sua interface no **Builder** suporta nativamente todas as integrações obrigatórias do desafio, como requisições HTTP (`ViaCEP`, `BrasilAPI`) e a implementação de lógica de condições complexas para **validação de dados** (`CPF`, `telefone`) e **cálculo de preços**. A possibilidade de centralizar as configurações em um serviço externo (Google Sheets) demonstra modularidade e facilidade na manutenção.

---

## ▶️ Como Testar o Chatbot

1.  **Acesse o link público do bot:**
    👉 [Teste o Bot Aqui](https://gabriel-zaffalon-4ln9f.chat.blip.ai/?appKey=emFmZmFsb25ncmVldGluZzo4OTFlZjk4Zi1hYzIwLTQ2NzgtYmQ3Yi01OTJjYjk2YzdlMWE=&_gl=1*103qa57*_ga*NDE5MTQ0NTg2LjE3NTk0MzQ4NzE.*_ga_8GVWK8YMGL*czE3NjA1MTc0MjIkbzI1JGcxJHQxNzYwNTE3OTc2JGo1OCRsMCRoMTYzNjA1MzUxNw..*_gcl_aw*R0NMLjE3NjAzNjMwNDQuQ2p3S0NBand4ckxIQmhBMkVpd0F1OUVkTTdCcXpJV21GRVcyVFY5ZTBjYmFLTUhNVnJiUjdNbGpOMWlFTE5BQW9STEdTLVd0X2Y0bXd4b0NSWDRRQXZEX0J3RQ..*_gcl_au*MTU2NTUwNzE2NS4xNzU5NDM0ODcxLjE0OTMyMDgyMzkuMTc2MDM1MzU4Mi4xNzYwMzUzNTgy)

2.  **Siga o fluxo principal:**
    * Informe seus dados pessoais solicitados: **Nome**, **Telefone** (com 10 ou 11 dígitos), **CPF** (com validação de dígito verificador) e **CEP**.
    * Escolha o **tipo de curso** (Graduação ou Pós-Graduação) e a **área de interesse**.

3.  **Funcionalidades Implementadas:**
    * **Validação de Dados:** O *bot* verifica o formato do telefone e o dígito verificador do CPF.
    * **Consulta Endereço:** Utiliza **ViaCEP** para preencher automaticamente a cidade e a UF.
    * **Aviso de Feriado:** Consulta **BrasilAPI – Feriados** e, se houver um feriado dentro de 3, personaliza a mensagem de contato humano para informar sobre horários reduzidos.
    * **Cálculo:** Aplica as regras de desconto (limite máximo de 20%) e exibe a simulação de preço final.
    * **Recomendação:** Mostra **até 3 cursos compatíveis** e oferece **simulação de matrícula** ou contato com um atendente ("Zaffer").

4.  **Comando de Encerramento (Opt-Out):**
    * O comando **“Parar”** está configurado como uma **Exceção de Conteúdo** no Blip, permitindo que o usuário encerre a conversa a qualquer momento do fluxo.

---

## ⚙️ Onde Alterar Configurações (Catálogo e Descontos)

O catálogo de cursos e os parâmetros de desconto e urgência estão centralizados em um **Google Sheets**. O *bot* utiliza uma **requisição HTTP** para buscar esses dados, convertê-los de CSV para JSON (via *script* ou *Parser*) e filtrá-los.

**Catálogo de Cursos (Google Sheets):**
https://docs.google.com/spreadsheets/d/18bbxAr-0kPqzZ24JCfm5QLfk-Ue55_GWurG6RpYlbNA/edit?usp=sharing

| Parâmetro (Colunas do Sheet) | Propósito e Alteração |
| :--- | :--- |
| **`preco_base`** | Altere o preço base de cada curso. |
| **`matricula_deadline`** | Edite esta data (Formato: `YYYY-MM-DD`) para influenciar o cálculo do desconto de **Urgência**. |
| **Parâmetros de Desconto** | As regras de desconto (ex: *Indicação de Amigo: -5%*, *Máximo: 20%*) são aplicadas via **Scripts** no Builder. Para alterá-las, edite o código dentro do bloco de cálculo. |

---

## 🔗 Endpoints e Payload Usados

### 1. ViaCEP – Consulta de Endereço

| Detalhe | Informação |
| :--- | :--- |
| **Endpoint** | `GET https://viacep.com.br/ws/{{cep}}/json/` |
| **Uso** | Preencher automaticamente os campos de cidade e UF (`{{localidade}}` e `{{uf}}` do retorno). |

### 2. BrasilAPI – Feriados Nacionais

| Detalhe | Informação |
| :--- | :--- |
| **Endpoint** | `GET https://brasilapi.com.br/api/feriados/v1/{{ano}}` |
| **Uso** | Verificar se há feriado nos próximos 3 dias da data atual para personalizar a *copy* de transferência humana. |

### 3. Envio de Lead (Simulação de Integração CRM)

| Detalhe | Informação |
| :--- | :--- |
| **Endpoint** | `POST` [webhook.site](https://webhook.site/#!/view/5bf05339-a760-4987-8f32-42cf3526acb7/721368fa-8eda-4d39-a983-8a2a29384fc5/1) |
| **Uso** | Simular a envio dos dados completos do *lead* em um CRM ou *backend*. |

**Payload Enviado (Exemplo com Variáveis Reais):**

```json
{
  "nome": "{{firstNameJs}}",
  "telefone": "{{formatPhoneJs}}",
  "cpf": "{{formatCpfJs}}",
  "cep": "{{formatCepJs}}",
  "cidade": "{{city}}",
  "uf": "{{uf}}",
  "nivel": "{{level}}",
  "area": "{{interestAreaJs}}",
  "curso_escolhido": "{{choosedCourseName}}"
}
```

---

## 🔧 Casos de Teste (5 Cenários)

| Caso de Teste | Interação Chave | Objetivo/Resultado Esperado |
| :--- | :--- | :--- |
| **1. CPF Inválido** | Inserir um CPF com 11 dígitos, mas com dígito verificador incorreto. | O *bot* deve rejeitar o valor e solicitar que o usuário insira um CPF válido novamente. |
| **2. Urgência (Lote ≤7 dias)** | Testar o fluxo afirmando ter urgência | O desconto de **Urgência (−7%)** deve ser aplicado e o preço final exibido. |
| **3. Desconto Máximo** | Ativar as 4 regras de desconto: Amigo (**−5%**), Cartão (**−5%**), Trabalha na Área (**−10%** - Apenas Pós), Urgência (**−7%**). | O *bot* deve calcular o total (27%), mas limitar o desconto final ao teto de **20%.** |
| **4. Sem Cursos Compatíveis** | Escolher **Pós-Graduação** e uma **Área** que não tenha cursos no catálogo (ex: Saúde, se só tiver 1 curso de Pós em TI). | O *bot* deve informar que não encontrou cursos compatíveis e oferecer o contato com um "Zaffer" (atendente). |
| **5. Opt-Out a Qualquer Momento** | Inserir o comando **“parar”** no meio da coleta de dados (ex: após o telefone). | O *bot* deve interromper imediatamente o fluxo principal e direcionar para o bloco de encerramento amigável. |