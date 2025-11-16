README - Serviço+ Web V2
========================

Conteúdo:
- index.html : Página web pronta para GitHub Pages
- logo.png (se fornecido)

O que esta versão faz:
- Busca dados da tua Google Sheet via Apps Script (API GET).
- Mostra resultados com busca e filtros simples.
- Permite ao utilizador 'Adicionar Serviço' através de um formulário.
- Tenta enviar a submissão via POST para a mesma URL da tua API.

ATENÇÃO — Apps Script:
Para que o envio funcione (POST), o teu Apps Script precisa de implementar `doPost(e)` para receber e gravar linhas na folha.
Segue o exemplo abaixo — cola no teu Apps Script (Editor) e publica:

----- INICIO doSnippet Apps Script -----
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.openById('SUA_SPREADSHEET_ID').getSheetByName('Servicos');
    var body = e.postData ? e.postData.contents : null;
    if (body) {
      var data = JSON.parse(body);
      sheet.appendRow([
        new Date(),
        data.Nome || '',
        data.Categoria || '',
        data.Telefone || '',
        data.WhatsApp || '',
        data.Bairro || '',
        data.Horario || '',
        data.Descrição || ''
      ]);
    }
    return ContentService.createTextOutput(JSON.stringify({status:'ok'})).setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService.createTextOutput(JSON.stringify({status:'error','message':err.message})).setMimeType(ContentService.MimeType.JSON);
  }
}
----- FIM doSnippet -----

Substitua 'SUA_SPREADSHEET_ID' pelo ID do teu Google Sheet (o que aparece na URL).

Se preferires não usar POST, a página também inclui fallback: ao falhar, abre o WhatsApp com mensagem pré-preenchida (edita o número no index.html).

Publicar no GitHub Pages:
1. Cria um repositório (ou usa o existente).
2. Upload dos ficheiros (index.html e logo.png).
3. Settings -> Pages -> Branch: main -> Folder: root -> Save.
4. Espera 20–40s e visita a URL.

Precisas que eu:
- Atualize o Apps Script para incluír doPost? (posso gerar o script completo)
- Substitua o número de WhatsApp de fallback no index.html pelo teu número real?
- Ajeite a folha (colunas) para o formato que preferes?

Responde dizendo o que queres que eu faça a seguir.
