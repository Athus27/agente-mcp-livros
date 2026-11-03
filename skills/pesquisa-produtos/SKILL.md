---
name: pesquisa-produtos
description: Pesquisa produtos em um site de vendas e gera um resumo em Markdown.
---

# Pesquisa de produtos

## Objetivo

Pesquisar produtos em um site de vendas usando termos fornecidos pelo usuário e gerar um resumo verificável em Markdown.

## Ferramenta de acesso

- Use Playwright como ferramenta principal para abrir e interagir com o site.
- Navegue com `page.goto` e aguarde `domcontentloaded`; depois aguarde os
  componentes dinâmicos aparecerem antes de ler os resultados.
- Prefira locators semânticos, `data-testid`, `aria-label`, `placeholder` e
  texto visível. Não dependa de classes CSS geradas ou de posições fixas.
- Se um campo de busca estiver oculto, abra o acionador visível da busca e só
  então preencha o campo editável.
- Após cada interação que altera a busca, aguarde a URL, o título ou o
  cabeçalho de resultados mudar e confirme que o termo pesquisado aparece na
  página.
- Não use `curl` como substituto da pesquisa no navegador: páginas de vendas
  podem exigir JavaScript, cookies ou proteção anti-robô.

### Fluxo Playwright para a OLX

1. Abra `https://www.olx.com.br/`.
2. Localize o campo de busca visível. Se houver um elemento somente leitura
   como acionador, clique nele e localize o novo campo editável.
3. Preencha o termo e pressione `Enter`, ou navegue diretamente para a URL de
   resultados equivalente, por exemplo
   `https://www.olx.com.br/brasil?q=<termo codificado>`.
4. No resultado, confirme o cabeçalho, a quantidade de resultados e os cards
   renderizados. Extraia somente texto, preço, links e localização exibidos no
   DOM.
5. Para filtrar uma cidade/estado, use o controle de localização exibido na
   página, como `Todo o Brasil` ou `Buscando em Todo o Brasil`; selecione a
   cidade e o estado na interface e confirme que o filtro ficou visível no
   resultado. Não presuma que `q` aplica localização.
6. Abra os primeiros cards em novas navegações Playwright quando a listagem
   não exibir todos os dados necessários. Volte à listagem ou use os links
   capturados para preservar a ordem original.
7. Capture uma tela de cada página de produto com Playwright, depois de a página
   terminar de carregar. Salve cada imagem em um caminho persistente relativo
   ao relatório e incorpore-a na seção correspondente do Markdown.

## Entradas

- **Site:** URL do site de vendas.
- **Termos de busca:** um ou mais nomes, palavras ou expressões a pesquisar.
- **Filtros (opcionais):** categoria, faixa de preço, disponibilidade ou outros filtros oferecidos pelo site.
- **Quantidade de resultados:** máximo de resultados a registrar por termo.

## Passo a passo

1. Acesse o site com Playwright.
2. Para cada termo, aplique os filtros opcionais, quando existirem.
3. **Estratégia de busca (em ordem):**
   1. Se houver campo de busca, pesquise o termo nele.
   2. Se não houver campo de busca ou o termo não for encontrado, navegue pela categoria mais provável.
   3. Se ainda não encontrar, percorra no máximo 5 páginas do catálogo geral e pare.
4. Para cada produto encontrado, até a quantidade solicitada, abra a página do produto e registre somente dados exibidos no site:
   - título;
   - preço;
   - disponibilidade;
   - avaliação;
   - categoria.
5. Para o primeiro resultado de cada termo, escreva um resumo de 2 ou 3 linhas da descrição, traduzido para português quando necessário.
6. Se não houver resultados, registre `não encontrado` e informe os locais consultados.
7. Salve o resultado no arquivo Markdown solicitado.
8. Antes das seções individuais, inclua uma tabela com todos os produtos
   confirmados, ordenada pelo preço numérico em ordem crescente. Preserve na
   tabela o preço exatamente como exibido e use o valor numérico apenas para
   ordenar. Inclua título, preço, localização e link.

### Falhas de acesso

- Se Playwright não conseguir iniciar, informe o erro técnico e pare; não
  substitua a navegação por dados inventados.
- Se a página carregar mas houver bloqueio, CAPTCHA, erro 4xx/5xx ou ausência de
  cards, registre o status como `não concluído`, a URL e a mensagem observada.
- Só informe `não encontrado` depois de uma página de resultados acessível ter
  sido consultada e não conter anúncios correspondentes.
- Não trate anúncios de outra cidade como resultados do filtro solicitado.
- A tabela de preços deve conter somente produtos confirmados e deve estar em
  ordem crescente; não ordene preços por texto ou pela ordem original da página.
- Cada produto confirmado deve ter uma imagem incorporada com Markdown no
  formato `![Captura de tela de ...](caminho/arquivo.png)`. Não use links
  quebrados, imagens externas não verificadas ou uma única captura para vários
  produtos.

## Regras

- Não invente resultados, atributos ou valores ausentes.
- Não registre dados pessoais de vendedores.
- Respeite o limite de no máximo 5 páginas do catálogo geral.
- Não continue a paginação geral depois de atingir esse limite.
- Diferencie claramente resultados encontrados de termos não encontrados.
- Use a moeda, a disponibilidade e a avaliação exatamente como exibidas, convertendo apenas a apresentação das estrelas quando necessário.

## Formato do arquivo de saída

```markdown
# Resultado da busca

- **Site:** [Nome do site](URL)
- **Termos:** `termo 1`, `termo 2`
- **Filtros:** filtros aplicados ou `nenhum`

## Tabela de preços (ordem crescente)

| # | Produto | Preço | Localização | Link |
|---|---|---:|---|---|
| 1 | Título do produto mais barato | valor exibido | localização exibida | [Abrir anúncio](URL) |

## Termo: termo 1

### 1. Título do produto

![Captura de tela de Título do produto](imagens/produto-1.png)

- **Preço:** valor exibido
- **Disponibilidade:** disponibilidade exibida
- **Avaliação:** quantidade de estrelas
- **Categoria:** categoria exibida
- **Descrição resumida:** resumo em português com 2 ou 3 linhas

## Termo: termo 2

**Não encontrado.** Locais consultados: ...
```
