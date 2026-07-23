# Pacote do app "Ferramentas Top RB"

Este pacote contém tudo que o app precisa para funcionar como PWA (Progressive Web App)
e para ser transformado em um `.apk` instalável no Android:

```
pwa_package/
├── index.html          → o app (calculadoras + relatório diário, com menu)
├── manifest.json        → configurações do app (nome, ícone, cor, tela cheia)
├── sw.js                 → service worker (faz o app funcionar offline)
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    ├── icon-512-maskable.png
    └── apple-touch-icon.png
```

Eu não consigo compilar o `.apk` diretamente aqui (isso exige o Android SDK e acesso à
internet, que não tenho neste ambiente). Mas com este pacote pronto, gerar o `.apk` é
rápido. Abaixo estão os dois caminhos mais fáceis.

---

## Caminho mais fácil: PWABuilder (sem precisar instalar nada)

1. **Hospede os 3 arquivos + a pasta `icons`** em algum lugar público na internet.
   Formas gratuitas e simples:
   - **GitHub Pages**: crie um repositório, suba esses arquivos, ative o Pages nas
     configurações do repositório.
   - **Netlify Drop** (https://app.netlify.com/drop): arraste a pasta inteira do
     `pwa_package` no navegador e ele já te dá um link público, sem precisar criar conta.
   - **Vercel**, **Firebase Hosting**, etc. também funcionam.

2. Copie o link público que apontar para o `index.html` (ex: `https://seusite.netlify.app/`).

3. Acesse **https://www.pwabuilder.com**, cole esse link e clique em **Start**.

4. O PWABuilder vai analisar o `manifest.json` e o `sw.js` automaticamente (ele vai
   mostrar notas verdes confirmando que o app está pronto para virar pacote nativo).

5. Clique em **Package for Stores** → escolha **Android** → baixe o pacote gerado.
   Dentro dele vem o `.apk` (ou `.aab`, se preferir para a Play Store) pronto para instalar.

6. Transfira o `.apk` para o celular (por cabo, WhatsApp, Google Drive, etc.) e instale
   normalmente (pode ser necessário permitir "instalar de fontes desconhecidas" nas
   configurações do Android).

---

## Caminho alternativo: Bubblewrap (linha de comando, mais controle)

Se você tiver Node.js instalado no seu computador:

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest="https://seusite.netlify.app/manifest.json"
bubblewrap build
```

Isso gera o `.apk`/`.aab` localmente. Também exige que o app esteja hospedado
publicamente (mesmo passo 1 do caminho acima), além do Android SDK (o próprio
Bubblewrap ajuda a baixar na primeira execução).

---

## Por que precisa estar hospedado online?

O jeito padrão de empacotar um PWA como app Android usa uma tecnologia chamada
**TWA (Trusted Web Activity)**: o app instalado abre a versão hospedada na internet
dentro de uma "casca" nativa, e usa o `sw.js` para funcionar offline depois da primeira
abertura. Por isso o `manifest.json` e o `index.html` precisam estar acessíveis por uma
URL pública antes de gerar o `.apk` — mesmo que depois o uso no dia a dia seja offline.

Se preferir um app 100% offline desde a instalação (sem depender de link nenhum), o
caminho é outro (empacotar com **Capacitor**, que embala o HTML dentro do próprio
`.apk`). Posso montar esse projeto também, se for o que você prefere — é só avisar.
