# AlgaShop - Microservices E-commerce

> **⚠️ Projeto em Desenvolvimento**
> Este projeto está atualmente em desenvolvimento ativo. Algumas funcionalidades podem estar incompletas ou sujeitas a alterações.

Sistema de e-commerce baseado em microserviços desenvolvido com Spring Boot, aplicando conceitos de Domain-Driven Design (DDD), Event Sourcing e CQRS.

## 📋 Visão Geral

O AlgaShop é uma aplicação de comércio eletrônico distribuída que demonstra boas práticas de arquitetura de microserviços, incluindo comunicação assíncrona por eventos, separação de contextos delimitados e padrões táticos de DDD.

## 🏗️ Arquitetura

O projeto é organizado como um monorepo com 3 microserviços independentes:

### Microserviços

#### 1. **Ordering Service** (Pedidos)
Responsável pelo gerenciamento completo do ciclo de vida de pedidos:
- Gerenciamento de clientes e programa de fidelidade
- Carrinho de compras
- Checkout e processamento de pedidos
- Integração com serviço de cálculo de frete (RapiDex)
- Estados do pedido: Draft → Placed → Paid → Ready → Canceled

**Porta**: Configurável
**Banco de dados**: H2 (file-based em `~/ordering`)

#### 2. **Billing Service** (Faturamento)
Gerencia toda a parte financeira e faturamento:
- Emissão de faturas
- Processamento de pagamentos via gateway
- Gerenciamento de cartões de crédito
- Estados da fatura: Issued → Paid → Canceled

**Porta**: 8082
**Banco de dados**: H2 (file-based em `~/billing`)

#### 3. **Product Catalog Service** (Catálogo de Produtos)
Serviço de catálogo de produtos (em desenvolvimento):
- Gerenciamento de produtos
- Categorias e preços
- API REST para consulta e cadastro

**Porta**: Configurável
**Status**: Em desenvolvimento inicial

## 🛠️ Tecnologias

- **Java 21**
- **Spring Boot 3.5.x**
- **Spring Cloud 2025.0.0**
- **Spring Data JPA**
- **Hibernate**
- **H2 Database**
- **Gradle 8.14.2**
- **Lombok**
- **ModelMapper**
- **Docker & Docker Compose**

### Testes
- JUnit 5
- Mockito
- AssertJ
- REST Assured
- Spring Cloud Contract (testes de contrato)

## 🎯 Padrões e Práticas

### Domain-Driven Design (DDD)
- **Aggregates**: Order, Invoice, Customer, ShoppingCart
- **Value Objects**: Money, Address, Email, Phone, Quantity
- **Domain Events**: OrderPlaced, OrderPaid, InvoiceIssued, InvoicePaid
- **Domain Services**: ShippingCostService, CustomerLoyaltyPointsService
- **Repositories**: Abstração de persistência
- **Bounded Contexts**: Cada microserviço é um contexto delimitado

### Arquitetura
- **Event Sourcing**: Rastreamento de eventos no Ordering Service
- **CQRS**: Separação de comandos e consultas
- **Event-Driven**: Comunicação assíncrona entre serviços
- **Auditing**: Rastreamento de alterações com Spring Data Auditing

## 🚀 Como Executar

### Pré-requisitos
- Java 21
- Docker e Docker Compose

### 1. Iniciar serviços auxiliares
```bash
docker-compose up -d
```
Isso iniciará o Wiremock na porta 8780 para simular APIs externas.

### 2. Executar os microserviços

**Ordering Service:**
```bash
cd microservices/ordering
./gradlew bootRun
```

**Billing Service:**
```bash
cd microservices/billing
./gradlew bootRun
```

**Product Catalog Service:**
```bash
cd microservices/product-catalog
./gradlew bootRun
```

### 3. Executar testes
```bash
./gradlew test
```

## 📁 Estrutura do Projeto

```
algashop-meta/
├── microservices/
│   ├── ordering/           # Serviço de pedidos
│   ├── billing/            # Serviço de faturamento
│   └── product-catalog/    # Catálogo de produtos
├── docs/                   # Documentação (submodule)
│   ├── openapi/           # Especificações OpenAPI
│   └── domain-model/      # Diagramas de domínio
├── etc/
│   ├── stub-runner/       # Stubs para testes de contrato
│   └── wiremock/          # Configurações do Wiremock
└── docker-compose.yml     # Orquestração de serviços
```

Cada microserviço segue a estrutura:
```
src/main/java/
└── com/algaworks/algashop/{service}/
    ├── application/        # Casos de uso
    ├── domain/model/       # Modelo de domínio
    ├── infrastructure/     # Adaptadores (persistência, clientes)
    └── presentation/       # Controladores REST
```

## 🔗 Integrações

- **RapiDex API**: Cálculo de frete (mockado via Wiremock)
- **Payment Gateway**: Processamento de pagamentos (mockado)

## 📊 Banco de Dados

Cada serviço possui seu próprio banco de dados H2:
- **Ordering**: `~/ordering`
- **Billing**: `~/billing`

Consoles H2 disponíveis em:
- Ordering: `http://localhost:{porta}/h2-console`
- Billing: `http://localhost:8082/h2-console`

## 🧪 Testes de Contrato

O projeto utiliza Spring Cloud Contract para testes de contrato entre consumidores e provedores:
```bash
cd microservices/product-catalog
./gradlew test
```

## 📝 Documentação

A documentação adicional está disponível no submódulo `docs/`:
- Especificações OpenAPI das APIs
- Diagramas de modelo de domínio (StarUML)

## 🎓 Propósito Educacional

Este projeto foi desenvolvido como exemplo educacional para demonstrar:
- Arquitetura de microserviços moderna
- Implementação prática de DDD
- Event Sourcing e CQRS
- Comunicação assíncrona entre serviços
- Testes de contrato e integração
- Separação de responsabilidades

## 📄 Licença

Este é um projeto educacional desenvolvido pela AlgaWorks.
