# Sistema de Alertas de Transações

## Escopo do Projeto
O sistema consiste em simular a aprovação de uma transação financeira e o envio de um alerta/notificação para o cliente. Ele terá 2 microsserviços, 1 gateway e 1 broker.

### Componentes da Arquitetura:
- API Gateway: A porta de entrada. Ele recebe as requisições e aplica uma regra simples de Rate Limiting.

- Serviço de Transações (Transaction-Service): Tem seu próprio banco de dados.
    - Recebe a transação, salva o registro e posta um evento na fila: TransacaoCriada.

- Message Broker (Fila): Usa o RabbitMQ rodando via Docker.

- Serviço de Alertas (Alert-Service): Serviço de background (worker). Não tem rotas HTTP.
    - Fica escutando a fila. Quando chega o evento TransacaoCriada, ele simula o envio de um e-mail/SMS (apenas printa no terminal).
    - Implementa Idempotência para não mandar o mesmo alerta duas vezes.