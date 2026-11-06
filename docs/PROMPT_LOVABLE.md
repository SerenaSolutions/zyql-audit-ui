# Prompt de construção — ZYQL Audit Console

Cole o bloco abaixo no Lovable. Ele é fechado de propósito: definir stack, telas e
contrato antes evita que a ferramenta invente endpoint ou calcule veredito no navegador.

---

## Bloco para colar

Construa um painel chamado **ZYQL Audit Console**. Ele não calcula nada: ele chama uma
API HTTP já existente e exibe o que ela respondeu, com o recibo verificável ao lado.

### Regras invioláveis

1. O veredito exibido é sempre o campo `verdict` da resposta. Nunca derive, arredonde
   nem recalcule veredito no cliente.
2. `REFUSED` não é erro. É resposta de primeira classe, com `reason` obrigatório, e deve
   aparecer com o mesmo peso visual de `1` e `0`, nunca como toast de falha.
3. Não invente rota. As únicas rotas existentes estão em `docs/openapi.json`.
4. Se a API falhar, mostre o motivo real devolvido pelo servidor. Não substitua por
   mensagem genérica.
5. Nenhum dado sensível no localStorage. O recibo pode ir para download, não para storage.

### Stack

React + Vite + TypeScript, Tailwind, componentes shadcn. Variável de ambiente
`VITE_ZYQL_API_URL` com a base da API. Sem backend próprio, sem banco, sem autenticação
própria.

### Telas

**1. Decidir** — formulário com `oracle` (const1, const0, parity), `n` (1 a 8), `shots`
(1 a 100000), `epsilon` (0 a 0.5), `family` (1 a 1000), `seed` inteiro. Botão "Executar".
Resultado mostra: veredito em destaque trivalente, `p0`, `margin` (banda), `shots`,
`mode`, `reason` quando houver. Abaixo, o recibo completo em JSON recolhível, com o
`selo_sha3_256` em destaque e botão para copiar e para baixar `.json`.

**2. Limites** — lê `GET /limits` e mostra duas colunas: "o que o sistema declara" e "o
que ele NÃO alega". Essa tela existe para impedir que alguém leia o painel como promessa
de vantagem quântica.

**3. Verificar recibo** — textarea para colar um recibo JSON e botão que chama
`POST /verify` com `{"receipt": {...}}`. Resultado binário: íntegro ou adulterado, com a
diferença do selo quando adulterado.

**4. Oráculo** — `POST /oracle/reveal` com `mask` (inteiro) e `n`, e `POST /oracle/promise`
com `truth_table` (array de 2 a 256 inteiros). Mostra a máscara revelada e o parecer.

### Semântica visual

- `1`: verde sóbrio, rótulo "CERTIFICADO".
- `0`: vermelho sóbrio, rótulo "REPROVADO".
- `REFUSED`: âmbar, rótulo "SEM EVIDÊNCIA", com o motivo sempre visível ao lado.
- Ícone nunca por emoji; use ícone vetorial ou apenas tipografia.
- Tema claro e escuro seguindo a preferência do sistema, sem cor fixa no código.

### Estados

Carregando com esqueleto. Vazio com instrução curta. Erro com o código HTTP e a mensagem
do servidor. Sucesso com horário local da consulta.

### Acessibilidade

Formulários com `label` associado, foco visível, contraste mínimo AA, navegação completa
por teclado e anúncio do veredito em `aria-live`.

### Entrega

Um repositório GitHub com README explicando a variável `VITE_ZYQL_API_URL`, `.env.example`
e um teste de fumaça que renderiza a tela Decidir com a API mockada.

---

## Depois de gerar

1. Conectar o repositório gerado ao time `SerenaSolutions` no GitHub.
2. Rodar localmente contra a API do Replit e conferir três coisas: veredito idêntico ao da
   API, `REFUSED` mostrado com motivo, e recibo baixável com selo intacto.
