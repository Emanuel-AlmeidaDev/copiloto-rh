# Copiloto de Agendamentos Marvão

Sistema com IA Generativa que escreve avisos de agendamento para os 700 motoristas da **Marvão Serviços** (transporte escolar), prontos para enviar no WhatsApp.

Trabalho da disciplina **Fundamentos da IA Generativa** – "Seu Primeiro Copiloto de IA".

- **Sistema no ar:** https://emanuel-almeidadev.github.io/copiloto-rh/
- **Repositório:** https://github.com/Emanuel-AlmeidaDev/copiloto-rh

## O problema

Os avisos para os motoristas (reuniões, abastecimento, mudança de escala, uniforme, ticket, cartão corporativo, feriados) são escritos cada um de um jeito. Muitos chegam sem horário, local ou o que levar, o que gera dúvidas repetidas e retrabalho para o RH.

## A solução

Um formulário web em que o RH escolhe o tipo de agendamento, a data, o local e o público. Um cenário no **Make.com** envia o pedido para o **Google Gemini** com um prompt de sistema elaborado, e a mensagem pronta volta para a própria página:

1. mensagem completa para o WhatsApp;
2. versão curta para lembrete na véspera.

## Como funciona

```
Formulário (GitHub Pages)
   │  pedido estruturado
   ▼
Make.com – Custom webhook
   │
   ▼
Google Gemini – prompt de sistema (prompt.txt)
   │
   ▼
Make.com – Webhook response
   │  mensagem pronta
   ▼
Formulário mostra a mensagem → RH revisa → envia no WhatsApp
```

- O formulário obriga tipo, data, horário, local e público, para a IA não precisar adivinhar nada.
- O dia da semana é calculado pela página, e não pela IA (modelos de linguagem costumam errar dias da semana).
- A chave da API fica guardada no Make, e não no código público da página.

## Como usar

1. Abra o sistema: **https://emanuel-almeidadev.github.io/copiloto-rh/**
2. Preencha o formulário e clique em **Gerar mensagem**.
3. Revise a mensagem.
4. Clique em **Copiar mensagem** e envie no grupo ou no privado do motorista.

## Arquivos

| Arquivo | O que é |
| --- | --- |
| `index.html` | O formulário (interface) |
| `prompt.txt` | Prompt de sistema usado no módulo do Gemini, no Make |
| `README.md` | Esta documentação |

## Como recriar

1. **Make:** crie um cenário com três módulos: `Webhooks → Custom webhook`, `Google Gemini AI` (módulo que gera uma resposta de texto) (com o conteúdo de `prompt.txt` como instrução de sistema e `{{pedido}}` como mensagem) e `Webhooks → Webhook response` (Body = texto gerado pelo Gemini; header `Access-Control-Allow-Origin: *`). Ative o cenário.
2. **Página:** em `index.html`, cole o endereço do webhook em `const WEBHOOK_URL = '...'`.
3. **GitHub Pages:** envie os arquivos para um repositório público e ative em Settings → Pages.

Sem o webhook configurado, a página funciona em **modo reserva**: abre o ChatGPT com o pedido já preenchido.

## Cuidados (ética e LGPD)

- Nunca digite nome, endereço ou escola de alunos, nem dados pessoais de motoristas.
- O texto é processado por um serviço de IA externo (Google).
- A IA sugere, mas quem envia é uma pessoa: sempre revise antes de enviar.
- O endereço do webhook é público na página. Em uso real, ele deveria ter proteção contra abuso (por exemplo, uma senha ou login).

## Ferramentas

- HTML, CSS e JavaScript (interface criada com apoio de IA)
- GitHub Pages (hospedagem)
- Make.com (automação)
- Google Gemini (modelo de linguagem)
"# copiloto-rh" 
