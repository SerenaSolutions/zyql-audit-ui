# ZYQL Audit Console — interface

Painel de auditoria que consome a **ZYQL Decision API** e mostra o veredito trivalente
com o recibo selado ao lado. A interface não calcula nada: todo veredito exibido veio do
servidor.

## Contrato

- [`docs/openapi.json`](docs/openapi.json) — especificação gerada do próprio servidor
- [`docs/EXEMPLOS_API.json`](docs/EXEMPLOS_API.json) — respostas reais, capturadas da API
- [`docs/PROMPT_LOVABLE.md`](docs/PROMPT_LOVABLE.md) — prompt de construção para o Lovable

## Rotas da API

`GET /health`, `GET /limits`, `POST /bound`, `POST /decide`, `POST /verify`,
`POST /oracle/reveal`, `POST /oracle/promise`.

## Regras de produto

1. Veredito sempre vem do campo `verdict` da resposta.
2. `REFUSED` é resposta legítima, com motivo, e não erro de tela.
3. Rota que não está no OpenAPI não existe.
4. Falha da API mostra o motivo real do servidor.

## Como rodar

```bash
cp .env.example .env      # VITE_ZYQL_API_URL=http://localhost:8000
npm install
npm run dev
```

A API sobe do pacote Python: `pip install "zephirum[api]"` e
`uvicorn api.server:app --host 0.0.0.0 --port 8000`.
