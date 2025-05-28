# DevOps - Generale

Guida completa alle pratiche DevOps, containerizzazione, CI/CD e deployment per progetti PANDEV.

## 🐳 Containerizzazione

### Docker Basics

**Dockerfile per Rails API:**
```dockerfile
# Dockerfile
FROM ruby:3.1.0-alpine

# Installa dipendenze sistema
RUN apk add --no-cache \
    build-base \
    postgresql-dev \
    nodejs \
    yarn \
    git \
    tzdata

# Crea directory app
WORKDIR /app

# Copia Gemfile
COPY Gemfile Gemfile.lock ./

# Installa gems
RUN bundle config --global frozen 1 && \
    bundle install

# Copia codice applicazione
COPY . .

# Esponi porta
EXPOSE 3000

# Comando default
CMD ["rails", "server", "-b", "0.0.0.0"]
```

**Docker Compose per Development:**
```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - bundle_cache:/usr/local/bundle
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/myapp_development
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      - db
      - redis
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -b 0.0.0.0"

  db:
    image: postgres:14-alpine
    environment:
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=myapp_development
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  sidekiq:
    build: .
    volumes:
      - .:/app
      - bundle_cache:/usr/local/bundle
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/myapp_development
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      - db
      - redis
    command: bundle exec sidekiq

volumes:
  postgres_data:
  redis_data:
  bundle_cache:
```

### Multi-stage Build per Production

```dockerfile
# Dockerfile.production
FROM ruby:3.1.0-alpine AS builder

# Installa dipendenze build
RUN apk add --no-cache \
    build-base \
    postgresql-dev \
    git

WORKDIR /app

# Installa gems
COPY Gemfile Gemfile.lock ./
RUN bundle config --global frozen 1 && \
    bundle config set --local deployment 'true' && \
    bundle config set --local without 'development test' && \
    bundle install

# Stage finale
FROM ruby:3.1.0-alpine

# Installa solo runtime dependencies
RUN apk add --no-cache \
    postgresql-client \
    tzdata

WORKDIR /app

# Copia gems dal builder
COPY --from=builder /usr/local/bundle /usr/local/bundle

# Copia applicazione
COPY . .

# Crea utente non-root
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

USER appuser

EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]
```

## 🔄 CI/CD

### GitHub Actions per Rails

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
    - uses: actions/checkout@v4

    - name: Set up Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 3.1.0
        bundler-cache: true

    - name: Setup Database
      env:
        RAILS_ENV: test
        DATABASE_URL: postgres://postgres:postgres@localhost:5432/myapp_test
      run: |
        bundle exec rails db:create
        bundle exec rails db:schema:load

    - name: Run RSpec
      env:
        RAILS_ENV: test
        DATABASE_URL: postgres://postgres:postgres@localhost:5432/myapp_test
        REDIS_URL: redis://localhost:6379/0
      run: bundle exec rspec

    - name: Run RuboCop
      run: bundle exec rubocop

    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage/coverage.xml
        fail_ci_if_error: true
```

### Deploy Workflow

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: eu-west-1

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: Build and push Docker image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        ECR_REPOSITORY: myapp
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -f Dockerfile.production -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        docker tag $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG $ECR_REGISTRY/$ECR_REPOSITORY:latest
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:latest

    - name: Deploy to ECS
      run: |
        aws ecs update-service --cluster production --service myapp --force-new-deployment
```

## 🌍 Infrastructure as Code

### Terraform per AWS

**Provider Configuration:**
```hcl
# terraform/main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "myapp-terraform-state"
    key    = "production/terraform.tfstate"
    region = "eu-west-1"
  }
}

provider "aws" {
  region = var.aws_region
}
```

**VPC e Networking:**
```hcl
# terraform/network.tf
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "${var.app_name}-vpc"
  }
}

resource "aws_subnet" "public" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 1}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]

  map_public_ip_on_launch = true

  tags = {
    Name = "${var.app_name}-public-${count.index + 1}"
  }
}

resource "aws_subnet" "private" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 10}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]

  tags = {
    Name = "${var.app_name}-private-${count.index + 1}"
  }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "${var.app_name}-igw"
  }
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }

  tags = {
    Name = "${var.app_name}-public-rt"
  }
}

resource "aws_route_table_association" "public" {
  count          = length(aws_subnet.public)
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}
```

