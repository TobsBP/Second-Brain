# Second Brain

Vault do Obsidian com minhas anotações da faculdade (Inatel), dos projetos pessoais e dos estudos de engenharia de software.

## Estrutura

```
Materias/            ← uma pasta por disciplina (código-sigla)
  <COD-SIGLA>/
    *.md             ← notas de aula
    Atividades/      ← listas, relatórios, trabalhos
    Provas/          ← practice exams e revisões
    Anexos/          ← imagens coladas nas notas
projects/            ← uma pasta por projeto pessoal (arquitetura, notas, desenhos)
Melhor Engenheiro/   ← estudos guiados pelo RoadMap.pdf, uma pasta por tópico
  Prática/           ← briefings de produto → requisitos, ADRs e código (ver Roteiro.md)
```

## Disciplinas

| Pasta | Disciplina |
|-------|------------|
| `C09-CG` | Computação Gráfica e Multimídia |
| `C12-SO` | Sistemas Operacionais |
| `C24-AI` | Inteligência Artificial |
| `M09-PO` | Pesquisa Operacional |
| `S02-BD2` | Banco de Dados 2 (Neo4j) |
| `S03-ArqSoft` | Arquitetura de Software |
| `S07-QGCE` | Qualidade e Gerência de Configuração |
| `T03-Redes` | Redes |

## Convenções

- Notas em português, estilo caderno. O nome do arquivo é o título (sem `#` H1); seções com `##` e `###`.
- Conteúdo que não veio da aula fica em `> [!note] Complemento`; correções de respostas em `> [!warning] Correção`.
- Desenhos são arquivos `.excalidraw.md` (plugin Excalidraw). Em `Melhor Engenheiro/`, cada tópico tem a nota e um `<Tópico> Draw` embutido no topo.
- Imagens coladas vão para `Anexos/` ao lado da nota (configurado em `.obsidian/app.json`).
