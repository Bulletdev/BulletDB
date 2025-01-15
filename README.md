# BulletDB

BulletDB é um sistema de banco de dados híbrido que combina as melhores características de bancos relacionais e não-relacionais, oferecendo alto desempenho, flexibilidade e consistência.

## Características Principais

- Suporte híbrido para modelos relacionais e documentais
- Conformidade ACID para transações
- Pipelines de agregação otimizados
- Compilação JIT (Just-In-Time) de expressões
- Particionamento automático de dados
- Paralelização de consultas
- Sistema robusto de monitoramento

## Requisitos

- Java 17 ou superior
- Maven 3.8+
- Mínimo 4GB RAM
- 20GB espaço em disco

## Instalação

```bash
git clone https://github.com/your-username/bulletdb.git
cd bulletdb
mvn clean install
```

## Configuração

1. Copie o arquivo `config/bulletdb.properties.example` para `config/bulletdb.properties`
2. Configure os parâmetros conforme necessário:
```properties
bulletdb.storage.path=/path/to/data
bulletdb.port=9090
bulletdb.max_connections=100
bulletdb.memory_limit=4G
```

## Uso

### Iniciar o Servidor

```bash
./bin/bulletdb-server start
```

### Conexão via CLI

```bash
./bin/bulletdb-cli --host localhost --port 9090
```

### Exemplo de Uso

```java
// Conectar ao banco
BulletDB db = BulletDB.connect("localhost:9090");

// Criar tabela relacional
db.createTable("users")
  .addColumn("id", Type.LONG)
  .addColumn("name", Type.STRING)
  .addColumn("age", Type.INTEGER)
  .execute();

// Inserir documento
db.collection("profiles")
  .insert({
    "user_id": 1,
    "preferences": {
      "theme": "dark",
      "notifications": true
    }
  });

// Query híbrida
db.query()
  .join("users", "profiles")
  .on("users.id = profiles.user_id")
  .where("age > 18")
  .select("name, preferences")
  .execute();
```

## Features Avançadas

### Particionamento

```java
// Particionamento automático por range
db.table("events")
  .withPartitioning()
  .byRange("timestamp")
  .withIntervalDays(30)
  .create();
```

### Pipeline de Agregação

```java
db.collection("sales")
  .aggregate()
  .match({"status": "completed"})
  .group({
    _id: "$region",
    total: { $sum: "$amount" }
  })
  .sort({ total: -1 })
  .execute();
```

### Monitoramento

```java
// Obter métricas
MonitoringReport report = db.monitoring()
  .withMetrics("cpu", "memory", "iops")
  .forPeriod(Duration.ofHours(24))
  .generate();
```

## Performance

- Throughput: 100K+ transações/segundo
- Latência: < 10ms para 99% das operações
- Suporte para datasets de até 100TB
- Escalabilidade horizontal automática

## Contribuindo

1. Fork o repositório
2. Crie sua branch (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## Licença

Este projeto está licenciado sob a Apache License 2.0 - veja o arquivo [LICENSE](LICENSE) para detalhes.

## Suporte

- Issues: [GitHub Issues](https://github.com/bulletdev/bulletdb/issues)