**ECS Service:**
```hcl
# terraform/ecs.tf
resource "aws_ecs_cluster" "main" {
  name = var.app_name

  setting {
    name  = "containerInsights"
    value = "enabled"
  }
}

resource "aws_ecs_task_definition" "app" {
  family                   = var.app_name
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = 512
  memory                   = 1024
  execution_role_arn       = aws_iam_role.ecs_execution_role.arn
  task_role_arn           = aws_iam_role.ecs_task_role.arn

  container_definitions = jsonencode([
    {
      name  = var.app_name
      image = "${aws_ecr_repository.app.repository_url}:latest"

      portMappings = [
        {
          containerPort = 3000
          protocol      = "tcp"
        }
      ]

      environment = [
        {
          name  = "RAILS_ENV"
          value = "production"
        },
        {
          name  = "DATABASE_URL"
          value = "postgresql://${aws_db_instance.postgres.username}:${random_password.db_password.result}@${aws_db_instance.postgres.endpoint}/${aws_db_instance.postgres.db_name}"
        }
      ]

      logConfiguration = {
        logDriver = "awslogs"
        options = {
          awslogs-group         = aws_cloudwatch_log_group.app.name
          awslogs-region        = var.aws_region
          awslogs-stream-prefix = "ecs"
        }
      }
    }
  ])
}

resource "aws_ecs_service" "app" {
  name            = var.app_name
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.app.arn
  desired_count   = 2
  launch_type     = "FARGATE"

  network_configuration {
    security_groups  = [aws_security_group.app.id]
    subnets          = aws_subnet.private[*].id
    assign_public_ip = false
  }

  load_balancer {
    target_group_arn = aws_lb_target_group.app.arn
    container_name   = var.app_name
    container_port   = 3000
  }

  depends_on = [aws_lb_listener.app]
}
```

## 📊 Monitoring e Logging

### CloudWatch Configuration

```yaml
# docker-compose.monitoring.yml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana
      - ./monitoring/grafana/dashboards:/etc/grafana/provisioning/dashboards
      - ./monitoring/grafana/datasources:/etc/grafana/provisioning/datasources

volumes:
  prometheus_data:
  grafana_data:
```

### Rails Application Monitoring

**Gemfile additions:**
```ruby
# Monitoring
gem 'prometheus-client'
gem 'yabeda-rails'
gem 'yabeda-prometheus'
gem 'yabeda-sidekiq'

# Logging
gem 'lograge'
gem 'amazing_print'
```

**Monitoring initializer:**
```ruby
# config/initializers/yabeda.rb
Yabeda.configure do
  group :rails do
    counter :requests_total, tags: [:controller, :action, :status], comment: "Total requests count"
    histogram :request_duration, tags: [:controller, :action], comment: "Request duration"
  end

  group :business do
    counter :user_registrations_total, comment: "User registrations count"
    counter :orders_total, tags: [:status], comment: "Orders count by status"
  end
end

# Rack middleware
Rails.application.config.middleware.use Yabeda::Prometheus::Exporter
```

## 🔒 Security

### Secrets Management

**AWS Secrets Manager:**
```ruby
# config/application.rb
config.secret_key_base = if Rails.env.production?
  Aws::SecretsManager::Client.new(region: 'eu-west-1')
    .get_secret_value(secret_id: 'myapp/secret_key_base')
    .secret_string
else
  ENV['SECRET_KEY_BASE']
end
```

**Environment-specific configurations:**
```yaml
# config/secrets.yml
production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
  database_url: <%= ENV["DATABASE_URL"] %>
  redis_url: <%= ENV["REDIS_URL"] %>
  smtp_password: <%= ENV["SMTP_PASSWORD"] %>
```

### Security Headers

```ruby
# config/application.rb
config.force_ssl = true

# config/initializers/security_headers.rb
Rails.application.config.force_ssl = true

Rails.application.config.ssl_options = {
  hsts: {
    expires: 1.year,
    subdomains: true,
    preload: true
  }
}

# Content Security Policy
Rails.application.configure do
  config.content_security_policy do |policy|
    policy.default_src :self
    policy.font_src    :self, :https, :data
    policy.img_src     :self, :https, :data
    policy.object_src  :none
    policy.script_src  :self, :https
    policy.style_src   :self, :https, :unsafe_inline
  end
end
```

## 📈 Performance Optimization

### Database Optimization

**Connection pooling:**
```yaml
# config/database.yml
production:
  adapter: postgresql
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 25 } %>
  timeout: 5000
  prepared_statements: false
  advisory_locks: false
```

**Redis configuration:**
```ruby
# config/initializers/redis.rb
Redis.current = Redis.new(
  url: ENV['REDIS_URL'],
  timeout: 1,
  reconnect_attempts: 3,
  reconnect_delay: 0.5,
  reconnect_delay_max: 5.0
)
```

### Caching Strategy

