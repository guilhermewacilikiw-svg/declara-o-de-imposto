# Organizador para o Imposto de Renda (Excel)

Ferramenta em Excel que reúne, num só lugar, as informações essenciais para a declaração de imposto de renda. O usuário preenche os dados com validação automática, navega por menu e vê um resumo calculado.

Arquivo: `declaracao_ir.xlsx`

> Todos os dados do arquivo (nome, CPF, bancos, valores) são fictícios, de exemplo.

## Objetivo

Ajudar quem vai declarar o imposto de renda a controlar as entradas de forma organizada e validada, com uma interface simples e sem precisar entender fórmulas.

## Estrutura da planilha

| Aba | Função |
|---|---|
| **MENU** | Página inicial: navegação, andamento do preenchimento, links rápidos e instruções |
| **TITULAR** | Dados pessoais e de contato |
| **INFORMES** | Informes de rendimentos de até 3 bancos, com total |
| **NOTAS** | Tabela de entradas de receita (data, categoria, valor) |
| **RESUMO** | Totais por categoria e por mês, com gráfico |
| **TABELAS** | Aba oculta com a lista de bancos e as categorias usadas nas listas suspensas |

Células **amarelas** são de preenchimento. Células **cinzas** são calculadas e não devem ser editadas.

## Recursos

### Menu de navegação
- O **MENU** tem links para cada aba (função `HYPERLINK` com `#aba!A1`).
- Cada aba tem o link **< Voltar ao menu** no topo.
- A coluna **PREENCHIDO** do menu conta quantos campos de cada aba já foram preenchidos, e a **SITUAÇÃO** mostra Completo, Em andamento ou Pendente.

### Validações automáticas
| Campo | Regra |
|---|---|
| CPF | exatamente 11 caracteres |
| CEP | exatamente 8 caracteres |
| E-mail | precisa conter `@` |
| Nascimento | data entre 01/01/1900 e hoje |
| Alterações, dependente, residente no exterior | lista SIM ou NÃO |
| Banco | lista de bancos (aba oculta TABELAS) |
| Valor dos informes | maior ou igual a zero |
| Data, categoria e valor das notas | data válida; categoria HOLERITE, CNPJ ou FREELANCE; valor maior que zero |

Em TITULAR, a coluna **SITUAÇÃO** mostra "Pendente" ou "OK" para cada campo.

### Links rápidos
No MENU: portal da Receita Federal e página "Meu Imposto de Renda" (gov.br).

### Resumo (agregador)
A aba **RESUMO** soma todas as notas lançadas: total de entradas, quantidade de lançamentos, média mensal, total em bancos, entradas por categoria (com percentual) e por mês, mais um gráfico de colunas. Use um arquivo por ano da declaração, porque o total por mês junta o mesmo mês de anos diferentes.

As fórmulas são curtas e usam intervalos nomeados:

| Cálculo | Fórmula |
|---|---|
| Total de entradas | `=SUM(valores)` |
| Quantidade de lançamentos | `=COUNT(valores)` |
| Total por categoria | `=SUMIF(categorias_notas, B12, valores)` |
| Total por mês | `=SUMPRODUCT((MONTH(datas)=C17)*valores)` |
| Situação no menu | `=IF(D7=13,"Completo","Pendente")` |

## Intervalos nomeados

| Nome | Refere-se a |
|---|---|
| `lista_bancos` | `TABELAS!A2:A51` |
| `lista_categorias` | `TABELAS!C2:C4` |
| `datas` | `NOTAS!B9:B500` |
| `categorias_notas` | `NOTAS!C9:C500` |
| `valores` | `NOTAS!D9:D500` |

## Como usar

1. Abra `declaracao_ir.xlsx` no **MENU**.
2. Preencha as abas 1, 2 e 3 (células amarelas). Em NOTAS, digite na primeira linha livre da tabela.
3. Acompanhe o andamento no menu e confira os totais no RESUMO.
4. Use esses dados para preencher o programa da declaração.

## O que foi feito em relação à planilha base

- Estrutura de abas mantida (TITULAR, INFORMES, NOTAS e a aba oculta TABELAS), com **dados fictícios próprios**.
- **Novo**: aba MENU com navegação, andamento do preenchimento, links rápidos e instruções.
- **Novo**: botão para voltar ao menu em todas as abas.
- **Novo**: aba RESUMO com totais, percentuais e gráfico.
- **Novo**: validação de CPF, CEP, e-mail, data de nascimento e valores, além das listas suspensas.
- **Novo**: coluna de situação (Pendente ou OK) nos dados do titular.
- **Fórmulas simplificadas**: poucas funções (`SUM`, `COUNT`, `COUNTA`, `SUMIF`, `IF`, `SUMPRODUCT`) e intervalos nomeados no lugar de endereços longos.
- A lista de bancos (código e nome) vem da aba oculta da planilha base.

## Observações

- O recurso de anexo da base foi mantido como o **nome do arquivo** do informe (campo de texto); a planilha não guarda os PDFs.
- A planilha ajuda a organizar os dados. Ela não substitui o programa oficial da Receita Federal nem faz cálculo de imposto.
