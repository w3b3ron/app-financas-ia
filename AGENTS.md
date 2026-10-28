# AGENTS.md

Regras para qualquer agente ou desenvolvedor que trabalhe neste repositório. Contexto completo em `docs/HANDOFF.md`.

## Decisões fixas (não reabrir sem motivo)
- Núcleo próprio. **Não copiar código do OpenMonetis** (licença Sustainable Use 1.0, sem uso comercial). Estudar padrões é permitido; escrever do zero.
- Escopo: finanças pessoais + investimentos + IA.
- Backend em **Python** (FastAPI, Pydantic, SQLAlchemy, Alembic, PostgreSQL).
- Interface responsiva (celular e web), qualidade UI/UX padrão Apple.
- Caminho regulatório A (uso pessoal), desenhado para evoluir para B.

## Princípios
1. Código determinístico calcula notas, payoffs, gregas e impostos. A IA só explica, via tool use com saída estruturada. Nunca inventa número.
2. Sem linguagem imperativa de recomendação ("compre", "venda"). Nota descritiva: atrativo / neutro / cautela.
3. Todo número exibido tem fonte e data. Dado de exemplo é rotulado como tal.
4. Valores monetários sempre em `Decimal`, nunca `float`.
5. Ganho e perda sempre com sinal e seta, não só cor.

## Qualidade
- Testes antes do código de cálculo. Casos de aceite de opções em `docs/HANDOFF.md` (seção 9).
- CI com `ruff`, `mypy` e `pytest`.
- Registrar toda decisão nova em `docs/HANDOFF.md`, com data.

## Idioma
Documentação e interface em português do Brasil.
