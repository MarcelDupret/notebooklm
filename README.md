# 🛰️ Desafio de Projeto: Explorando o Poder do NotebookLM
### Bootcamp Santander & DIO — Automação com N8N e IA

---

## 📌 1. Tema e Objetivo

> **Tema:** Rastreamento veicular autônomo com o dispositivo **J16 4G** e a plataforma open-source **Traccar**.  
> **Objetivo:** *"Criar um ecossistema completo de telemetria e rastreamento veicular livre de mensalidades, compreendendo a arquitetura de nuvem (VPS/Docker), a parametrização de rede e protocolos (GT06), a instalação elétrica do hardware (fiação e relé de corte) e a gestão segura da plataforma Traccar."*

Este projeto explora as capacidades analíticas e de síntese do **NotebookLM** como um consultor especialista em IoT e telemetria, sintetizando documentações técnicas, diagramas elétricos e parâmetros de rede em um guia prático para implementação de ponta a ponta.

---

## 📚 2. Fontes Utilizadas e Critérios de Confiabilidade

Para alimentar o NotebookLM com dados técnicos sólidos, foram selecionadas fontes complementares e validadas:

| Fonte | Tipo | Motivo da Confiabilidade |
| :--- | :--- | :--- |
| **Documentação Oficial do Traccar** | Docs / Guias Técnicos | Fonte primária oficial sobre portas de escuta (`listener`), protocolos suportados (GT06 na porta 5023), banco de dados PostgreSQL e APIs de push notification. |
| **Manuais Técnicos do Rastreador J16 4G** | Especificações de Hardware | Tabela oficial de comandos SMS, pinagem do chicote de 4 fios, consumo elétrico (9V–90V DC) e tabela de comandos de protocolo estendido (`SZCS`). |
| **Vídeos Técnicos Especializados (YouTube)** | Tutoriais Práticos Validados | Demonstrações práticas em bancada e testes em veículos reais com alto volume de visualizações e feedback positivo da comunidade de rastreamento. |
| **Deep Research (NotebookLM)** | Síntese e Descoberta de Fontes | Pesquisas aprofundadas conduzidas pelo mecanismo integrado do NotebookLM, cruzando referências sobre resolução de bugs conhecidos (ex: sincronização de status ACC/ignição). |

**Por que confiar nessas fontes:**  
Selecionei manuais dos fabricantes de hardware (chipset SIMCom / protocolo GT06) e a documentação do software do servidor (Traccar), além de incluir vídeos do YouTube com muitas visualizações e boas avaliações. Aceitei algumas sugestões da opção de Deep Research do NotebookLM. Um total de 31 fontes foram incorporadas.

---

## 🧭 3. Diretriz de Comportamento (Persona do NotebookLM)

Para orientar a geração de respostas precisas, didáticas e aplicadas à bancada e à nuvem, a seguinte diretriz de sistema foi configurada no chat do notebook:

```text
Atue como um consultor especialista para solucionar dúvidas sobre o rastreador veicular J16 e a plataforma de rastreamento Traccar. Priorize respostas claras e concisas, se possível em tópicos para serem seguidos passo a passo.
```

Essa diretriz garantiu que o NotebookLM:
- Estruturasse instruções como procedimentos operacionais padrão (SOP / passo a passo);
- Evitasse jargões sem aplicação prática imediata;
- Destacasse alertas de segurança elétrica (ex: risco de queima de SIM card ou curto em relés).

---

## ❓ 4. Perguntas Feitas, Respostas do NotebookLM e Fontes Citadas

Abaixo constam dois exemplos reais de consultas ao NotebookLM, demonstrando o nível de grounding (ancoragem nas fontes fornecidas):

### 💬 Interação 1: Pós-Instalação e Segurança do Servidor Traccar
* **Pergunta do Usuário:**  
  *Quais são as configurações essenciais no servidor, na rede e nos dispositivos após realizar a instalação básica do Traccar via Docker numa VPS?*
* **Fontes Embasadas:**  
  *Documentação Oficial Traccar (Configuração de Portas e Segurança) + Manuais de Protocolo GT06/J16.*
