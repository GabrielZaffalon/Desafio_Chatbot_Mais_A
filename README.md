# Chatbot de Qualificação de Leads – Faculdade Zaffalon

#### Desafio Técnico – Vaga Desenvolvedor de Chatbots
---
### Desenvolvido por Gabriel Zaffalon Ferreira Rocha
---

## 🧭 Sumário

* [Plataforma Usada e Justificativa](#plataforma-usada-e-justificativa)
* [Wireframe e Fluxo de Conversa (Design)](#wireframe-e-fluxo-de-conversa)
* [Como Testar o Chatbot](#como-testar-o-chatbot)
* [Onde Alterar Configurações (Catálogo e Descontos)](#onde-alterar-configurações-catálogo-e-descontos)
* [Endpoints e Payload Usados](#endpoints-e-payload-usados)
* [Casos de Teste (5 Cenários)](#casos-de-teste-5-cenários)
* [Observação sobre o Aviso de Feriado](#observação-sobre-o-aviso-de-feriado)

---

## 🤖 Plataforma Usada e Justificativa

**Plataforma:** [Blip](https://www.blip.ai/) (Take Blip)

**Motivo da escolha:**
O Blip foi escolhido por ser uma plataforma *low-code* robusta com a qual possuo familiaridade. Sua interface no **Builder** suporta nativamente todas as integrações obrigatórias do desafio, como requisições HTTP (`ViaCEP`, `BrasilAPI`) e a implementação de lógica de condições complexas para **validação de dados** (`CPF`, `telefone`) e **cálculo de preços**. A possibilidade de centralizar as configurações em um serviço externo (Google Sheets) demonstra modularidade e facilidade na manutenção.

---

## 🎨 Wireframe e Fluxo de Conversa (Design)

Para garantir uma experiência de usuário (UX) fluida e um desenvolvimento eficiente, todo o fluxo de conversação do chatbot foi primeiramente desenhado e prototipado no **Figma**.

### Benefícios do uso do Figma:

* **Mapeamento Visual:** Foi criado um *wireframe* completo que mapeia visualmente cada passo, condições lógicas e transição de blocos, espelhando a estrutura final implementada no Blip Builder.
* **Planejamento de Copy:** O Figma serviu como um **Storytelling Canvas**, permitindo o planejamento e a revisão da *copy* em um ambiente visual, otimizando a clareza, empatia e persuasão das mensagens.
* **Estrutura de Dados:** O fluxo detalha os pontos de coleta de dados do *lead* (Nome, CPF, etc.) e os momentos exatos das integrações com APIs e do cálculo de descontos, garantindo que o *payload* final seja montado com todas as variáveis necessárias.
* **Validação de Regras:** O design visual facilitou a validação das regras de negócio, como o limite máximo de desconto de 20% e as condições para o desconto de urgência.

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
| **`id`** | identificador do curso. Dinido por *g* (graduação) ou p (pós) - area - nome ou sigla. |
| **`nivel`** | graduacao ou pos |
| **`area`** | Área de especialização do curso |
| **`nome`** | Nome do curso. |
| **`preco_base`** | Preço base de cada curso. |
| **`duracao`** | Tempo de duração. |
| **`modalidade`** | EAD ou Presencial. |
| **`principal_area`** | Principal área de atuação. |
| **`cargos_possiveis`** | Possíveis cargos de atuação. |

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
  "curso_escolhido": "{{choosedCourseName}}",
  "data_envio": "{{timesTempJs}}"
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

---

## 📢 Observação sobre o Aviso de Feriado

[cite_start]Para demonstrar a funcionalidade de integração com a **BrasilAPI – Feriados**[cite: 46], foi inserida uma data futura próxima à do Feriado da Proclamação da República (15 de Novembro).

* [cite_start]**Comportamento Atual:** Se você avançar para o atendimento humano, notará o aviso de que o **horário de atendimento está reduzido**[cite: 47]. [cite_start]Este comportamento foi forçado para exemplificar o funcionamento do *bot* caso um feriado estivesse realmente próximo ($\leq 3$ dias)[cite: 46].

* **Como Desativar o Aviso (Instrução Técnica):**

    O aviso de feriado é controlado por um *Script* chamado **`nextHolidayJs`** que se encontra no bloco **`apiGetNationalHolidays`** no Blip Builder.

    Para que o *bot* comece a usar a data atual e desativar o aviso (a menos que haja um feriado real próximo), altere o código nas linhas 13 e 21 do *script*, substituindo:

    ```javascript
    const d = new Date("2025-11-15"); 
    ```
    pela variável de data atual:
    ```javascript
    const d = new Date();
    ```