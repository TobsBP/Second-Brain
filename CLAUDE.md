# CLAUDE.md

Vault pessoal do Obsidian (anotações da faculdade, projetos, estudos). Estrutura e lista de disciplinas estão no [README](README.md).

## Regras ao editar notas

- **Preserve a voz do Tobias.** É caderno pessoal, não livro. Corrija ortografia, formatação e fatos errados; não reescreva o que está certo só por estilo.
- **Marque o que é seu.** Conteúdo novo (que não veio da aula) vai em `> [!note] Complemento`. Correção de resposta em lista/prova vai em `> [!warning] Correção` com a explicação. Completar frase cortada ou corrigir erro pequeno pode ser inline.
- **Formato:** sem `#` H1 (o Obsidian usa o nome do arquivo), `##` para seções, `###` para subseções, sem frontmatter. Fórmulas em LaTeX (`$...$`, `$$...$$`). Links internos com `[[wikilink]]`, com moderação.
- **Não edite** arquivos `.excalidraw.md` nem imagens. Nunca remova embeds `![[...]]`, mesmo que a imagem esteja faltando.
- **Não invente.** Na dúvida, prefira conteúdo padrão de livro-texto e sinalize palpites explicitamente.

## Onde colocar arquivos

- Nota de aula → `Materias/<COD-SIGLA>/`
- Lista, relatório, trabalho → `Materias/<COD-SIGLA>/Atividades/`
- Practice exam / revisão → `Materias/<COD-SIGLA>/Provas/`
- Imagem → `Anexos/` ao lado da nota que a usa
- Disciplina nova → pasta `<CÓDIGO>-<Sigla>` (ex.: `C12-SO`) e uma linha na tabela do README
- Projeto pessoal → `projects/<Nome>/`

## Histórico

A antiga estrutura `raw/` + `wiki/` (resumos gerados, em inglês) foi removida em `3335aff`. Se precisar de algum resumo antigo: `git show 3335aff~1:wiki/subjects/<pasta>/<arquivo>.md`.

## Git

Commits sem `Co-Authored-By` e sem menção ao Claude. `.obsidian/workspace.json` muda sozinho a cada uso; não precisa de commit próprio.
