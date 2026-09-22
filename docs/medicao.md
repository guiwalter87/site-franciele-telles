# Medição do site

Conta Google proprietária dos ativos: **francieletellesclinica@gmail.com**
Configurado em 22/09/2026.

---

## Google Analytics 4

| Item | Valor |
| --- | --- |
| Conta | Franciele Telles |
| Propriedade | francieletelles.com.br |
| ID da conta | 408994487 |
| ID da propriedade | 555348854 |
| Fluxo de dados | Site Franciele Telles |
| ID do fluxo | 15823669522 |
| **ID de medição** | **G-7NSV32WPTR** |
| Fuso dos relatórios | GMT-03:00, São Paulo |
| Moeda | Real brasileiro |
| Setor | Beleza e fitness |
| Objetivos de negócio | Gerar leads, Entender o tráfego da Web |

A tag está em `site/index.html`, logo antes do `</head>`, no formato gtag direto.
Não usamos Google Tag Manager. Se um dia entrar mídia paga e a necessidade de
adicionar pixels sem publicar o site, vale migrar para GTM.

### Evento de conversão

`whatsapp_click`, disparado em qualquer link com o atributo `data-wa`.

| Parâmetro | Conteúdo |
| --- | --- |
| `origem` | qual botão foi clicado (cabecalho, hero, facial, corporal, spa, plano, rodape, flutuante) |
| `pagina` | caminho da página |

São 31 pontos de disparo na página. O código tem um caminho alternativo que
empurra para o `dataLayer` caso a tag do Google não carregue, então o site
não quebra se o Analytics estiver bloqueado no navegador da visitante.

---

## Search Console

| Item | Valor |
| --- | --- |
| Tipo de propriedade | Domínio |
| Propriedade | francieletelles.com.br |
| Método de verificação | Registro TXT na zona DNS do registro.br |
| Registro usado | `google-site-verification=Q51pGIZxULEGZFQupwEdNQHWh168PBKEWpJz1XbKh0Q` |

A propriedade de domínio cobre o domínio raiz, o www e tanto http quanto https.

**Não remova esse TXT da zona DNS.** Se ele sair, a verificação cai e o histórico
de dados do Search Console deixa de ser coletado.

Sitemap `https://francieletelles.com.br/sitemap.xml` enviado e processado em 22/09/2026.
Indexação da home solicitada manualmente na mesma data.

---

## Vínculos ativos

**Search Console.** Habilita, dentro do Analytics, os relatórios "Consultas" e
"Tráfego de pesquisa orgânica", que mostram o termo pesquisado no Google e o que
a visitante fez depois no site.

**Perfil da Empresa no Google.** Ficha vinculada em 22/09/2026:
"Franciele Telles - Clínica Estética e Terapias de Spa's". Traz para o Analytics
os dados de interação com o perfil, como cliques em ligar e em rotas.

## Marcação de origem do Perfil da Empresa

O campo Site do Perfil da Empresa no Google passou a apontar para:

```
https://francieletelles.com.br/?utm_source=gbp&utm_medium=organic&utm_campaign=primary-website-link
```

Alterado em 22/09/2026, com revisão do Google pendente na ocasião.

Por que esses valores, e não `utm_source=google`: com `google` o tráfego do perfil
ficaria indistinguível da busca orgânica comum dentro do Analytics, que é
justamente o que queremos separar. Com `gbp` a origem fica identificável, e
`utm_medium=organic` mantém o tráfego classificado como orgânico, que é o
correto, já que o perfil é descoberta e não mídia paga.

O `utm_campaign` identifica qual recurso do perfil gerou o clique. Se um dia
entrar link de agendamento ou postagens com link, use outro valor de campaign
para cada um, mantendo source e medium iguais.

**Ponto de atenção:** o Google às vezes remove parâmetros UTM do campo Site do
Perfil da Empresa sem aviso. Vale conferir esse campo a cada dois ou três meses.

---

## Pendências

- [ ] Marcar `whatsapp_click` como Evento Principal no GA4. Só é possível depois
      que o Google listar o evento, o que leva até 24 horas a partir do primeiro
      disparo. Caminho: Administrador, Exibição de dados, Eventos, e clicar na
      estrela ao lado do nome.
- [x] Vincular o Perfil da Empresa no Google ao GA4. Feito em 22/09/2026.
- [x] Marcar a origem do campo Site do Perfil da Empresa com UTM. Feito em
      22/09/2026, aguardando revisão do Google.
- [ ] Conferir, alguns dias depois, se o Google manteve os parâmetros UTM no
      campo Site do perfil.
- [ ] Atualizar o link da bio do Instagram, também com UTM.
- [ ] Rodar o PageSpeed Insights e guardar a medição inicial.

---

## Registro de verificação feita em 22/09/2026

A tag carrega na página, o `gtag` fica disponível e o clique dispara a
requisição correta para o Google, com o ID de medição certo e os dois
parâmetros. Conferido na aba de rede do navegador:

```
en=page_view       tid=G-7NSV32WPTR
en=whatsapp_click  tid=G-7NSV32WPTR  ep.origem=cabecalho  ep.pagina=/
```

Nas primeiras horas o endpoint do Google respondeu 503 a essas requisições.
É o comportamento esperado para uma propriedade recém-criada, enquanto o ID de
medição termina de ser provisionado na infraestrutura de coleta. A implementação
está correta, os dados começam a aparecer sozinhos.
