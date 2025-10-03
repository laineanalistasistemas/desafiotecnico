# Ecommerce Microservices (.NET Core)

Scaffold with two microservices: InventoryService and SalesService, plus an Ocelot API Gateway and RabbitMQ for events.

## Tecnologias
- .NET 7 (ou .NET 6)
- C#
- Entity Framework Core
- RabbitMQ
- JWT (Auth)
- PostgreSQL (exemplo)
- Ocelot API Gateway
- Docker / Docker Compose

## Como usar (rápido)
1. Ajuste `appsettings.json` em cada serviço com connection strings e RabbitMQ URI.
2. `docker-compose up --build`
3. Execute migrations (`dotnet ef database update`) nos serviços ou use scripts de inicialização.
4. Teste via Gateway (ex.: `http://localhost:5000/api/orders`).

Arquivos gerados automaticamente por um scaffold. Ajuste e complete conforme necessidade.
