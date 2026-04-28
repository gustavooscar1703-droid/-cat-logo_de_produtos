🦀 Sistema de Busca Otimizado para Catálogo de Produtos - MegaStore API
API REST para gerenciamento de produtos construídos com Rust, utilizando:

Axum (framework web)
SQLx (acesso ao banco)
PostgreSQL (persistência)
Redis (cache)
Arquitetura em camadas: Repositório + UseCase
📦 Funcionalidades
Criar produto
Produtos Listar
Buscar produto por ID
Buscar produtos por filtros (nome, marca, categoria)
Cache com Redis para otimização de leitura
🧠 Arquitetura
src/
  domain/
    entity/
      product.rs

  application/
    dto/
      create_product_request.rs
    usecase/
      product_catalog_use_case.rs

  infrastructure/
    database/
      product_repository.rs
    cache/
      redis_cache.rs
      product_cache.rs

  main.rs
Responsabilidades
Domínio → entidades puras
Repositório → acesso ao Postgres
UseCase → regras de negócio + cache
Cache (Redis) → leitura otimizada
🚀 Como executar o projeto
1. Pré-requisitos
Docker
Docker Compose
Rust (opcional, se quiser rodar local sem container)
2. Configurar.env
Crie um arquivo .envna raiz:

APP_HOST=0.0.0.0
APP_PORT=3000

POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=product_catalog

DATABASE_URL=postgres://postgres:postgres@postgres:5432/product_catalog
REDIS_URL=redis://redis:6379/
3. Subir tudo com Docker
docker compose up --build
Isso irá subir:

API → http://localhost:3000
Postgres → porta 5432
Redis → porta 6379
4. Verificar se está rodando
curl http://localhost:3000/health
Resposta:

ok
📡 Pontos finais
🔹 Criar produto
POST /products
Corpo:
{
  "name": "iPhone 15",
  "brand": "Apple",
  "category": "Smartphone",
  "price_cents": 500000
}
🔹 Listar produtos
GET /products
🔹 Buscar por ID
GET /products/{id}
🔹 Busca personalizada
GET /products/search?name=iphone&brand=apple&category=smartphone
Parâmetros:
Parâmetro	Tipo	Descrição
nome	corda	busca parcial por nome
marca	corda	busca parcial por marca
categoria	corda	busca parcial por categoria
⚡ Cache com Redis
Essencial
GET /products→ cache:products:all
GET /products/{id}→ cache:products:{id}
Invalidação
POST /products→ :

products:all
products:{id}
TTL
60 segundos
🧪 Rodando testes
cargo test
Saída COM:

cargo test -- --nocapture
🧱 Banco de dados
Tabela esperada:

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    brand TEXT NOT NULL,
    category TEXT NOT NULL,
    price_cents BIGINT NOT NULL
);
🐳 Docker
Serviços
aplicativo → API Rust
postgres → banco
redis → cache
Comunicação interna
Dentro dos contêineres:

Postgres →postgres:5432
Redis →redis:6379
⚠️Observações importantes
❌ Não usar localhost dentro do container
Use sempre:

postgres
redis
⚠️Cache não é crítico
Se o Redis cair:

A API continua funcionando
apenas desempenho perdedor# -cat-logo_de_produtos
