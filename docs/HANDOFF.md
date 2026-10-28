# Handoff do projeto: app de finanças pessoais com investimentos e IA

Documento de passagem para outro agente (ou desenvolvedor) continuar o projeto sem depender da conversa original. Escrito em 28/10/2026.

## 1. Em uma frase

App responsivo (celular e web) de finanças pessoais com módulo de investimentos (FIIs, ações, stablecoins, estratégias com opções) e uma camada de IA que **explica** análises calculadas por código determinístico. Núcleo próprio em Python, interface com qualidade de app Apple.

## 2. O que já existe

| Item | Situação | Onde |
|---|---|---|
| Documento de ideia aperfeiçoada | Pronto (privado) | https://claude.ai/artifact/KrUKy6XUBTaBqwwaFnBww8 |
| Proposta visual (3 telas em celular e web) | Pronta, não verificada visualmente | `design/` |
| Auditoria do OpenMonetis | Feita | Seção 6 |
| Plano do projeto | **Não iniciado** | Seção 10 |
| Código do app | **Não iniciado** | Fase 0 |

## 3. Decisões já tomadas (não reabrir sem motivo)

- **Núcleo próprio**, sem fork do OpenMonetis; ele serve só como referência de padrões, com regra de clean-room (seção 6).
- Escopo: finanças pessoais + investimentos + IA.
- **Python** no motor e na API.
- Interface com qualidade **UI/UX padrão Apple**.
- **Responsivo**: um único app para celular e web.
- Caminho regulatório **A (uso pessoal)**, desenhado para evoluir para **B (serviço informativo)**.

## 4. Princípios de produto

1. Código determinístico calcula notas, payoffs, gregas e impostos. A IA só explica, via tool use com saída estruturada. Nunca inventa número.
2. Nada de recomendação imperativa: sem "compre" ou "venda". Nota descritiva em faixas **atrativo / neutro / cautela**.
3. Todo número exibido tem fonte e data. Dados de exemplo são rotulados.
4. Dinheiro sempre em `Decimal`, nunca `float`.
5. Ganho e perda sempre com sinal e seta, nunca só cor.

## 5. Stack decidida

- Backend: Python, FastAPI, Pydantic, SQLAlchemy, Alembic, PostgreSQL, `Decimal`, NumPy/SciPy.
- Qualidade: pytest, ruff, mypy.
- Interface: React + TypeScript + Tailwind + Radix + Framer Motion (recomendação; o Python segue fixo no backend). Streamlit e Reflex foram descartados por não alcançarem o polimento pedido.
- IA: LLM com tool use e structured output.
- Opcional: PWA para instalar no celular.

## 6. OpenMonetis: o que sabemos e a regra de uso

- Repositório: github.com/felipegcoutinho/openmonetis-v2 (finanças pessoais em TypeScript: Hono, Drizzle, TanStack Start, Better Auth).
- Licença **Sustainable Use License 1.0**: não permite uso comercial nem distribuição. Por isso não copiamos código.
- Auditoria: 622 testes passam; 26 vulnerabilidades em dependências de produção (3 críticas); não tem investimentos nem IA.
- Regra clean-room: pode-se estudar padrões (modelagem de contas, categorias, lançamentos, importação), mas o código é escrito do zero, sem colar trechos.

## 7. Regulatório e tributário (resumo)

- CVM Res. 19, 20 e 21: recomendação personalizada exige pessoa autorizada. Por isso o app descreve, não aconselha.
- Lei 15.270/2025: retenção de 10% sobre dividendos acima de R$ 50 mil por mês por pagador.
- BCB Res. 519, 520 e 521: stablecoins e transferências cripto tratadas como operações de câmbio.
- LGPD se aplica se houver usuários além do dono.
- Pendência: definir quem revisa tributário e jurídico (seção 10).

## 8. Dados e fontes

- **brapi**, plano gratuito: 15 mil requisições por mês, 1 ticker por chamada, 3 meses de histórico, atraso de cerca de 30 minutos, sem dividendos. Exige cache agressivo e fila de atualização.
- **CoinGecko** (plano Demo) para cripto e stablecoins.
- Dividendos e fundamentos precisam de outra fonte ou importação manual. Escolha em aberto.

