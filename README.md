# FGC manifests infraestructure

## Kubernets infraestructure:

<img width="1536" height="1024" alt="kubernets arquitecture" src="https://github.com/user-attachments/assets/266d2722-4e47-4e1f-9af0-a13604a28ac9" />

## Assincronous service infraestructure:

Produção de eventos

Quando um microsserviço realiza uma operação crítica (ex.: PaymentsAPI processa um pagamento ou UsersAPI cria um usuário), ele publica um evento no RabbitMQ.

Exemplo de eventos:

UserCreatedEvent → publicado pelo UsersAPI

PaymentProcessedEvent → publicado pelo PaymentsAPI

GameCreatedEvent → publicado pelo GamesAPI

Filas do RabbitMQ

Cada tipo de evento vai para uma fila dedicada, permitindo que consumidores diferentes recebam apenas os eventos que lhes interessam.

Exemplo:

Fila user-events → consumida por EmailLambda ou serviços de notificação.

Fila payment-events → consumida por NotificationAPI ou AccountingAPI.

Consumo assíncrono

Os microsserviços ou lambdas assinantes consomem os eventos da fila de forma assíncrona.

Isso garante:

Que o serviço produtor não precisa esperar o processamento do consumidor.

Que a comunicação continue mesmo que um consumidor esteja temporariamente offline.

Retry e Dead-Letter (opcional)

Se algum evento falhar no processamento, ele pode ser enviado para uma Dead-Letter Queue para reprocessamento posterior.

Monitoramento do fluxo

Jaeger captura traces distribuídos, permitindo visualizar o caminho completo do evento entre microsserviços.

Prometheus + Grafana monitoram métricas como taxa de consumo das filas, latência de eventos e carga dos pods.

