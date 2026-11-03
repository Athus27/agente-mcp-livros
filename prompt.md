# Registro dos prompts e da execução

## Objetivo

Manter o histórico organizado das solicitações feitas para a skill
`pesquisa-produtos`, incluindo prompts, falhas, acertos, discrepâncias e a
solução aplicada. Cada pesquisa possui seu próprio arquivo de resultado.

## Prompts registrados

### 1. Pesquisa inicial na OLX

> @SKILL.md acesse https://www.olx.com.br/ e pesquise "Play 5" em joão
> monlevade MG resuma os 5 primeiros anuncios, dando certo ou errado crie e
> registre no prompt.md

**Resultado:** falha de acesso na primeira tentativa. O navegador informou que
o Chromium não estava disponível e a tentativa HTTP recebeu bloqueio do
Cloudflare. O resultado provisório foi registrado no próprio `prompt.md`.

### 2. Correção solicitada para o acesso

> o chromium ja esta, era pra acessar usando playright

**Resultado:** a skill foi modificada para priorizar Playwright, lidar com
campos dinâmicos da OLX, aplicar localização pela interface e diferenciar
bloqueio de ausência de resultados.

### 3. Nova tentativa com a skill modificada

> agora faça uma nova tentativa usando a skill @SKILL.md modificada e registre no
> @prompt.md

**Resultado:** acerto. O Playwright filtrou `Play 5` em João Monlevade/MG. A
OLX exibiu quatro resultados, dos quais dois foram confirmados com dados
completos. O relatório foi atualizado com os anúncios confirmados.

### 4. Tabela e capturas de tela

> modifique a skill, no relatório de busca quero visualizar uma tabela que contem
> os preços ordenados em ordem crescente, quero ver essa tabela. Agora na seção
> de cada produto quero que você capture a tela (print de cada livro) e cole no
> relatório

**Resultado:** a skill passou a exigir tabela de preços crescente e uma captura
por produto. As capturas dos dois anúncios da OLX foram salvas em `imagens/` e
incorporadas ao relatório.

### 5. Atualização da pesquisa de livros

> agora atualize a pesquisa dos livros utilizando a skill atualizada @SKILL.md

**Resultado:** acerto. A pesquisa `Harry Potter` e `The Chronicles of Narnia`
foi executada com Playwright. O relatório de livros recebeu tabela crescente e
três capturas em `livros/`. `The Chronicles of Narnia` permaneceu como não
encontrado.

### 6. Prompt atual — organização dos relatórios

> blz, agora atualize o prompts.md com nosso dialogo, seguindo o padrão do
> documento, registre uma discrepancia entre o local do salvamento da pesquisa do
> olx, era pra salvar só caso desse errado, o motivo e a solução abordada, mas o
> resultado da pesquisa ainda era em um resultado-busca.md , então crie um
> resultado-busca.md para a pesquisa do olx, e deixa o prompt como um relatório
> organizado, registrando os prompts, falhas e acertos, inclusive quero esse
> prompt registrado lá

**Resultado:** acerto. Este arquivo passou a ser o relatório organizado do
diálogo, e a pesquisa da OLX foi separada em
[resultado-busca.md](./resultado-busca.md).

## Discrepância de salvamento

### Problema

O primeiro erro de acesso à OLX foi registrado em `prompt.md`, conforme a
solicitação inicial. Depois, quando a pesquisa foi concluída com sucesso, o
relatório da OLX continuou no mesmo arquivo. Isso misturou histórico de
execução com resultado de pesquisa e divergiu do padrão usado para a pesquisa
dos livros.

### Motivo

O nome `prompt.md` foi interpretado inicialmente como o arquivo de saída
solicitado para a tentativa da OLX. A mudança posterior para um relatório
organizado de prompts não foi acompanhada da separação imediata do resultado
bem-sucedido.

### Solução abordada

- `prompt.md` agora contém somente o histórico organizado dos prompts,
  falhas, acertos e decisões.
- `resultado-busca.md` agora contém exclusivamente o relatório da OLX.
- `resultado-busca-livros.md` preserva exclusivamente o relatório da pesquisa
  dos livros.
- As imagens dos anúncios permanecem em `imagens/`.
- As imagens dos livros permanecem em `livros/`.

## Estado final

| Item | Estado |
|---|---|
| Skill com Playwright | Atualizada |
| Pesquisa OLX | Concluída com 2 anúncios confirmados |
| Tabela OLX | Ordenada por preço crescente |
| Capturas OLX | 2 imagens incorporadas |
| Histórico dos prompts | Registrado neste arquivo |
| Discrepância de salvamento | Documentada e corrigida |
