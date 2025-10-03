# 🛒 E-commerce Microservices com .NET Core

Este projeto é um **sistema de e-commerce** desenvolvido com **arquitetura de microserviços**, voltado para o gerenciamento de **estoque** e **vendas**.
Utiliza **.NET Core, C#, RabbitMQ, API Gateway (Ocelot), JWT para autenticação** e **PostgreSQL** como banco relacional.

> 📌 Projeto desenvolvido como **desafio técnico da DIO com a Avanade**, implementado por **Regilaine**.

---

## 🚀 Tecnologias Utilizadas

* **.NET 7 / .NET 6**
* **C#**
* **Entity Framework Core**
* **RabbitMQ** (mensageria entre serviços)
* **Ocelot API Gateway**
* **JWT** (JSON Web Token para autenticação)
* **PostgreSQL**
* **Docker & Docker Compose**

---

## 📌 Arquitetura

O sistema é composto por três principais serviços:

* **InventoryService**: gerenciamento de produtos e estoque
* **SalesService**: gerenciamento de pedidos e vendas
* **API Gateway (Ocelot)**: entrada única para os microserviços

Comunicação assíncrona entre os serviços é feita via **RabbitMQ (event-driven)**.

```
[ Cliente ] → [ API Gateway ] → [ SalesService ] ↔ [ InventoryService ]
                            ↘ RabbitMQ (eventos)
```

---

## 📂 Estrutura do Projeto

```
/ecommerce-microservices
├── gateway/              → API Gateway (Ocelot)
├── src/
│   ├── Common/           → DTOs, helpers de mensageria, autenticação
│   ├── InventoryService/ → Microserviço de estoque
│   └── SalesService/     → Microserviço de vendas
├── docker-compose.yml    → Orquestração com Docker
├── README.md             → Documentação do projeto
└── .gitignore
```

---

## 🔄 Fluxo Básico

1. O cliente cria um pedido via `SalesService` (via Gateway).
2. O `SalesService` publica o evento **OrderCreated** no RabbitMQ.
3. O `InventoryService` consome o evento, verifica estoque e responde com **ProductReserved** ou **ProductReservationFailed**.
4. O `SalesService` atualiza o status do pedido para **Confirmado** ou **Cancelado**.

---

## ⚡ Como Rodar o Projeto

### Pré-requisitos

* [.NET 7 SDK](https://dotnet.microsoft.com/download)
* [Docker & Docker Compose](https://docs.docker.com/get-docker/)

### Passos

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/ecommerce-microservices-dotnet.git
cd ecommerce-microservices-dotnet

# 2. Suba os containers
docker-compose up --build

# 3. Execute migrations (em cada serviço)
cd src/InventoryService
dotnet ef database update

cd ../SalesService
dotnet ef database update
```

### Acessos

* API Gateway: `http://localhost:5000`
* RabbitMQ Management: `http://localhost:15672` (user: guest / pass: guest)
* PostgreSQL: `localhost:5432`

---

## 🔑 Autenticação JWT

* Autenticação via **Bearer Token**
* Ocelot Gateway valida os tokens antes de redirecionar para os microserviços.
* Tokens devem ser enviados no **header Authorization**:

```http
Authorization: Bearer {seu_token_jwt}
```

---

## 📚 Exemplos de Endpoints

### InventoryService

* `GET /api/products` → lista produtos
* `POST /api/products` → cria novo produto
* `PUT /api/products/{id}/stock` → altera estoque

### SalesService

* `POST /api/orders` → cria pedido
* `GET /api/orders/{id}` → status do pedido

---

## 🛠️ Próximos Passos

* Implementar **Outbox Pattern** para garantir consistência dos eventos
* Adicionar **observabilidade** (logs estruturados, métricas e tracing)
* Criar **testes automatizados** (unitários e de integração)
* Expandir para mais microserviços (pagamentos, usuários, catálogo etc.)

---

## 👨‍💻 Contribuição

Contribuições são bem-vindas!
Para contribuir:

1. Faça um fork do repositório
2. Crie uma branch (`git checkout -b feature/nova-feature`)
3. Commit suas alterações (`git commit -m 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request 🚀

---

## 📜 Licença

Este projeto é distribuído sob a licença **MIT**.
Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
