# Checklist de Integração - Colégio Carbonell

## Descrição do Projeto

Este projeto é um formulário web desenvolvido para facilitar o processo de integração de novos colaboradores no Colégio Carbonell. O formulário é um checklist que registra informações essenciais sobre o colaborador e verifica a configuração de acessos digitais, conexões de rede, plataformas internas e outros recursos necessários para o início de suas atividades. As informações preenchidas no formulário são enviadas e armazenadas em uma planilha do Google Sheets por meio de uma API Google Apps Script.

O projeto é hospedado no **GitHub Pages** e está acessível em: [https://carbonellgit.github.io/checklist-carbonell/](https://carbonellgit.github.io/checklist-carbonell/).

## Funcionalidades

- **Dados do Colaborador**: Coleta o nome e o setor do colaborador (TI, RH, Financeiro, Pedagógico, Administrativo, Infraestrutura, Marketing, Comercial ou Outros).
- **Checklist de Acessos e Configurações**:
  - Configuração de e-mail institucional e verificação em duas etapas (2FA).
  - Conexão à rede Wi-Fi institucional.
  - Acesso ao Slack e outras plataformas internas (SAS, Layers, Sophia, SAA, LMS, etc.).
  - Configuração de Chromebooks e acesso ao Portal do Colaborador.
- **Envio de Dados**: Os dados preenchidos são enviados para uma planilha do Google Sheets via uma requisição POST para um script do Google Apps Script.
- **Interface Intuitiva**: Design limpo e responsivo, com o logotipo do Colégio Carbonell e cores institucionais (vermelho e branco).
- **Feedback ao Usuário**: Exibe alertas de sucesso ou erro após o envio do formulário.

## Tecnologias Utilizadas

- **HTML5**: Estrutura do formulário.
- **CSS3**: Estilização personalizada com design responsivo e cores institucionais.
- **JavaScript**: Lógica para coletar dados do formulário e enviar para o Google Sheets.
- **Google Apps Script**: Backend para receber os dados e salvá-los em uma planilha do Google Sheets.
- **GitHub Pages**: Hospedagem do formulário.

## Como Usar

1. **Acesse o Formulário**:
   - Visite [https://carbonellgit.github.io/checklist-carbonell/](https://carbonellgit.github.io/checklist-carbonell/).
2. **Preencha o Checklist**:
   - A assistente de TI e o novo colaborador devem preencher o formulário juntos.
   - Insira o nome do colaborador e selecione o setor.
   - Marque as caixas de seleção conforme as configurações realizadas.
3. **Envie o Formulário**:
   - Clique no botão "Enviar Checklist" para salvar os dados na planilha do Google Sheets.
   - Um alerta confirmará o sucesso ou indicará erro no envio.
4. **Verifique os Dados**:
   - Os dados enviados são registrados em uma planilha do Google Sheets configurada pelo time de TI.

## Estrutura do Repositório

```
checklist-carbonell/
├── index.html          # Arquivo principal do formulário
├── ColegioCarbonell.svg # Logotipo do Colégio Carbonell
└── README.md           # Documentação do projeto
```

## Configuração do Google Apps Script

Para que o formulário funcione corretamente, é necessário configurar um script no Google Apps Script para processar os dados enviados. Siga os passos abaixo:

1. **Crie uma Planilha no Google Sheets**:
   - Crie uma nova planilha para armazenar os dados.
   - Configure o cabeçalho da planilha com as seguintes colunas, na ordem exata:
     ```
     Timestamp, Nome do Colaborador, Setor, E-mail, 2FA, Celular, Chromebook, Wi-Fi, Slack, Slack Celular, SAS, Layers, Sophia, SAA, Rede, LMS, Portal, Agendas, Canais, Login Chrome, 2FA Chrome, Sistemas Chrome
     ```

2. **Configure o Google Apps Script**:
   - Acesse [Google Apps Script](https://script.google.com/).
   - Crie um novo projeto.
   - Substitua o código padrão pelo seguinte:
     ```javascript
     function doPost(e) {
       var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
       var data = JSON.parse(e.postData.contents);
       
       // Ordem das colunas deve corresponder ao cabeçalho da planilha
       var row = [
         data.timestamp,
         data.nomeColaborador,
         data.set##, data.email, data['2fa'],
         data.celular,
         data.chromebook,
         data.wifi,
         data.slack,
         data.slackCelular,
         data.dicasseguranca,
         data.sas,
         data.layers,
         data.sophia,
         data.saa,
         data.rede,
         data.lms,
         data.portal,
         data.agendas,
         data.canais,
         data.loginChrome,
         data['2faChrome'],
         data.sistemasChrome
       ];
       
       sheet.appendRow(row);
       return ContentService.createTextOutput("Sucesso").setMimeType(ContentService.MimeType.TEXT);
     }
     ```
3. **Publique o Script**:
   - Clique em **Implantar** > **Nova implantação**.
   - Selecione **Aplicativo da Web**, configure como **Qualquer pessoa** (ou restrinja conforme necessário) e implante.
   - Copie a URL fornecida (ex.: `https://script.google.com/macros/s/SUA_URL/exec`).
   - Atualize a variável `fetch` no arquivo `index.html` (no trecho do JavaScript) with this URL:
     ```javascript
     fetch('https://script.google.com/macros/s/SUA_URL/exec', {
     ```

4. **Teste o Formulário**:
   - Preencha o formulário e envie para verificar se os dados aparecem corretamente na planilha.
   - Confirme que as colunas da planilha correspondem à ordem dos dados no script `doPost`.

## Implantação no GitHub Pages

1. **Suba os Arquivos para o Repositório**:
   - Certifique-se de que `index.html` e `ColegioCarbonell.svg` estão no repositório.
2. **Configure o GitHub Pages**:
   - No GitHub, vá para as configurações do repositório.
   - Em **Pages**, selecione a branch `main` (ou a branch desejada) e a pasta raiz (`/root`).
   - Salve e aguarde a publicação. O formulário está disponível em: [https://carbonellgit.github.io/checklist-carbonell/](https://carbonellgit.github.io/checklist-carbonell/).
3. **Acesse o Formulário**:
   - Use o link acima para acessar o formulário online.

## Contribuições

Este projeto é destinado ao uso interno do Colégio Carbonell, mas contribuições são bem-vindas. Para sugerir melhorias ou reportar problemas:

1. Abra uma **issue** no repositório.
2. Descreva o problema ou a sugestão com detalhes.
3. Para contribuições diretas, crie um **pull request** com as alterações propostas.

## Licença

Este projeto está licenciado sob a [MIT License](LICENSE). Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## Contato

Para dúvidas ou suporte técnico, entre em contato com o time de TI do Colégio Carbonell.

---

**Desenvolvido por Thiago Marques Luiz**  
**Colégio Carbonell - 2025**
