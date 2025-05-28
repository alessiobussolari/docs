# Performance Guidelines

Linee guida per ottimizzare le performance del codice Ruby e Rails nei progetti PANDEV.

## 🚀 Performance Ruby Generale

### String Operations

**❌ Evitare:**
```ruby
# String concatenation in loop
result = ""
1000.times { |i| result += "item #{i}" }

# Multiple gsub calls
text.gsub(/foo/, 'bar').gsub(/baz/, 'qux').gsub(/old/, 'new')
```

**✅ Preferire:**
```ruby
# Use array join
items = []
1000.times { |i| items << "item #{i}" }
result = items.join

# Chain gsub or use single regex
text.gsub(/foo|baz|old/, 'foo' => 'bar', 'baz' => 'qux', 'old' => 'new')

# Or frozen strings
TEMPLATE = "Hello %s".freeze
result = TEMPLATE % name
```

### Array/Hash Operations

**❌ Evitare:**
```ruby
# Multiple iterations
users.select(&:active?).map(&:name).sort

# Hash access in loop
users.each do |user|
  puts settings['default_role'] # Hash lookup ogni iterazione
end

# include? su array grandi
large_array.include?(item) # O(n)
```

**✅ Preferire:**
```ruby
# Single iteration con lazy
users.lazy.select(&:active?).map(&:name).force.sort

# Cache hash lookup
default_role = settings['default_role']
users.each do |user|
  puts default_role
end

# Use Set per membership test
require 'set'
large_set = large_array.to_set
large_set.include?(item) # O(1)
```

### Method Calls

**❌ Evitare:**
```ruby
# Repeated method calls
def expensive_calculation
  heavy_compute_operation
end

# In loop
items.each do |item|
  puts expensive_calculation # Chiamato ogni volta
end

# send vs direct call
obj.send(:method_name, args)
```

**✅ Preferire:**
```ruby
# Memoization
def expensive_calculation
  @expensive_calculation ||= heavy_compute_operation
end

# Cache outside loop
cached_result = expensive_calculation
items.each do |item|
  puts cached_result
end

# Direct method call
obj.method_name(args)
```

## 🗄️ Database Performance

### Query Optimization

**❌ Problemi N+1:**
```ruby
# N+1 Query Problem
users = User.all
users.each do |user|
  puts user.posts.count # Query per ogni user
  puts user.profile.name # Query per ogni user
end

# Inefficient count
User.all.count # Carica tutti i record in memoria
```

**✅ Eager Loading:**
```ruby
# Use includes/joins
users = User.includes(:posts, :profile)
users.each do |user|
  puts user.posts.size # Usa array già caricato
  puts user.profile.name # Già caricato
end

# Use database count
User.count # Query diretta al database
```

### Selective Loading

**❌ Evitare:**
```ruby
# Load tutti i campi
users = User.all

# Load record non necessari
all_posts = Post.all
published_posts = all_posts.select(&:published?)
```

**✅ Preferire:**
```ruby
# Select solo campi necessari
users = User.select(:id, :name, :email)

# Filter nel database
published_posts = Post.where(published: true)
```

### Index Usage

**❌ Queries senza indici:**
```ruby
# Senza indice su email
User.where(email: 'user@example.com')

# Composite search senza indice
User.where(active: true, role: 'admin')

# LIKE queries inefficienti
User.where('name LIKE ?', '%john%')
```

**✅ Con indici appropriati:**
```ruby
# Migration con indici
class AddIndicesToUsers < ActiveRecord::Migration[7.0]
  def change
    add_index :users, :email, unique: true
    add_index :users, [:active, :role]
    add_index :users, :name # Per prefix searches
  end
end

# Use prefix searches quando possibile
User.where('name LIKE ?', 'john%') # Può usare indice
```

### Batch Processing

**❌ Evitare:**
```ruby
# Load tutti i record in memoria
User.all.each do |user|
  ProcessUserJob.perform_now(user)
end

# Update senza batch
users.each do |user|
  user.update(processed: true)
end
```

**✅ Batch Processing:**
```ruby
# Use find_each per batch processing
User.find_each(batch_size: 1000) do |user|
  ProcessUserJob.perform_later(user)
end

# Bulk updates
User.where(id: user_ids).update_all(processed: true)

# Use insert_all per bulk inserts
data = [
  { name: 'User 1', email: 'user1@example.com' },
  { name: 'User 2', email: 'user2@example.com' }
]
User.insert_all(data)
```

## 🏃‍♂️ Rails Performance

### Controller Optimization

**❌ Evitare:**
```ruby
class UsersController < ApplicationController
  def index
    @users = User.all # Carica tutto
    @user_count = User.count # Query separata
  end

  def show
    @user = User.find(params[:id])
    @posts = @user.posts.order(:created_at) # N+1 potenziale
  end
end
```

**✅ Ottimizzato:**
```ruby
class UsersController < ApplicationController
  def index
    @users = User.includes(:profile)
                 .page(params[:page])
                 .per(20)
    @user_count = @users.total_count # Usa pagination count
  end

  def show
    @user = User.includes(posts: :tags)
                .find(params[:id])
  end
end
```

### View Optimization

**❌ Evitare:**
```erb
<!-- Queries nelle view -->
<% User.all.each do |user| %>
  <div>
    <%= user.name %>
    <%= user.posts.published.count %> <!-- N+1 -->
  </div>
<% end %>

<!-- Helper chiamati ripetutamente -->
<% @items.each do |item| %>
  <%= format_currency(item.price, current_user.currency) %>
<% end %>
```

