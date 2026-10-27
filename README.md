# DrusaQI

Aplicação de teste exploratório de raciocínio, com 40 questões originais, cálculo realizado no servidor e relatório premium bloqueado até confirmação de pagamento.

## Executar localmente

Requer Node.js 20 ou superior.

```powershell
Copy-Item .env.example .env
npm test
npm start
```

Abra `http://localhost:3000`. Não use o arquivo HTML diretamente: o fluxo depende da API do servidor.

## O que está implementado

- 40 questões (10 por categoria), cada uma com quatro alternativas, dificuldade, pontuação e explicação.
- Progresso salvo a cada resposta no servidor e no navegador; voltar e revisar respostas é permitido.
- Cálculo reprodutível de acertos, erros, percentual e desempenho por categoria no servidor.
- Paywall sem resultado falso: a oferta mostra apenas o que será entregue.
- Proteção de relatório: a API retorna o relatório somente quando `paymentStatus` é `approved`.
- Checkout preparado para Mercado Pago e webhook com verificação HMAC SHA-256 / idempotência.
- Eventos de funil mínimos, sem enviar respostas individuais a plataformas publicitárias.
- Páginas de termos, privacidade e reembolso em versão inicial.

## Configurar Mercado Pago antes de produção

1. Crie uma aplicação, habilite Checkout Pro via Preferences e configure Webhooks no painel do Mercado Pago.
2. Configure `MP_ACCESS_TOKEN`, `MP_WEBHOOK_SECRET` e `APP_BASE_URL` HTTPS no ambiente de hospedagem. Se usar armazenamento local temporariamente, defina também `DATA_DIR` no disco persistente.
3. Aponte a notificação para `https://seu-dominio.com.br/api/webhooks/mercadopago`.
4. Faça compras de teste e valide os estados aprovado, pendente, recusado e reembolsado.

O servidor cria uma preferência de Checkout Pro usando o preço mantido exclusivamente no servidor. Ele não trata nem armazena dados de cartão. Sem essas variáveis, o botão informa claramente que o checkout não está configurado; nenhuma compra é simulada. Para uma implantação nova, avalie migrar para a Orders API recomendada atualmente pelo Mercado Pago antes de colocar o produto em produção.

## Limites antes de investir em anúncios

Este repositório usa `data/store.json` como armazenamento local de desenvolvimento. Antes de produção, substitua-o por banco gerenciado (PostgreSQL), adicione migrações, backup, limitação de taxa, e-mail transacional para recuperação de relatório, gestão de consentimento de cookies/pixels, revisão jurídica de LGPD/termos/reembolso e uma revisão psicométrica das questões. Publique somente sob HTTPS e configure observabilidade, logs seguros e monitoramento de webhooks.

## Publicação

Hospede como serviço Node persistente (por exemplo, Render, Fly.io, Railway ou VPS), defina as variáveis de ambiente pelo painel seguro do provedor, conecte um domínio HTTPS e configure o webhook do Mercado Pago. Não coloque segredos em arquivos versionados, JavaScript do navegador ou HTML.
