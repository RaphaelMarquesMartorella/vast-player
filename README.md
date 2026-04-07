# Player VAST/VPAID - Metrike

Player de video com suporte completo a tags VAST (2.0/3.0/4.0) e VPAID (2.0).

## Tecnologias

- [Video.js 8](https://videojs.com/) - Player de video HTML5
- [Google IMA SDK](https://developers.google.com/interactive-media-ads) - SDK de anuncios do Google
- [videojs-ima](https://github.com/googleads/videojs-ima) - Plugin oficial do Google para Video.js

## Como Testar Localmente

### Opcao 1: Python (mais simples)

```bash
cd vast-vpaid-player
python3 -m http.server 8080
```

Acesse: http://localhost:8080

### Opcao 2: Node.js

```bash
npx serve .
```

### Opcao 3: PHP

```bash
php -S localhost:8080
```

## Arquivos

- `index.html` - Pagina principal com interface de teste
- `embed.html` - Pagina para incorporacao via iframe

## Como Usar

### 1. Testar uma Tag VAST/VPAID

1. Abra `index.html` no navegador
2. Cole a URL da tag VAST/VPAID no campo
3. Selecione o modo VPAID desejado:
   - **Enabled** (padrao): Permite VPAID em iframe seguro
   - **Insecure**: Permite VPAID sem restricoes (use com cuidado)
   - **Disabled**: Desabilita VPAID completamente
4. Clique em "Carregar Tag"
5. Clique no play para iniciar o anuncio

### 2. Incorporar em Outro Site (Embed)

Use o iframe gerado:

```html
<iframe
    src="https://seu-dominio.com/embed.html?vast=URL_DA_TAG_ENCODADA"
    width="640"
    height="360"
    frameborder="0"
    allowfullscreen
    allow="autoplay; encrypted-media"
></iframe>
```

#### Parametros do embed.html

| Parametro | Descricao | Exemplo |
|-----------|-----------|---------|
| `vast` | URL da tag VAST (URL encoded) | `vast=https%3A%2F%2F...` |
| `vpaid` | Modo VPAID (0=disabled, 1=insecure, 2=enabled) | `vpaid=2` |
| `content` | URL do video de conteudo | `content=https://...video.mp4` |
| `autoplay` | Iniciar automaticamente (1 ou true) | `autoplay=1` |

## Exemplo com a Tag do Fabio

```
http://localhost:8080/embed.html?vast=https%3A%2F%2Fvast.doubleverify.com%2Fv3%2Fvast%3F_media%3D3%26ctx%3D42465539%26cmp%3DDV2159697%26sid%3D40725%26plc%3D181570%26adsrv%3D166%26blk%3D1%26psf%3D1%26_vast%3Dhttps%253A%252F%252Fservedby.metrike.com.br%252Fvast.spark%253FsetID%253D68524%2526ID%253D181570%26_api%3D%5BAPIFRAMEWORKS%5D%26_ssm%3D%5BSERVERSIDE%5D%26_tsm%3D%5BTIMESTAMP%5D%26gdpr%3D%24%7BGDPR%7D%26gdpr_consent%3D%24%7BGDPR_CONSENT_126%7D%26_abm%3D%5BAPPBUNDLE%5D%26_pum%3D%5BPAGEURL%5D
```

## Comunicacao via PostMessage (para integracao avancada)

O player embed envia eventos para a janela pai:

```javascript
window.addEventListener('message', function(e) {
    if (e.data.source === 'vast-player') {
        console.log('Evento:', e.data.event); // ads-ready, ad-loaded, ad-started, ad-ended, ad-error
    }
});
```

Voce pode controlar o player:

```javascript
iframe.contentWindow.postMessage({ target: 'vast-player', action: 'play' }, '*');
iframe.contentWindow.postMessage({ target: 'vast-player', action: 'pause' }, '*');
iframe.contentWindow.postMessage({ target: 'vast-player', action: 'reload' }, '*');
```

## Troubleshooting

### Erro de CORS

Se aparecer erro de CORS, o servidor que hospeda a tag VAST precisa permitir requisicoes cross-origin. Isso e configurado no servidor da tag, nao no player.

### VPAID nao funciona

- Tente o modo "Insecure" se o modo "Enabled" nao funcionar
- Algumas tags VPAID de terceiros podem ter problemas com o Google IMA SDK

### Anuncio nao carrega

- Verifique se a URL da tag esta correta
- Verifique o console do navegador (F12) para ver erros detalhados
- Algumas tags tem restricoes de dominio

## Deploy

Para deploy, basta hospedar os arquivos em qualquer servidor web estatico:
- Vercel
- Netlify
- GitHub Pages
- AWS S3 + CloudFront
- Qualquer servidor Apache/Nginx

## Licenca

MIT