**✅ Ottimizzato:**
```erb
<!-- Data preparate nel controller -->
<% @users_with_post_counts.each do |user, post_count| %>
  <div>
    <%= user.name %>
    <%= post_count %>
  </div>
<% end %>

<!-- Cache helper results -->
<% user_currency = current_user.currency %>
<% @items.each do |item| %>
  <%= format_currency(item.price, user_currency) %>
<% end %>
```

### Caching Strategies

**Fragment Caching:**
```erb
<!-- Cache fragments costosi -->
<% cache [@user, 'profile_summary'] do %>
  <div class="profile">
    <%= render 'expensive_profile_calculation' %>
  </div>
<% end %>

<!-- Cache collections -->
<% cache ['users_list', @users.maximum(:updated_at)] do %>
  <%= render @users %>
<% end %>
```

**Low-level Caching:**
```ruby
class User < ApplicationRecord
  def expensive_calculation
    Rails.cache.fetch("user_#{id}_calculation", expires_in: 1.hour) do
      # expensive operation
      complex_calculation_result
    end
  end
end
```

**Russian Doll Caching:**
```erb
<!-- Outer cache -->
<% cache ['articles', @articles.maximum(:updated_at)] do %>
  <% @articles.each do |article| %>
    <!-- Inner cache -->
    <% cache article do %>
      <%= render article %>
    <% end %>
  <% end %>
<% end %>
```

## 📊 Monitoring e Profiling

### Profiling Tools

**Gemfile per profiling:**
```ruby
group :development do
  gem 'rack-mini-profiler'
  gem 'memory_profiler'
  gem 'stackprof'
  gem 'ruby-prof'
end
```

**Memory Profiling:**
```ruby
require 'memory_profiler'

report = MemoryProfiler.report do
  # codice da profilare
  User.includes(:posts).limit(100).to_a
end

report.pretty_print
```

**CPU Profiling:**
```ruby
require 'stackprof'

StackProf.run(mode: :cpu, out: 'tmp/stackprof.dump') do
  # codice da profilare
  heavy_computation
end

# Analizza risultati
# stackprof tmp/stackprof.dump --text
```

### Query Analysis

**Log Analysis:**
```ruby
# development.rb
config.log_level = :debug

# Abilita query analysis
config.active_record.verbose_query_logs = true

# Warn su slow queries
config.active_record.warn_on_records_fetched_greater_than = 500
```

**Bullet Gem (N+1 Detection):**
```ruby
# Gemfile
group :development do
  gem 'bullet'
end

# development.rb
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
  Bullet.rails_logger = true
end
```

## 🔧 Production Optimization

### Server Configuration

**Puma Configuration:**
```ruby
# config/puma.rb
workers ENV.fetch("WEB_CONCURRENCY") { 2 }
threads_count = ENV.fetch("RAILS_MAX_THREADS") { 5 }
threads threads_count, threads_count

preload_app!

before_fork do
  ActiveRecord::Base.connection_pool.disconnect! if defined?(ActiveRecord)
end

on_worker_boot do
  ActiveRecord::Base.establish_connection if defined?(ActiveRecord)
end
```

**Database Connection Pool:**
```yaml
# config/database.yml
production:
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000
  prepared_statements: false
  advisory_locks: false
```

### Background Jobs

**Sidekiq Optimization:**
```ruby
# config/initializers/sidekiq.rb
Sidekiq.configure_server do |config|
  config.redis = { 
    url: ENV['REDIS_URL'],
    network_timeout: 5,
    pool_timeout: 5
  }
end

# Job best practices
class ProcessUserJob < ApplicationJob
  queue_as :default
  
  def perform(user_id)
    # Usa ID invece di oggetti
    user = User.find(user_id)
    
    # Batch processing se possibile
    process_user_data(user)
  rescue ActiveRecord::RecordNotFound
    # Handle gracefully
    Rails.logger.warn "User #{user_id} not found"
  end
end
```

## 📈 Performance Metrics

### Key Metrics da Monitorare

1. **Response Time**: < 200ms per API requests
2. **Database Query Time**: < 50ms per query
3. **Memory Usage**: < 200MB per worker
4. **Cache Hit Rate**: > 90%
5. **Background Job Queue**: < 100 pending jobs

### Monitoring Setup

**Application Performance Monitoring:**
```ruby
# Gemfile
gem 'scout_apm'
# o
gem 'newrelic_rpm'

# Custom metrics
class ApplicationController < ActionController::Base
  around_action :track_performance

  private

  def track_performance
    start_time = Time.current
    yield
    duration = Time.current - start_time
    
    Rails.logger.info "Action: #{action_name}, Duration: #{duration}ms"
  end
end
```

## 🚨 Performance Anti-patterns

### Da Evitare Assolutamente

1. **Queries in Loop**: Sempre usare eager loading
2. **Loading tutto in memoria**: Usare pagination e batch processing
3. **No caching**: Implementare caching strategy appropriata
4. **Ignoring indices**: Sempre analizzare query plans
5. **Synchronous external calls**: Usare background jobs
6. **No monitoring**: Implementare metrics e alerting

### Code Review Checklist

- [ ] Queries ottimizzate (no N+1)
- [ ] Eager loading appropriato
- [ ] Indici database necessari
- [ ] Caching implementato dove necessario
- [ ] Pagination per grandi dataset
- [ ] Background jobs per operazioni lente
- [ ] Memory usage ragionevole
- [ ] No repeated computations costose

Seguendo queste linee guida, i progetti PANDEV manterranno performance ottimali anche con il crescere della complessità e del volume di dati.
