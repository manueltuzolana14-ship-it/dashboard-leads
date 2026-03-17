# Dashboard TV Cabo com Google Sheets

Sistema completo para pesquisa e atualização de pagamentos de clientes, com frontend em HTML/CSS/JavaScript e backend em Google Apps Script.

## Estrutura

- `index.html`: estrutura principal do dashboard.
- `styles.css`: visual responsivo inspirado no dashboard de referência.
- `app.js`: pesquisa dinâmica, filtros, gráficos, seleção de cliente e envio para o Web App.
- `backend/Code.gs`: API do Google Apps Script, criação das abas mensais, histórico e formatação da planilha.
- `backend/appsscript.json`: manifesto do projeto Apps Script.

## Onde configurar

### 1. ID da planilha Google Sheets

No ficheiro `backend/Code.gs`, altere:

```javascript
SPREADSHEET_ID: "COLE_AQUI_O_ID_DA_PLANILHA",
```

### 2. Nome da aba principal

No mesmo ficheiro, altere se quiser:

```javascript
MAIN_SHEET_NAME: "Clientes",
```

### 3. URL do Web App

No ficheiro `app.js`, altere:

```javascript
webAppUrl: "COLE_AQUI_A_URL_DO_WEB_APP",
```

## Estrutura esperada da aba principal

A primeira linha deve conter pelo menos estas colunas:

```text
Nomes | JAN | FEV | MAR | ABR | MAI | JUN | JUL | AGO | SET | OUT | NOV | DEZ | Localização | Nº de Telefone | Nº
```

Se a aba principal não existir, o script cria esta estrutura automaticamente.

## O que o Apps Script faz

- lê a aba principal;
- devolve os clientes em JSON;
- atualiza o mês escolhido com `Pago`, `Corte` ou `Pendente`;
- colore automaticamente as células:
  - `Pago` em verde;
  - `Corte` em vermelho;
  - `Pendente` em amarelo;
- cria as abas mensais `Janeiro` a `Dezembro` se não existirem;
- grava ou atualiza o histórico mensal sem duplicar o cliente;
- mantém a planilha formatada com cabeçalhos e colunas ajustadas.

## Passo a passo de configuração

1. Crie uma planilha Google Sheets.
2. Crie ou confirme a aba principal com o nome definido em `MAIN_SHEET_NAME`.
3. Abra [script.google.com](https://script.google.com) e crie um projeto Apps Script.
4. Copie o conteúdo de `backend/Code.gs` para o ficheiro `Code.gs` do Apps Script.
5. Copie o conteúdo de `backend/appsscript.json` para o manifesto do Apps Script.
6. Troque o `SPREADSHEET_ID` pelo ID real da planilha.
7. No editor do Apps Script, execute a função `setupSpreadsheet()` uma vez para criar/formatar abas e autorizar o script.
8. Publique o projeto como Web App:
   - `Deploy` > `New deployment`
   - Tipo: `Web app`
   - `Execute as`: `Me`
   - `Who has access`: `Anyone`
9. Copie a URL `/exec` do Web App.
10. No ficheiro `app.js`, cole essa URL em `webAppUrl`.
11. Publique `index.html`, `styles.css` e `app.js` num alojamento estático.

## Como publicar o frontend

Pode publicar estes 3 ficheiros em qualquer opção simples:

- GitHub Pages
- Netlify
- Vercel
- servidor local/interno da empresa

Basta manter os três ficheiros juntos.

## Fluxo de uso

1. O funcionário abre o link do dashboard.
2. Pesquisa por nome ou localização.
3. Seleciona o cliente.
4. Escolhe o mês e o status.
5. Clica em `Salvar atualização`.
6. O frontend envia a alteração ao Apps Script.
7. O Apps Script atualiza a aba principal e a aba mensal correspondente.
8. O dashboard recarrega os dados automaticamente.

## Observações importantes

- O sistema não depende de upload manual de Excel.
- A ligação fica fixa à mesma planilha até mudar o `SPREADSHEET_ID`.
- O Google Sheets fica privado; os utilizadores só veem o dashboard.
- O dashboard foi preparado para desktop e telemóvel.
- Para evitar problemas de permissões, a primeira execução do `setupSpreadsheet()` deve ser feita pelo dono da planilha.