* **Resposta Síntese do NotebookLM:**
  1. **Segurança Inicial:** Alteração imediata das credenciais padrão (`admin`/`admin`) e desativação do cadastro público de novos usuários (`registration = false`).
  2. **Firewall e Portas:** Abertura da porta `8082 (TCP)` para a interface web e abertura da porta `5023 (TCP/UDP)` dedicada ao protocolo **GT06** do J16.
  3. **Apontamento do Dispositivo:** Cadastro do IMEI no Traccar e envio do comando SMS `SERVER,0,IP_DA_VPS,5023,0#` com fuso ajustado em UTC 0 (`GMT,E,0,0#`).
  4. **Notificações:** Criação de alertas de ignição ligada/desligada e corte de alimentação, vinculando o alerta explicitamente ao veículo no painel.
  5. **Correção de Ignição:** Ativação do *Attribute Copy* e envio do comando `SZCS#GT06SEL=1#` para transmissão contínua do estado da chave.

---

### 💬 Interação 2: Instalação Elétrica e Bloqueio com Relé
* **Pergunta do Usuário:**  
  *Como deve ser feita a instalação elétrica do chicote do rastreador J16 e a ligação dos pinos do relé de corte de combustível?*
* **Fontes Embasadas:**  
  *Manual de Hardware do Rastreador J16 + Diagramas Elétricos Automotivos e Vídeos de Bancada.*
* **Resposta Síntese do NotebookLM:**
  1. **Fiação do J16:**
     - **Vermelho (V+)**: Positivo contínuo 12V/24V (direto na bateria).
     - **Preto (GND)**: Aterramento no chassi do veículo.
     - **Laranja/Branco (ACC)**: Pós-chave (sinal 12V ativo apenas com ignição ligada).
     - **Amarelo (Corte)**: Saída negativa para controle da bobina do relé.
  2. **Pinagem do Relé (Corte Normalmente Fechado - 30 e 87a):**
     - **Pino 85**: Conectado ao fio Amarelo do rastreador (sinal de desarme).
     - **Pino 86**: Ligado a 12V pós-chave (alimentação da bobina).
     - **Pinos 30 e 87a**: Intercalados no fio da bomba de combustível (30 vindo da caixa de fusíveis / 87a indo para a bomba).
  3. **Comandos de Teste Operacional:**  
     - Bloqueio: `RELAY,1#`  
     - Desbloqueio: `RELAY,0#`  
     - Anti-sabotagem: `SZCS#SOURCE_OFF_TYPE=1#`

---

## 🎨 5. Recursos e Artefatos Gerados

Além das respostas no chat, o NotebookLM foi utilizado para sintetizar o conhecimento em dois materiais visuais de referência anexados a este repositório:

1. **[Mapa Mental da Arquitetura (Mind Map Rastreador.png)](file:///Users/marcel/projetos/n8n/Bootcamp%20DIO/Mind%20Map%20Rastreador.png)**:  
   Visão hierárquica cobrindo:
   - Infraestrutura VPS (AWS Lightsail, Firewall, portas 8082 e 5023);
   - Banco de dados PostgreSQL;
   - Parametrização do hardware via SMS;
   - Camadas de segurança e anti-fraude.

2. **[Apresentação Completa em Slides (Rastreador J16 e Traccar.pdf)](file:///Users/marcel/projetos/n8n/Bootcamp%20DIO/Rastreador%20J16%20e%20Traccar.pdf)**:  
   Guia visual de 15 lâminas gerado a partir do caderno, detalhando arquitetura de telecomunicação, diagramas elétricos com código de cores e árvores de decisão para troubleshooting.

---

## 🔗 6. Link do NotebookLM Compartilhado

O caderno de estudo completo com todas as fontes indexadas, notas e sessões de chat pode ser acessado no link:

👉 **[Acessar NotebookLM: Rastreamento J16 & Traccar](https://notebook.google.com/notebook/c88a955c-2f10-46bf-a091-ac62039d5205)**