```ruby
# config/environments/production.rb
config.cache_store = :redis_cache_store, {
  url: ENV['REDIS_URL'],
  expires_in: 1.hour,
  namespace: 'myapp_cache',
  race_condition_ttl: 10.seconds
}

# Russian Doll Caching example
# app/views/articles/index.html.erb
<% cache ['articles', @articles.maximum(:updated_at)] do %>
  <% @articles.each do |article| %>
    <% cache article do %>
      <%= render article %>
    <% end %>
  <% end %>
<% end %>
```

## 🚀 Deployment Strategies

### Blue-Green Deployment

```bash
#!/bin/bash
# scripts/blue_green_deploy.sh

CLUSTER_NAME="production"
SERVICE_NAME="myapp"
NEW_IMAGE="$1"

if [ -z "$NEW_IMAGE" ]; then
  echo "Usage: $0 <new_image_uri>"
  exit 1
fi

# Get current task definition
CURRENT_TASK_DEF=$(aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME --query 'services[0].taskDefinition' --output text)

# Create new task definition with new image
NEW_TASK_DEF=$(aws ecs describe-task-definition --task-definition $CURRENT_TASK_DEF --query 'taskDefinition' --output json | \
  jq --arg IMAGE "$NEW_IMAGE" '.containerDefinitions[0].image = $IMAGE' | \
  jq 'del(.taskDefinitionArn, .revision, .status, .requiresAttributes, .placementConstraints, .compatibilities, .registeredAt, .registeredBy)')

# Register new task definition
NEW_TASK_DEF_ARN=$(echo $NEW_TASK_DEF | aws ecs register-task-definition --cli-input-json file:///dev/stdin --query 'taskDefinition.taskDefinitionArn' --output text)

# Update service
aws ecs update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME --task-definition $NEW_TASK_DEF_ARN

# Wait for deployment to complete
aws ecs wait services-stable --cluster $CLUSTER_NAME --services $SERVICE_NAME

echo "Deployment completed successfully!"
```

### Zero-downtime migrations

```ruby
# Strong migrations setup
# Gemfile
gem 'strong_migrations'

# config/initializers/strong_migrations.rb
StrongMigrations.configure do |config|
  config.auto_analyze = :auto
  config.lock_timeout = 10.seconds
  config.statement_timeout = 1.hour
end

# Example safe migration
class AddIndexSafely < ActiveRecord::Migration[7.0]
  disable_ddl_transaction!

  def change
    add_index :users, :email, algorithm: :concurrently
  end
end
```

## 📋 Best Practices

### 12-Factor App Compliance

1. **Codebase**: Una codebase tracciata in version control
2. **Dependencies**: Dichiarare esplicitamente le dipendenze
3. **Config**: Configurazione nell'ambiente
4. **Backing services**: Trattare i backing services come risorse attached
5. **Build, release, run**: Separare rigorosamente i stage
6. **Processes**: Eseguire l'app come processi stateless
7. **Port binding**: Esportare servizi via port binding
8. **Concurrency**: Scalare tramite il process model
9. **Disposability**: Massimizzare robustezza con fast startup e graceful shutdown
10. **Dev/prod parity**: Mantenere ambienti simili
11. **Logs**: Trattare i log come event streams
12. **Admin processes**: Eseguire admin/management tasks come one-off processes

### Environment Configuration

```ruby
# config/application.rb
module MyApp
  class Application < Rails::Application
    # Environment-based configuration
    config.load_defaults 7.0

    # Force all access to the app over SSL
    config.force_ssl = Rails.env.production?

    # Configuration for the application
    config.time_zone = 'Rome'
    config.i18n.default_locale = :it

    # Custom configuration
    config.x.external_api_url = ENV.fetch('EXTERNAL_API_URL', 'http://localhost:4000')
    config.x.max_upload_size = ENV.fetch('MAX_UPLOAD_SIZE', '10').to_i.megabytes
  end
end
```

### Health Checks

```ruby
# config/routes.rb
get '/health', to: 'health#check'

# app/controllers/health_controller.rb
class HealthController < ApplicationController
  def check
    checks = {
      database: database_check,
      redis: redis_check,
      external_api: external_api_check
    }

    status = checks.values.all? ? :ok : :service_unavailable
    render json: { status: status, checks: checks }, status: status
  end

  private

  def database_check
    ActiveRecord::Base.connection.execute('SELECT 1')
    'healthy'
  rescue => e
    "unhealthy: #{e.message}"
  end

  def redis_check
    Redis.current.ping
    'healthy'
  rescue => e
    "unhealthy: #{e.message}"
  end

  def external_api_check
    # Check external services
    'healthy'
  rescue => e
    "unhealthy: #{e.message}"
  end
end
```

Questa sezione DevOps fornisce una base solida per implementare pratiche moderne di deployment, monitoring e gestione dell'infrastruttura per progetti Ruby on Rails.
