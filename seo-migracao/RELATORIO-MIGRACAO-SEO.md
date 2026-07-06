# Relatório de Migração e SEO — CR Medicina Fetal

**Última atualização:** 06/07/2026

## Resumo em uma frase

Migramos o site de um WordPress antigo, pesado e instável (`crmedicinafetal.com`) para um site novo, estático e rápido (`crmedicinafetal.com.br`), preservando o máximo possível do que já rankeava no Google — e o que ainda não foi preservado (o blog) está identificado e priorizado abaixo.

---

## 1. Como era antes

- Site em WordPress no domínio **`crmedicinafetal.com`**, construído com Elementor + uma pilha pesada de plugins (KingComposer, WooCommerce, Jetpack, Optimole, Ajax Search Lite, Sucuri Security, WPvivid Backup, Super Page Cache, Ultimate Post Kit). Essa combinação de plugins concorrentes era a causa provável da instabilidade que motivou a criação do site novo.
- URLs com estrutura antiga (`/exames/us-3d-e-4d/`, `/perfil/`, etc.), sem sitemap.xml, sem robots.txt, sem tag canonical em nenhuma página.
- Um blog com ~44 posts publicados que, sozinhos, respondiam pela maior parte do tráfego orgânico do site (ver seção 5).

## 2. Como está ficando o site novo

- Site estático (HTML puro) hospedado na **Vercel**, no domínio **`crmedicinafetal.com.br`**.
- 40 páginas no total: 20 em português (raiz) + 20 em inglês (`/en/`).
- URLs limpas, sem `.html` (ex: `/clinica` em vez de `/clinica.html`), via `cleanUrls: true` no `vercel.json`.
- O blog **ainda não tem casa definitiva** — ver seção 6, é o item mais importante em aberto.

---

## 3. O que já foi feito — Migração e Infraestrutura

| Item | Status |
|---|---|
| Site novo publicado na Vercel (`crmedicinafetal.com.br`) | ✅ Feito |
| `cleanUrls: true` ativado — remove `.html` das URLs | ✅ Feito |
| Confirmado: Vercel redireciona automaticamente `.html` → URL limpa (308) | ✅ Confirmado ao vivo |
| Links internos das 40 páginas atualizados para URLs limpas | ✅ Feito |
| Plugin **Redirection** instalado no WordPress antigo | ✅ Feito |

## 4. O que já foi feito — SEO

| Item | Status | Detalhe |
|---|---|---|
| Redirects 301 de alta confiança | ✅ 37 redirects | Página antiga → equivalente exata no site novo |
| Redirects 301 de confiança média | ✅ 2 redirects | Mapeamento correto, mas vale confirmar o conteúdo bate 100% |
| Redirects apontando direto para URL limpa (sem hop duplo) | ✅ Feito | Evita redirecionar 2x (antiga → .html → limpa) |
| `sitemap.xml` | ✅ Criado do zero | Não existia. 40 URLs, com `hreflang` PT↔EN |
| `robots.txt` | ✅ Criado do zero | Não existia. Referencia o sitemap |
| Tag `<link rel="canonical">` | ✅ Nas 40 páginas | Reforça pro Google qual URL é a "oficial", evitando duplicidade |
| Meta description ausente detectada e corrigida | ⚠️ Parcial | `perfil.html` e `en/perfil.html` não tinham description — **ainda não corrigido, ver seção 6** |
| Link "Blog" quebrado no menu (apontava pra `blog.html`, inexistente) | ✅ Corrigido | Agora aponta para `crmedicinafetal.com/blog/`, testado em desktop e mobile |
| Ajuste de conteúdo SEO na página `morfologico-1o-trimestre.html` | ✅ Feito | Title, description e corpo do texto agora citam "osso nasal" e "translucência nucal" — os termos com melhor posição de todo o site (ver seção 5) |

**Páginas que ainda precisam de verificação manual (baixa/média confiança):** 16 páginas — em sua maioria páginas de arquivo automático do WordPress (autor, categoria, data) e duplicatas, mapeadas provisoriamente. Lista completa em `seo-migracao/mapeamento-redirects.csv`.

## 5. O que os dados reais do Google Search Console mostraram

Período analisado: últimos 3 meses, propriedade `crmedicinafetal.com`.

