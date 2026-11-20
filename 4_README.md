# GS 2026.2 · CitizenTech — Assistente de Mobilidade Urbana 🚌

**Disciplina:** Prompt and Artificial Intelligence · **Turma:** 1CCPA · **Prof.:** Jorge Luiz Gomes
**Frente escolhida:** A — Mobilidade urbana
**Integrante:** [NOME: Erick Vieiro Martins RM: 575683]

## O que é
Assistente que responde ao passageiro, em linguagem simples, se um ponto/linha de ônibus está funcionando, lotado ou interditado, com horário, atraso esperado, acessibilidade e alternativa. Usa dados simulados de uma prefeitura fictícia (13 registros, 5 com status fora do normal) e só responde com base neles.

## Arquivos
| Arquivo | Conteúdo |
|---|---|
| `1_Notebook_GS2026_2.ipynb` | Notebook com as seções 0 a 7 |
| `2_dados_servico.json` | Dados simulados (gerado pelo notebook) |
| `3_prompts_kit.md` | System prompt final, v1, v2, few-shot e iterações |
| `4_README.md` | Este arquivo |
| `5_certificacao.txt` | Tela final do notebook + links + RMs |
| `6_Chat_Log.txt` | Registro do uso de IA |

## Como executar
1. Abra o notebook no Google Colab (**Arquivo → Abrir notebook → GitHub**, ou faça upload).
2. Em **Secrets** (ícone de chave), crie `OLLAMA_API_KEY` com a sua chave do Ollama Cloud e ative o acesso ao notebook.
3. Rode **Runtime → Run all**.
4. Preencha `NOTAS_HUMANAS` na Seção 5 com as notas do grupo e rode a célula de novo.
5. Se a sessão cair, rode por blocos; o JSON é regravado nas Seções 1 e 3.

> 🔐 A chave nunca é escrita no notebook, no JSON ou neste README. Sem a chave, o notebook roda mesmo assim, usando só o fallback determinístico.

## Prints
*[Inserir: (1) Run all concluído da Seção 1; (2) tabela original × resumo IA; (3) tabela de auditoria com coluna `fonte`; (4) matriz 5×2; (5) bloco JSON da Seção 6]*

## Decisões técnicas
- **Dados sem `class`**: lista de dicionários, conforme a regra do 1º semestre.
- **4 regras de negócio**: id único (duplicado renumerado), `acessivel` booleano (contagem de correções), `horario` normalizado (`24h` e intervalos que cruzam a meia-noite, como 04:30–00:30) e `status_motivo` obrigatório quando o status não é `ativo`. Para `ativo` sem motivo, preenchemos "operação normal" e contamos a correção.
- **Temperatura 0.1** no enriquecimento: tarefa de classificação, queremos resultado estável.
- **Validação + fallback**: JSON inválido, urgência fora do domínio, resumo vazio ou > 140 caracteres, incoerência com o status e padrão de injection levam à regra determinística (`interditado/inativo ⇒ urgente`; `lotado/em_manutencao ⇒ atencao`; `ativo ⇒ ok`). A auditoria mostra `fonte = llm | regra`.
- **Mitigações**: "fala a verdade ou não fala nada" (recusa com Central 156) e relato como entrada hostil (aspas triplas + declaração no prompt; o registro FZC-007 contém uma injection de teste).
- **Bônus implementados**: desambiguação, fallback regra → IA → humano, trilha de auditoria com versão do prompt e pergunta em linguagem informal.

## Resultados da avaliação
*[Resumir a média humana v1 × v2 da matriz e a evidência de melhoria.]*

## Registro de uso de IA
Usamos IA (Claude) para gerar o rascunho do código e dos textos. Revisamos, executamos e entendemos o material; as notas da matriz são humanas. Detalhes em `6_Chat_Log.txt`.
