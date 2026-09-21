# Site Franciele Telles

Site institucional da **Franciele Telles, Clínica Estética e Terapias de Spa**, em Flores da Cunha/RS.
Página única, HTML estático, sem framework e sem dependência externa além das fontes do Google.
Preparado para publicação no GitHub Pages com domínio próprio.

---

## Estrutura

```
site-franciele-telles/
├── README.md                    este arquivo
├── .gitignore
├── site/                        raiz de publicação (é esta pasta que vai para o GitHub Pages)
│   ├── index.html               a página inteira, com CSS e JSON-LD embutidos
│   ├── 404.html
│   ├── robots.txt
│   ├── sitemap.xml
│   ├── .nojekyll
│   └── img/                     imagens do site
├── docs/
│   ├── pendencias.md            o que falta a Franciele confirmar antes de publicar
│   ├── prompts-de-imagens.md    prompts de IA e roteiro das fotos reais
│   └── deploy-github-pages.md   passo a passo da publicação e do DNS
└── marca/                       ativos originais da identidade
    ├── logo-colorido-original.png
    ├── logo-fundo-branco.png
    ├── logo-branco.png
    ├── logo-preto.png
    ├── CaviarDreams.ttf
    ├── CaviarDreams_Bold.ttf
    └── paleta.md
```

## Como editar

Tudo vive em `site/index.html`. O arquivo é único de propósito: CSS, conteúdo e dados
estruturados estão no mesmo lugar, o que evita cache desencontrado e facilita a manutenção
por quem pegar o projeto depois.

Pontos de atenção ao mexer:

1. Os blocos com a classe `ph` são espaços reservados de imagem. Para trocar por foto real,
   substitua a `div` inteira por `<img src="img/NOME.jpg" alt="descrição" loading="lazy">`.
   O nome de arquivo esperado está escrito dentro de cada bloco.
2. Todos os links de WhatsApp carregam `data-wa` com a seção de origem. Isso alimenta o
   evento `whatsapp_click` no dataLayer. Ao criar um link novo, mantenha o atributo.
3. O JSON-LD no fim do arquivo precisa acompanhar qualquer mudança de endereço, telefone
   ou horário. NAP divergente entre site e Perfil da Empresa no Google prejudica o local.
4. Comentários marcados com `CONFIRMAR` apontam textos que ainda são rascunho meu e
   dependem do aval da Franciele. Veja `docs/pendencias.md`.

## Regras de conteúdo (combinadas com a cliente)

- **A clínica não vende produtos.** Nenhuma menção a venda de produto em lugar nenhum.
- **Atendimento exclusivo para mulheres.** Todo o texto usa o feminino, e a FAQ responde isso de forma direta e acolhedora.
- **Nenhum preço exposto.** A FAQ responde a pergunta de valor e direciona ao WhatsApp.
- **Avaliações apenas de clientes mulheres**, reproduzidas no texto original do Google.
- Vocabulário de esteticista e massoterapeuta, nunca de clínica médica.

## Identidade

Gradiente da marca, extraído da logo original: azul `#3E93D1`, índigo `#6277BD`, violeta `#9A7BB6`.
Tagline oficial: **Saúde & Bem-Estar**.
Fonte da marca é a Caviar Dreams. Na web usamos Jost, que é o equivalente geométrico com
licença aberta e eixo variável, pareada com Fraunces nos títulos. Detalhes em `marca/paleta.md`.

## Aviso importante sobre git e iCloud

Esta pasta está na Área de Trabalho, que o macOS sincroniza pelo iCloud Drive por padrão.
No repositório do site da Atto essa combinação já causou travamento de `git pull`, arquivos
`index 2` dentro de `.git/` e `index.lock` preso. **Antes de rodar `git init` aqui, mova o
projeto para fora do iCloud**, por exemplo para `~/Code/site-franciele-telles/`. Se optar por
manter onde está, use sempre `git pull --rebase --autostash`.