- **306 cliques totais**, a maioria no Brasil (277) e em celular (247 — **81% do tráfego é mobile**).

**Top páginas por clique:**

| Página | Cliques | Situação |
|---|---|---|
| `/nao-vi-o-osso-nasal-e-problema/` (post do blog) | 80 | ⚠️ Sem destino definido no site novo |
| `/a-transluscencia-nucal-do-meu-bebe-esta-aumentada-e-agora/` (post do blog) | 66 | ⚠️ Sem destino definido |
| `/preciso-fazer-exame-transvaginal-na-morfologica-de-2o-t/` (post do blog) | 52 | ⚠️ Sem destino definido |
| `/exames/us-3d-e-4d/` | 37 | ✅ Já redirecionado |
| `/` (home) | 31 | ✅ Já redirecionado |
| Demais páginas institucionais | ~40 | ✅ Já redirecionadas |

**Achado principal: só esses 3 posts de blog somam 198 dos 306 cliques — cerca de 65% de todo o tráfego orgânico do site.** Eles ainda não têm URL de destino definida no site novo (status `pendente_blog`).

**Palavras-chave com melhor posição:**
- "osso nasal morfológico 1 trimestre" — posição 1.45
- "tn aumentada e bebê normal" — posição 1.35
- "cardiotocografia basal" — posição 1.36
- "medicina fetal rio de janeiro" — posição 3.06

Temas dominantes: ultrassom 3D/4D, osso nasal, translucência nucal, morfológica de 1º trimestre.

---

## 6. Próximos passos — em ordem de prioridade

1. 🔴 **Decidir onde o blog vai morar de vez.** Hoje ele está sendo formatado no WordPress antigo (como ambiente de teste, mais fácil de editar), mas o destino final ainda está em aberto: WordPress novo (`novo.crmedicinafetal.com`), o próprio WordPress antigo formatado, ou outra solução. Essa decisão destrava tudo mais abaixo.
2. 🔴 **Recriar pelo menos os 3 posts campeões de clique** (osso nasal, translucência nucal, exame transvaginal) no destino escolhido, com conteúdo igual ou melhor que o original.
3. 🔴 **Criar redirect 301 específico para cada post** do blog assim que a página de destino existir — nunca redirecionar um post específico para uma página de índice genérica (isso é tratado quase como "página removida" pelo Google, mesmo sendo um 301).
4. 🟡 **Submeter o `sitemap.xml`** na propriedade `crmedicinafetal.com.br` do Google Search Console (confirmar antes que essa propriedade está verificada).
5. 🟡 **Corrigir a meta description ausente** em `perfil.html` e `en/perfil.html`.
6. 🟡 **Revisar as 16 páginas "verificar"** (arquivos de autor/categoria/data do WordPress antigo, baixa prioridade mas bom fechar o mapeamento).
7. 🟢 Depois que o blog estiver 100% migrado com mapeamento 1:1, considerar usar a ferramenta **"Alteração de Endereço"** do Search Console no domínio antigo, pra acelerar a transferência de autoridade de uma vez só.
8. 🟢 Verificar o plano da conta Semrush/Ahrefs (retornou "plano insuficiente" mesmo reconectado) — uma vez ativo, permite acompanhar ranking de forma automatizada em vez de exportação manual do GSC.

---

## 7. Como a autoridade de SEO é transferida (referência rápida)

Cliques, impressões e ranking são contabilizados **por domínio** no Google — `crmedicinafetal.com` e `crmedicinafetal.com.br` são propriedades totalmente separadas. Não existe transferência automática só por serem o mesmo negócio. A única forma de transferir a autoridade de uma URL antiga é um **redirect 301 direto para a URL nova equivalente** — depois disso, o Google leva tipicamente **2 a 8 semanas** para re-rastrear e mover o sinal de ranking. Enquanto uma página não tiver esse redirect específico, toda a autoridade dela continua presa no domínio antigo.

---

## Arquivos de referência neste projeto

- `seo-migracao/mapeamento-redirects.csv` — mapeamento completo de todas as 103 URLs (antigas → novas), com status e confiança
- `seo-migracao/redirection-import.csv` — arquivo de importação usado no plugin Redirection
- `sitemap.xml` — sitemap do site novo
- `robots.txt` — robots.txt do site novo
