# scanner-url-browser

Web Scanner perfeito para navegadores! 🚀

📢 Apresentação
Venho apresentar um Web Scanner perfeito para navegadores! 🚀

O scanner faz uma busca completa na aplicação web e traz os resultados instantaneamente, facilitando sua vida de bug hunter e pentester, ajudando a encontrar URLs, diretórios e arquivos ocultos.

⚠️ Nota: Este script não faz uma busca super profunda, mas é um ótimo complemento para outras ferramentas como Waybackurls, GAU, GAUplus.

🔍 Código do Scanner
Copie:

javascript:void(function(){
    let e = document.createElement('div');
    e.style.cssText = 'position:fixed;bottom:0;left:0;width:100%;height:350px;background:#2d2d2d;color:white;z-index:999999;padding:20px;overflow:auto;font-family:Arial, sans-serif;border-top:4px solid #0055ff;border-radius:10px 10px 0 0;';
    e.innerHTML = '<h3 style="color:#00ff00;text-align:center;font-size:20px;margin-bottom:15px;">🔍 <strong>BlackLine Scanner</strong></h3>' +
                  '<div style="margin-bottom:15px;color:#00ff00;font-size:14px;text-align:center;">' +
                  'Palavras-chave para pesquisa: <span style="color:#ffcc00;font-weight:bold;">api, key, token, auth, config, database, backup, admin, login, user, secret, private, .json, .env, debug, test</span>' +
                  '</div>' +
                  '<input type="text" id="searchBox" placeholder="Pesquisar URLs..." style="width:80%;padding:10px;border-radius:5px;border:none;font-size:14px;color:#333;background-color:#fff;margin-bottom:15px;box-shadow:0 0 10px rgba(0,0,0,0.1);">' +
                  '<button id="searchButton" style="width:18%;padding:10px;margin-left:2%;border-radius:5px;border:none;background:#0055ff;color:white;font-size:14px;cursor:pointer;">Pesquisar</button>' +
                  '<div id="evidenceCount" style="margin:10px 0;color:#00ff00;text-align:center;font-size:16px;"></div>' +
                  '<div id="results" style="color:white;font-size:14px;overflow:auto;">Scanning...</div>';
    document.body.appendChild(e);
    
    let currentDomain = window.location.hostname;
    let foundUrls = new Set();
    
    function findUrls() {
        let urls = [];
        let sources = [...document.getElementsByTagName('a'), ...document.getElementsByTagName('script'),
                       ...document.getElementsByTagName('img'), ...document.getElementsByTagName('link'), 
                       ...document.getElementsByTagName('form')];
        
        sources.forEach(element => {
            if (element.href) urls.push(element.href);
            if (element.src) urls.push(element.src);
            if (element.action) urls.push(element.action);
        });

        let content = document.documentElement.innerHTML;
        let urlPattern = /(?:url\(|href="|src="|action="|url:|endpoint:|path:|route:)\s*['"]?([^'")\s>]+)/gi;
        let match;
        
        while ((match = urlPattern.exec(content)) !== null) {
            if (match[1] && !match[1].startsWith('data:')) urls.push(match[1]);
        }

        let scriptPattern = /"[^"]*"|'[^']*'/g;
        let scripts = document.documentElement.innerHTML.match(scriptPattern) || [];
        
        scripts.forEach(script => {
            let urlMatches = script.match(/(?:\/[a-zA-Z0-9_-]+)+(?:\.[a-zA-Z0-9]+)?/g) || [];
            urlMatches.forEach(url => urls.push(url));
        });

        performance.getEntriesByType('resource').forEach(entry => urls.push(entry.name));
        
        return [...new Set(urls)];
    }
    
    let allUrls = findUrls();
    allUrls.sort();
    
    let resultsContainer = document.getElementById('results');
    resultsContainer.innerHTML = 
        `<div style="margin:15px 0;color:#00ff00;text-align:center;font-size:18px;">✅ <strong>Encontrado ${allUrls.length} URLs & Endpoints</strong> em ${currentDomain}</div>
        <div id="urlList" style="background:#1e1e1e;padding:15px;border-radius:10px;max-height:200px;overflow:auto;box-shadow:0 0 15px rgba(0,0,0,0.3);">
        ${allUrls.map((url, index) => {
            return `<div class="urlItem" data-index="${index}" style="color:white;margin:10px 0;padding:10px;background:#333;border-radius:5px;word-break:break-all;box-shadow:0 0 5px rgba(0,0,0,0.2);">
                    <a href="${url}" target="_blank" style="color:#00ff00;text-decoration:none;" onclick="event.stopPropagation();">${url}</a>
                    </div>`;
        }).join('')}
        </div>`;
    
    let searchBox = document.getElementById('searchBox');
    let searchButton = document.getElementById('searchButton');
    let evidenceCount = document.getElementById('evidenceCount');
    let items = document.querySelectorAll('.urlItem');
    let matches = [];
    let matchIndex = -1;
    
    function searchAndHighlight() {
        let query = searchBox.value.toLowerCase();
        matches = [];
        matchIndex = -1;
        
        items.forEach((item, index) => {
            if (item.innerText.toLowerCase().includes(query)) {
                matches.push(item);
                item.style.background = '#0055ff';
            } else {
                item.style.background = '#333';
            }
        });
        
        evidenceCount.innerText = `🔍 ${matches.length} evidências encontradas`;
    }
    
    function jumpToNextMatch() {
        if (matches.length === 0) return;
        matchIndex = (matchIndex + 1) % matches.length;
        matches[matchIndex].scrollIntoView({ behavior: 'smooth', block: 'center' });
    }
    
    searchButton.addEventListener('click', searchAndHighlight);
    searchBox.addEventListener('keydown', function(event) {
        if (event.key === 'Enter') {
            jumpToNextMatch();
        }
    });
})();


interface

![image](https://github.com/user-attachments/assets/83c06db6-786c-4365-8b6f-2a2a95c10e68)

COMO USAR

ATENÇAO VOCE TEM QUE SALVA O CODIGO COMO FAVORITO NO NAVEGADOR NESSE METODO EXPLICADO ABAIXO FOI FEITO NO NAVEGADOR FIREFOX POIS CADA NAVEGADOR TEM UM JEITO DIFERENTE DE FAZER ISSO ENTAO SE VOCE SOUBE FAÇA NO SEU NAVEGADOR QUE VOCE QUISER

1 COPIE O CODIGO  TODO 

2 ADICIONE QUALQUER PAGINA A BARRA DE FAVORITO DO NAVEGADOR COLOQUE QUALQUER NOME EXEMPLO scaner E DEPOIS EDITE A URL ONDE TA URL COLE O CODIGO DO SCRIPT E SALVE

3 TUDO PRONTO AGORA ABRA A URL DE UM SITE QUALQUER QUE QUEIRA SCANEAR E DEPOIS CLIQUE NO NA BARRA DO SCANER FAVORITO QUE VOCE SALVO NO NAVEGADOR

![image](https://github.com/user-attachments/assets/7ab44f43-f8a9-4f12-b6e3-815984307c31)




