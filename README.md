# 🛢️ CrewAI Fuel OP - Sistema de Monitoramento Inteligente

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18+-blue.svg)](https://reactjs.org)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/Tests-Pytest-red.svg)](https://pytest.org)

## 📋 Visão Geral

O **CrewAI Fuel OP** é um sistema avançado de monitoramento inteligente para operações de combustível, desenvolvido com tecnologias modernas e práticas de desenvolvimento profissional. O sistema oferece monitoramento em tempo real, detecção de anomalias com IA, previsão de demanda e dashboards interativos.

### ✨ Principais Funcionalidades

- 🤖 **Detecção de Anomalias com IA**: Machine learning para identificar padrões anômalos
- 📊 **Dashboard Interativo**: Interface moderna com React e visualizações avançadas
- 🔐 **Autenticação Robusta**: Sistema de autenticação JWT com middleware de segurança
- 📡 **Integração MQTT**: Comunicação em tempo real com sensores IoT
- 🔗 **Webhooks**: Recebimento de dados de sistemas externos
- 📈 **Previsão de Demanda**: Modelos preditivos para planejamento
- 🧪 **Testes Abrangentes**: Cobertura de testes > 80%
- 🚀 **Pronto para Produção**: Configurações de deployment e monitoramento

## 🏗️ Arquitetura do Sistema

```
CrewAI_Fuel_OP/
├── app/                    # Aplicação principal
│   ├── api/               # Endpoints da API
│   ├── auth/              # Sistema de autenticação
│   ├── core/              # Configurações e banco de dados
│   ├── integration/       # MQTT, webhooks, APIs externas
│   ├── middleware/        # Middleware de segurança
│   └── ml/                # Modelos de machine learning
├── dashboard/             # Dashboard Streamlit
├── fuel-op-dashboard/     # Dashboard React moderno
├── tests/                 # Testes unitários e integração
├── docs/                  # Documentação técnica
└── deployment/            # Configurações de produção
```

## 🚀 Início Rápido

### Pré-requisitos

- Python 3.11+
- Node.js 18+
- PostgreSQL 13+ (ou SQLite para desenvolvimento)
- Redis (opcional, para cache)

### 1. Clonagem e Configuração

```bash
# Clonar o repositório
git clone <repository-url>
cd CrewAI_Fuel_OP

# Criar ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows

# Instalar dependências
pip install -r requirements.txt
```

### 2. Configuração do Ambiente

```bash
# Copiar arquivo de configuração
cp .env.example .env

# Editar variáveis de ambiente
nano .env
```

**Variáveis de Ambiente Essenciais:**

```env
# Banco de Dados
DATABASE_URL=postgresql://user:password@localhost/fuel_op_db
# ou para desenvolvimento:
DATABASE_URL=sqlite:///./fuel_op.db

# Segurança
SECRET_KEY=your-super-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# MQTT (opcional)
MQTT_BROKER_URL=localhost
MQTT_PORT=1883
MQTT_USERNAME=
MQTT_PASSWORD=

# APIs Externas (opcional)
EXTERNAL_API_KEY=your-api-key
NOTIFICATION_SERVICE_URL=https://api.notification-service.com
```

### 3. Configuração do Banco de Dados

```bash
# Executar migrações
alembic upgrade head

# Ou criar tabelas diretamente (desenvolvimento)
python -c "from app.core.database import create_tables; create_tables()"
```

### 4. Executar a Aplicação

#### Backend (FastAPI)

```bash
# Desenvolvimento
uvicorn app.api.main:app --reload --host 0.0.0.0 --port 8000

# Produção
uvicorn app.api.main:app --host 0.0.0.0 --port 8000 --workers 4
```

#### Dashboard Streamlit

```bash
# Executar dashboard Streamlit
streamlit run dashboard/enhanced_dashboard.py --server.port 8501
```

#### Dashboard React (Moderno)

```bash
# Navegar para o diretório React
cd fuel-op-dashboard

# Instalar dependências (primeira vez)
pnpm install

# Executar em desenvolvimento
pnpm run dev

# Build para produção
pnpm run build
```

### 5. Verificar Instalação

Acesse os seguintes URLs:

- **API**: http://localhost:8000
- **Documentação da API**: http://localhost:8000/docs
- **Dashboard Streamlit**: http://localhost:8501
- **Dashboard React**: http://localhost:3000 (desenvolvimento)

## 📖 Documentação Detalhada

### 🔧 Configuração Avançada

#### Banco de Dados PostgreSQL

```bash
# Instalar PostgreSQL
sudo apt-get install postgresql postgresql-contrib

# Criar banco de dados
sudo -u postgres psql
CREATE DATABASE fuel_op_db;
CREATE USER fuel_op_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE fuel_op_db TO fuel_op_user;
\q
```

#### Configuração MQTT

```bash
# Instalar Mosquitto (broker MQTT)
sudo apt-get install mosquitto mosquitto-clients

# Iniciar serviço
sudo systemctl start mosquitto
sudo systemctl enable mosquitto

# Testar conexão
mosquitto_pub -h localhost -t fuel_op/test -m "Hello MQTT"
```

#### Redis (Cache)

```bash
# Instalar Redis
sudo apt-get install redis-server

# Configurar no .env
REDIS_URL=redis://localhost:6379/0
```

### 🧪 Executar Testes

```bash
# Executar todos os testes
pytest

# Testes com cobertura
pytest --cov=app --cov-report=html

# Testes específicos
pytest tests/test_auth.py -v
pytest -m "unit" -v
pytest -m "integration" -v
pytest -m "api" -v

# Testes paralelos (mais rápido)
pytest -n auto
```

### 📊 Monitoramento e Logs

#### Configuração de Logs

```python
# Configuração em app/core/config.py
import logging
import structlog

logging.basicConfig(
    format="%(message)s",
    stream=sys.stdout,
    level=logging.INFO,
)

structlog.configure(
    processors=[
        structlog.stdlib.filter_by_level,
        structlog.stdlib.add_logger_name,
        structlog.stdlib.add_log_level,
        structlog.stdlib.PositionalArgumentsFormatter(),
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.StackInfoRenderer(),
        structlog.processors.format_exc_info,
        structlog.processors.UnicodeDecoder(),
        structlog.processors.JSONRenderer()
    ],
    context_class=dict,
    logger_factory=structlog.stdlib.LoggerFactory(),
    wrapper_class=structlog.stdlib.BoundLogger,
    cache_logger_on_first_use=True,
)
```

#### Métricas e Health Checks

```bash
# Health check da API
curl http://localhost:8000/health

# Métricas detalhadas
curl http://localhost:8000/health/detailed
```

## 🔌 Integrações

### MQTT - Recebimento de Dados de Sensores

```python
# Exemplo de publicação MQTT
import paho.mqtt.client as mqtt
import json

client = mqtt.Client()
client.connect("localhost", 1883, 60)

# Dados do sensor
sensor_data = {
    "station_id": "STATION_001",
    "sensor_type": "fuel_level",
    "value": 75.5,
    "unit": "percent",
    "quality_score": 0.95,
    "timestamp": "2024-01-01T12:00:00Z"
}

client.publish("fuel_op/sensors/STATION_001/data", json.dumps(sensor_data))
```

### Webhooks - Integração com Sistemas Externos

```bash
# Exemplo de webhook
curl -X POST http://localhost:8000/webhooks/fuel_supplier \
  -H "Content-Type: application/json" \
  -H "X-Hub-Signature-256: sha256=your_signature" \
  -d '{
    "event_type": "sensor_data",
    "data": {
      "sensors": [
        {
          "station_id": "STATION_001",
          "type": "fuel_level",
          "value": 67.8,
          "unit": "percent",
          "quality": 0.94
        }
      ]
    }
  }'
```

### API REST - Inserção Manual de Dados

```bash
# Autenticação
curl -X POST http://localhost:8000/auth/login \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=admin&password=admin123"

# Inserir dados de sensor
curl -X POST http://localhost:8000/sensor-data \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "station_id": "STATION_001",
    "sensor_type": "fuel_level",
    "value": 75.5,
    "unit": "percent",
    "quality_score": 0.95
  }'
```

## 🤖 Machine Learning

### Detecção de Anomalias

O sistema utiliza algoritmos de machine learning para detectar anomalias em tempo real:

```python
# Exemplo de uso da API de detecção
import requests

response = requests.post(
    "http://localhost:8000/ml/detect-anomaly",
    json={
        "station_id": "STATION_001",
        "sensor_type": "fuel_level",
        "value": 15.0,  # Valor suspeito
        "quality_score": 0.95
    },
    headers={"Authorization": "Bearer YOUR_TOKEN"}
)

result = response.json()
print(f"Anomalia detectada: {result['is_anomaly']}")
print(f"Score: {result['anomaly_score']}")
```

### Previsão de Demanda

```python
# Previsão de consumo
response = requests.post(
    "http://localhost:8000/ml/predict/consumption",
    json={
        "station_id": "STATION_001",
        "hours_ahead": 24
    },
    headers={"Authorization": "Bearer YOUR_TOKEN"}
)

prediction = response.json()
print(f"Consumo previsto: {prediction['total_predicted_consumption']} L")
```

### Retreinamento de Modelos

```bash
# Retreinar modelo de anomalias
curl -X POST http://localhost:8000/ml/retrain/anomaly-model \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"contamination": 0.1}'

# Retreinar modelo de demanda
curl -X POST http://localhost:8000/ml/retrain/demand-model \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## 🚀 Deploy em Produção

### Docker

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "app.api.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/fuel_op
    depends_on:
      - db
      - redis
  
  db:
    image: postgres:13
    environment:
      POSTGRES_DB: fuel_op
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:7-alpine
    
  dashboard:
    build:
      context: .
      dockerfile: Dockerfile.streamlit
    ports:
      - "8501:8501"
    depends_on:
      - api

volumes:
  postgres_data:
```

### Nginx (Proxy Reverso)

```nginx
# /etc/nginx/sites-available/fuel-op
server {
    listen 80;
    server_name your-domain.com;

    location /api/ {
        proxy_pass http://localhost:8000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /dashboard/ {
        proxy_pass http://localhost:8501/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Systemd (Serviços)

```ini
# /etc/systemd/system/fuel-op-api.service
[Unit]
Description=CrewAI Fuel OP API
After=network.target

[Service]
Type=exec
User=fuel-op
Group=fuel-op
WorkingDirectory=/opt/fuel-op
Environment=PATH=/opt/fuel-op/venv/bin
ExecStart=/opt/fuel-op/venv/bin/uvicorn app.api.main:app --host 0.0.0.0 --port 8000
Restart=always

[Install]
WantedBy=multi-user.target
```

## 🔒 Segurança

### Autenticação e Autorização

- **JWT Tokens**: Autenticação baseada em tokens
- **Middleware de Segurança**: Rate limiting, CORS, validação
- **Hash de Senhas**: bcrypt para armazenamento seguro
- **Verificação de Assinaturas**: Webhooks com HMAC

### Configurações de Segurança

```python
# Configurações recomendadas para produção
SECURITY_SETTINGS = {
    "SECRET_KEY": "use-a-strong-random-key-here",
    "ALGORITHM": "HS256",
    "ACCESS_TOKEN_EXPIRE_MINUTES": 30,
    "RATE_LIMIT_PER_MINUTE": 100,
    "CORS_ORIGINS": ["https://your-domain.com"],
    "HTTPS_ONLY": True
}
```

## 📈 Performance e Otimização

### Configurações Recomendadas

```python
# Configurações de produção
PRODUCTION_SETTINGS = {
    "workers": 4,  # Número de workers Uvicorn
    "max_connections": 1000,
    "keepalive_timeout": 5,
    "database_pool_size": 20,
    "redis_pool_size": 10
}
```

### Monitoramento

```bash
# Instalar ferramentas de monitoramento
pip install prometheus-client grafana-api

# Métricas customizadas
from prometheus_client import Counter, Histogram

REQUEST_COUNT = Counter('requests_total', 'Total requests')
REQUEST_LATENCY = Histogram('request_duration_seconds', 'Request latency')
```

## 🐛 Troubleshooting

### Problemas Comuns

#### 1. Erro de Conexão com Banco de Dados

```bash
# Verificar conexão
python -c "from app.core.database import engine; print(engine.connect())"

# Verificar variáveis de ambiente
echo $DATABASE_URL
```

#### 2. Problemas com MQTT

```bash
# Testar conexão MQTT
mosquitto_sub -h localhost -t fuel_op/sensors/+/data

# Verificar logs
tail -f /var/log/mosquitto/mosquitto.log
```

#### 3. Erros de Importação

```bash
# Verificar instalação de dependências
pip check

# Reinstalar dependências
pip install -r requirements.txt --force-reinstall
```

#### 4. Problemas de Performance

```bash
# Monitorar recursos
htop
iotop

# Verificar logs de performance
tail -f logs/performance.log
```

## 🤝 Contribuição

### Configuração do Ambiente de Desenvolvimento

```bash
# Instalar dependências de desenvolvimento
pip install -r requirements-dev.txt

# Configurar pre-commit hooks
pre-commit install

# Executar linting
black app/ tests/
isort app/ tests/
flake8 app/ tests/

# Executar type checking
mypy app/
```

### Padrões de Código

- **Python**: PEP 8, Black formatter
- **JavaScript/React**: ESLint, Prettier
- **Commits**: Conventional Commits
- **Branches**: GitFlow

### Executar Testes Antes do Commit

```bash
# Testes rápidos
pytest -m "not slow"

# Testes completos
pytest --cov=app --cov-fail-under=80
```

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 📞 Suporte

Para suporte técnico ou dúvidas:

- **Email**: support@fuel-op.com
- **Documentação**: [docs.fuel-op.com](https://docs.fuel-op.com)
- **Issues**: [GitHub Issues](https://github.com/your-org/fuel-op/issues)

## 🙏 Agradecimentos

- **FastAPI**: Framework web moderno e rápido
- **React**: Biblioteca para interfaces de usuário
- **Streamlit**: Ferramenta para dashboards interativos
- **scikit-learn**: Biblioteca de machine learning
- **CrewAI**: Framework para agentes inteligentes

---

**Desenvolvido com ❤️ pela equipe CrewAI Fuel OP**

