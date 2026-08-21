# Drone Marília — Landing Page

Landing page de página única para serviços de fotografia e videografia com drone.
HTML e CSS puros, sem build e sem dependências: basta abrir o `index.html`.

## Estrutura

```
dronemarilia-lp/
├── index.html      # a página inteira (HTML + CSS embutido)
├── assets/
│   └── logo.png    # logo usada no header, rodapé e favicon
└── README.md
```

## Como visualizar

Abra o `index.html` no navegador. Para testar com servidor local:

```bash
python -m http.server 5173
```

E acesse `http://localhost:5173`.

## O que precisa ser preenchido antes de publicar

Todos os pontos abaixo estão com valores provisórios.

### 1. Telefone do WhatsApp

Procure por `5514999999999` no `index.html` e troque em **todas** as ocorrências
(header, hero, três botões de plano, CTA final, rodapé, botão flutuante e o
bloco JSON-LD). O formato é `55` + DDD + número, sem espaços ou símbolos.

Os links já levam uma mensagem pronta. Nos planos, cada botão abre o WhatsApp
citando o pacote escolhido, então o cliente já sabe do que a pessoa está falando.
Se mudar o texto da mensagem, lembre de codificar a URL (espaço vira `%20`,
acento vira `%C3%A1` e afins).

### 2. Redes sociais e e-mail

Procure por `INSTAGRAM_AQUI`, `YOUTUBE_AQUI` e `contato@dronemarilia.com.br`.

### 3. Fotos e vídeos

A galeria do portfólio usa espaços reservados. Cada um é um bloco assim:

```html
<figure class="item obras largo slot ar-169">
  <span class="tag">Obras</span>
  <span>Foto aérea da obra<br>16:9 · 3840×2160</span>
</figure>
```

Para colocar a mídia real, troque o conteúdo interno por uma imagem:

```html
<figure class="item obras largo ar-169">
  <img src="assets/obra-01.jpg" alt="Vista aérea da obra do Residencial X" loading="lazy">
</figure>
```

Detalhes importantes:

- Remova a classe `slot` (é ela que desenha o placeholder tracejado).
- Mantenha a classe de proporção (`ar-1`, `ar-43` ou `ar-169`) para o layout não
  pular enquanto a imagem carrega.
- A classe de categoria (`obras`, `imoveis`, `eventos`, `empresas`) é o que faz o
  filtro funcionar. Um item pode ter mais de uma.
- A classe `largo` faz o item ocupar a largura inteira.
- Exporte em WebP quando possível e mantenha as imagens abaixo de 300 KB — a
  página é leve e não vale a pena perder isso na galeria.

Para vídeo, o mais econômico é subir no YouTube e usar um `<iframe loading="lazy">`
dentro do `figure`, ou usar `<video controls poster="assets/capa.jpg">` se o
arquivo for hospedado junto.

### 3.1. Vídeo de fundo do hero

O `<video>` já está no HTML e começa a rodar sozinho assim que os arquivos
existirem. Não precisa editar nada — basta colocar na pasta `assets/`:

| Arquivo                  | O que é                                            |
| ------------------------ | -------------------------------------------------- |
| `assets/hero.mp4`        | vídeo em loop, sem áudio                           |
| `assets/hero-poster.jpg` | primeiro frame, aparece enquanto o vídeo carrega   |

Recomendações para o vídeo: 8 a 15 segundos, 1920×1080, H.264, **até 5 MB**.
Vídeo de fundo pesado é o jeito mais fácil de deixar a página lenta no 4G, e o
corte é em loop, então o ganho de qualidade acima disso não aparece na tela.

O vídeo entra entre duas camadas: um gradiente de base embaixo e um véu escuro
com brilho vermelho em cima, que é o que garante a leitura do título. Enquanto
os arquivos não existirem, o gradiente aparece no lugar e o hero continua com o
mesmo visual — a única diferença é um 404 no console.

### 4. Planos e preços

Os valores (`R$ 350` e `R$ 690`) são de referência. Ajuste nomes, preços e itens
na seção `PLANOS` do HTML conforme a tabela real.

### 5. Depoimentos

A seção está com texto genérico de propósito. Publique só com depoimentos reais
e autorização para usar nome e empresa. Enquanto não houver nenhum, o melhor é
remover a seção inteira (o bloco `<section ... id="depoimentos">`) em vez de
deixar texto de exemplo no ar.

### 6. Domínio

Ao definir o domínio, atualize a tag `<link rel="canonical">` e a `og:image`
para uma URL absoluta — redes sociais não exibem preview com caminho relativo.

## Decisões técnicas

- **Mobile first.** Todo o CSS base é para telas pequenas; os `@media` só
  ampliam o layout a partir de 760px e 900px.
- **Praticamente sem JavaScript.** O menu, os filtros do portfólio e o FAQ
  funcionam com `checkbox`, `radio` e `<details>`. No FAQ, o atributo
  `name="faq"` nos `<details>` é o que faz o navegador manter só uma pergunta
  aberta por vez, sem script.
  Existe um único bloco de ~12 linhas de JS no fim do arquivo, e ele é opcional:
  fecha o menu mobile ao clicar num link e, em navegadores antigos que não
  suportam `<details name>`, replica o comportamento de acordeão. Removendo o
  script, a página continua inteira funcionando.
- **Sem scroll horizontal.** A faixa vermelha inclinada é mais larga que a tela
  de propósito; quem segura isso é o container `.faixa-area` com
  `overflow:hidden`. No `html` e no `body` há `overflow-x:clip` como rede de
  segurança — `clip` em vez de `hidden` porque `hidden` no `html` quebra o
  `position:sticky` do header em alguns navegadores.
- **SEO local.** Título, meta description, Open Graph e um JSON-LD de
  `LocalBusiness` com as cidades atendidas, que é o que ajuda a página a
  aparecer em buscas do tipo "drone em Marília".
- **Acessibilidade.** Contraste alto, foco visível no teclado e respeito a
  `prefers-reduced-motion` (a faixa vermelha e o pulso do WhatsApp param para
  quem pediu menos animação no sistema).

## Publicação

Como é um site estático, sobe em qualquer lugar. Netlify, Vercel, Cloudflare
Pages ou GitHub Pages funcionam arrastando a pasta, sem configuração.
