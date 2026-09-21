# Instagram no site

## Decisão atual

A seção do Instagram é uma **chamada para seguir o perfil**, sem feed embutido.
Nenhuma imagem do Instagram é carregada no site.

O motivo é de risco, não de preguiça: um feed embutido que para de atualizar envelhece
o site inteiro, e foi exatamente o erro que encontramos no benchmark, com concorrente
exibindo na home o último post de 2024 em plena navegação de 2026. Enquanto não houver
uma rotina confiável de atualização, a chamada simples entrega o mesmo valor
(mandar a visitante para o perfil ativo) sem nenhum risco de envelhecer.

## O cenário técnico, verificado em 21/09/2026

**A Instagram Basic Display API foi desligada em 4 de dezembro de 2024.** Não foi
depreciação gradual: toda requisição retorna erro. Isso invalida praticamente todo
tutorial, plugin e snippet anterior a essa data, inclusive os que ainda ranqueiam bem
no Google. Fonte: changelog oficial da Meta.

A substituta relevante é a **Instagram API with Instagram Login**, lançada em 23/07/2024,
em `graph.instagram.com`. Para ler as próprias mídias ela exige:

| Requisito | Situação |
| --- | --- |
| Conta Professional (Business ou Creator) | Obrigatória, em qualquer caminho oficial |
| Página do Facebook vinculada | Não exige, nesse fluxo |
| App no Meta for Developers | Obrigatório |
| App Review e Verificação de Negócio | **Não exige**, porque o app serve apenas a própria conta (Standard Access) |
| Escopo | `instagram_business_basic` |

**O ponto crítico é o token.** O de longa duração vale 60 dias e se renova por um GET
em `graph.instagram.com/refresh_access_token`. A documentação é explícita: um token que
passa 60 dias sem renovação expira e **não pode mais ser renovado**, exigindo refazer o
OAuth no navegador. Trocar a senha do Instagram também invalida o token.

Outra armadilha: as URLs em `media_url` do CDN da Meta **expiram em poucos dias**. Linkar
direto para elas quebra sozinho. Baixar as imagens não é otimização, é requisito.

## Caminhos de upgrade, se um dia fizer sentido

Em ordem de recomendação, com custo anual e esforço estimado:

| Caminho | Custo/ano | Esforço | JS de terceiro | Risco de parar sozinho |
| --- | --- | --- | --- | --- |
| Behold.so gratuito como fonte JSON, GitHub Actions baixando as imagens | R$ 0 | 2 a 4 h | nenhum | muito baixo |
| API oficial própria, com Actions renovando o token todo dia | R$ 0 | 4 a 8 h | nenhum | médio, por causa dos 60 dias |
| Widget do Behold colado direto na página | R$ 0 | 15 min | um web component leve | muito baixo |
| LightWidget pago | R$ 77 uma vez, mais R$ 201/ano com otimização de imagem | 30 min | iframe mais script | baixo |
| Embed oficial de posts escolhidos a dedo | R$ 0 | 20 min | mais de 100 KB | alto, é manual |

**O primeiro caminho é o que eu faria.** A dona autoriza o Instagram no Behold uma vez,
clicando num link, e o Behold passa a ser o responsável eterno pelo app e pelo token.
Um GitHub Action diário busca o JSON em `feeds.behold.so`, baixa as imagens já em WebP,
grava em `site/img/instagram/`, gera o HTML com `srcset` e `loading="lazy"`, apaga o que
saiu do ar e commita. O site publicado continua sem uma única linha de JavaScript de
terceiro, e se o Action quebrar as imagens já commitadas seguem no ar, congeladas mas
funcionando. O plano gratuito entrega 6 posts, que é exatamente o que a faixa comporta.

Pendência antes de adotar: confirmar por e-mail com o suporte do Behold se o consumo do
JSON por CI conta contra o limite de 1.200 visualizações por mês do plano gratuito.
A estimativa é de cerca de 60 chamadas mensais, bem dentro do limite, mas não achei
texto deles autorizando ou proibindo explicitamente esse uso.

**O que eu não faria:** Elfsight, por travar a thread principal, e Curator, EmbedSocial,
Taggbox e Juicer, que custam de R$ 834 a R$ 2.050 por ano para entregar menos do que o
Behold entrega de graça. E jamais qualquer biblioteca que use a API privada do Instagram,
porque viola os termos da Meta e quebra sem aviso.

## Providência que independe do caminho

A conta **precisa ser Professional**, Business ou Creator, para qualquer integração
oficial, inclusive nos serviços pagos. Se ainda for pessoal, converter é o passo zero:
são poucos toques nas configurações do Instagram, é gratuito e reversível, e de quebra
libera as métricas do perfil. Vale conferir mesmo sem decidir pelo feed.

## Sobre a permissão de guardar as imagens

Os Meta Platform Terms em vigor desde 31/08/2020 **não estabelecem prazo máximo de cache**.
O limite de 24 horas que circula em fóruns vem da política antiga, revogada. A obrigação
real é funcional: manter os dados sincronizados e apagar quando o conteúdo sair do ar.
Um build diário que reescreve o conjunto do zero cumpre isso naturalmente. No caso aqui
o risco é ainda menor, porque a dona do conteúdo, a autorizadora do app e a dona do site
são a mesma pessoa.
