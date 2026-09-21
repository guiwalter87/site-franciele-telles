# Publicação no ar

Conta GitHub: **guiwalter87**
Repositório: **github.com/guiwalter87/site-franciele-telles** (público)
Domínio principal: **francieletelles.com.br**
Domínio de redirecionamento: **francieletellesclinica.com.br**
Ambos registrados no registro.br em 21/09/2026, com expiração em 21/09/2027.

---

## Situação atual

| Etapa | Estado |
| --- | --- |
| Repositório criado no GitHub | Concluído |
| Pages configurado como "GitHub Actions" | Concluído |
| Arquivos preparados e conferidos (26 arquivos, 0,9 MB) | Concluído |
| Primeiro push | Pendente, precisa ser rodado no Terminal do Mac |
| Zona DNS do domínio principal | Bloqueada pelo registro.br até o fim da transição |
| Custom domain no GitHub e HTTPS | Depois do DNS |
| Domínio de redirecionamento | Depois do principal no ar |

---

## Passo 1. Primeiro push

O `git init`, o `git add` e a branch `main` já estão feitos. No Terminal do macOS:

```bash
cd ~/Desktop/Guilherme/Fran/site-franciele-telles
git commit -m "Site Franciele Telles, versao inicial"
git remote add origin https://github.com/guiwalter87/site-franciele-telles.git
git push -u origin main
```

Se o GitHub pedir autenticação, ele abre o navegador para autorizar a conta.

Nota sobre o iCloud: a pasta fica na Área de Trabalho, que é sincronizada. No repositório
da Atto essa combinação já travou `git pull` e deixou `index.lock` preso. Como este site
terá poucas atualizações, o risco é baixo, mas use sempre `git pull --rebase --autostash`
ao invés do `git pull` simples.

## Passo 2. Conferir o deploy

Assim que o push subir, o workflow `.github/workflows/deploy.yml` roda sozinho e publica a
pasta `site/`. Acompanhe em `github.com/guiwalter87/site-franciele-telles/actions`.
Leva cerca de um minuto.

Endereço provisório para conferência e para a aprovação da Franciele:

```
https://guiwalter87.github.io/site-franciele-telles/
```

Escolhemos o modo GitHub Actions porque o Pages, no modo "Deploy from a branch", só aceita
a raiz ou `/docs`, e aqui a pasta de publicação é `site/`.

## Passo 3. Zona DNS no registro.br

O domínio foi registrado hoje e o painel informa "Domínio em transição", com previsão de
cerca de duas horas. Enquanto isso o editor de zona aceita a tela mas não grava registros.
É o comportamento normal para domínios recém-registrados, não é erro de configuração.

Quando liberar, em `registro.br/painel/dominios/?dominio=francieletelles.com.br`, na seção
DNS, opção "Configurar zona DNS" em modo avançado:

| Tipo | Nome | Dados |
| --- | --- | --- |
| A | (vazio, o domínio raiz) | 185.199.108.153 |
| A | (vazio) | 185.199.109.153 |
| A | (vazio) | 185.199.110.153 |
| A | (vazio) | 185.199.111.153 |
| AAAA | (vazio) | 2606:50c0:8000::153 |
| AAAA | (vazio) | 2606:50c0:8001::153 |
| AAAA | (vazio) | 2606:50c0:8002::153 |
| AAAA | (vazio) | 2606:50c0:8003::153 |
| CNAME | www | guiwalter87.github.io. |

Os quatro AAAA são opcionais mas recomendados, e cobrem visitantes em redes IPv6. O ponto
final em `guiwalter87.github.io.` é obrigatório no padrão do registro.br.

## Passo 4. Ligar o domínio no GitHub

Ponto de atenção que corrige a instrução anterior: no modo **GitHub Actions**, o GitHub
**ignora** o arquivo `site/CNAME`. Ele só é lido no modo "Deploy from a branch". O domínio
precisa ser digitado à mão.

Em Settings, Pages, campo **Custom domain**, digitar `francieletelles.com.br` e salvar.
O GitHub valida o DNS na hora. Depois que o certificado for emitido, de alguns minutos a
algumas horas, marcar **Enforce HTTPS**. Se a opção estiver cinza, é só o certificado ainda
não estar pronto.

O arquivo `site/CNAME` fica no repositório de propósito: ele não atrapalha e serve de
registro do domínio pretendido, além de funcionar caso um dia o modo de publicação mude.

## Passo 5. Domínio de redirecionamento

O GitHub Pages aceita **um** domínio por repositório, então o segundo precisa do seu próprio.

1. Criar o repositório público `francieletellesclinica-redirect`
2. Subir o conteúdo da pasta `redirect-francieletellesclinica/` na raiz dele
3. Settings, Pages: Source = Deploy from a branch, Branch `main`, Folder `/ (root)`
   (neste modo o arquivo `CNAME` da pasta é lido e o domínio é configurado sozinho)
4. Marcar Enforce HTTPS quando o certificado sair
5. No registro.br, na zona DNS desse domínio, os **mesmos quatro registros A**, os
   **mesmos quatro AAAA** e o CNAME de `www`

O redirecionamento é por `meta refresh` mais `canonical` mais `location.replace`, porque o
GitHub Pages não faz 301 de verdade. Para um domínio defensivo, sem histórico e sem
autoridade a preservar, isso não tem custo prático.

## Passo 6. Depois de no ar

- [ ] Abrir `https://francieletelles.com.br` e conferir em celular e desktop
- [ ] Conferir se `www.francieletelles.com.br` redireciona para o domínio raiz
- [ ] Testar os botões de WhatsApp, inclusive os de cada serviço
- [ ] Colar o container do GTM nos dois pontos marcados no `index.html`
- [ ] Verificar a propriedade no Google Search Console usando o prefixo de URL
      `https://francieletelles.com.br/` e enviar o `sitemap.xml`
- [ ] Trocar o campo Site do Perfil da Empresa no Google por
      `https://francieletelles.com.br/?utm_source=google&utm_medium=organic&utm_campaign=gbp_perfil`
- [ ] Atualizar o link da bio do Instagram, também com UTM
- [ ] Marcar o evento `whatsapp_click` como Evento Principal no GA4
- [ ] Rodar o PageSpeed Insights e guardar a medição inicial

## Como publicar uma alteração daqui em diante

```bash
cd ~/Desktop/Guilherme/Fran/site-franciele-telles
git add .
git commit -m "descricao do que mudou"
git push
```

O workflow publica em cerca de um minuto. Se algo falhar, o GitHub manda e-mail para o dono
do repositório e o log fica na aba Actions.