## 9. Opções: casos de teste obrigatórios

Resultado por ação no vencimento (S_T = preço no vencimento):

- Lançamento coberto: `L = (S_T − S0) + p − max(S_T − K, 0)`
- Trava de alta com call: `L = max(S − K1, 0) − max(S − K2, 0) − (c1 − c2)`

| Caso | Entradas | Esperado |
|---|---|---|
| Lançamento coberto | S0 = 30, K = 33, prêmio = 0,60 | lucro máx. 3,60; equilíbrio 29,40 |
| Trava de alta | K1 = 30 paga 1,80; K2 = 33 recebe 0,60 | lucro máx. 1,80; equilíbrio 31,20; prejuízo máx. 1,20 |

Gregas por Black-Scholes. Na proposta visual, os valores das gregas são só de exemplo. Custos, impostos e liquidez da série ainda não entram no cálculo.

## 10. Fases e o que falta

Fases 0 a 8 estão na tabela de módulos do documento de ideia. As fases 0 e 1 (base e finanças pessoais) são construídas por nós, não adaptadas.

**Fase 0 inclui:** design system (tokens abaixo), as 3 telas-chave prototipadas em celular e web (já em `design/`), repositório, CI, banco e migrações.

**Decisões em aberto, necessárias para o plano do projeto:**

- [ ] Escopo da primeira versão (o que entra no MVP).
- [ ] Corretoras e bolsas a importar primeiro.
- [ ] Orçamento mensal para dados e IA.
- [ ] Horas por semana e data-alvo.
- [ ] Quem revisa tributário e jurídico.
- [ ] Nome do app (hoje o placeholder é "[Nome do app]").
- [ ] Fonte de dividendos e fundamentos.
- [ ] Tema escuro e tela de lançamento coberto (não prototipados).

## 11. Design: tokens e regras

- Fundo `#F2F2F7`; cartões brancos com raio 20 a 24; texto `#1C1C1E`; secundário `#6B6B70`.
- Acento `#0060DF`; ganho `#1B7F3B`; perda `#C23B22`; nota azul `#0A4FA8` sobre `#E3EEFC`; laranja `#B35C00` / `#D97B0E`.
- Fonte do sistema com Inter de reserva; números tabulares.
- Alvos de toque de 44 px.
- Breakpoints: até 599 celular; 600 a 1023 tablet; 1024 ou mais web. Sem rolagem horizontal. No web, a lateral vira barra inferior abaixo de 820 px.
- Testar em 360, 390, 768, 1024 e 1440 px, incluindo Safari no iPhone.

Detalhes da proposta visual: `design/README.md`.

## 12. Riscos conhecidos

- Limite do plano gratuito da brapi.
- Alucinação da IA em números: mitigada pelo princípio 1.
- Enquadramento regulatório se o app deixar de ser de uso pessoal.
- Cálculo tributário de opções e cripto sem revisão profissional.

## 13. Como o próximo agente deve começar

1. Ler `docs/IDEIA.md` (e o documento completo linkado) e a proposta visual em `design/`.
2. Fazer ao Weber as perguntas da seção 10 ainda sem resposta e só então escrever o **plano do projeto** (marcos, estimativas, backlog por fase, critérios de aceite).
3. Iniciar a fase 0: estrutura FastAPI, modelos, migrações, CI com ruff, mypy e pytest, e os testes da seção 9 antes de qualquer interface.
4. Não copiar código do OpenMonetis.
5. Registrar cada decisão nova aqui, com data.

### Prompt sugerido para o novo agente

> Você assume o projeto descrito neste repositório. Leia `docs/HANDOFF.md`, `docs/IDEIA.md` e `AGENTS.md`, e respeite as decisões da seção 3 e os princípios da seção 4. Primeiro, confirme comigo as decisões em aberto da seção 10 e produza o plano do projeto em PT-BR. Depois de aprovado, comece a fase 0. Escreva testes antes do código de cálculo e use `Decimal` em todo valor monetário.

## 14. Preferências de trabalho do Weber

- Comunica-se em português do Brasil.
- Trabalha com BI na saúde e tem experiência com FastAPI e PostgreSQL em outros projetos.
- Prefere ver propostas e decisões antes de execuções grandes.
