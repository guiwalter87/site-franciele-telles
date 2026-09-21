# Redirecionamento de francieletellesclinica.com.br

Repositório separado, com a única função de redirecionar o domínio secundário para
`francieletelles.com.br`.

Existe porque o GitHub Pages aceita **um** domínio personalizado por repositório, então o
segundo domínio precisa do seu próprio repositório com o seu próprio arquivo `CNAME`.

## Como publicar

1. Criar um repositório público no GitHub, por exemplo `francieletellesclinica-redirect`
2. Subir o conteúdo desta pasta na raiz dele
3. Em Settings, Pages: Source = Deploy from a branch, Branch = `main`, Folder = `/ (root)`
4. Em Custom domain, informar `francieletellesclinica.com.br` e marcar Enforce HTTPS
5. No registro.br, apontar o DNS desse domínio para os mesmos IPs do GitHub Pages

## Limitação conhecida

O GitHub Pages não faz redirecionamento 301 de verdade. Este é o padrão possível:
`meta refresh`, `canonical` apontando para o domínio principal, `noindex` e um
`location.replace` no JavaScript. Do ponto de vista de SEO é mais fraco que um 301, o que
não é problema aqui porque o domínio secundário é defensivo e não tem histórico nem
autoridade a preservar.
