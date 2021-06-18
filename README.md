## 📋Tutorial to create rails app step-by-step

### Create new app

**Rails new** has many options. Use 👉```$ rails new --help``` to see all list. We will create an app with **RSpec** tests and **PostgreSQL** database.
* navigate to folder that will contain project and write to console: ```$ rails new <appname> --database=postgresql -T``` <br>flag **-T** required to skip default test generation. 
<br>

### 🗃Configure pg

  * ```$ sudo apt install postgresql postgresql-contrib libpq-dev``` (required libs to work with pg)

  * ```$ sudo -u postgres createuser <username>``` press Enter and enter a 🔑password.
 
  * configure _config/database.yml_: 
  ```...
      default: &default
        adapter: postgresql
        encoding: unicode
        # For details on connection pooling, see Rails configuration guide
        # http://guides.rubyonrails.org/configuring.html#database-pooling
        pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
        username: <your_db_username>
        password: <%= ENV['APPNAME_DATABASE_PASSWORD'] %>

      development:
        <<: *default
        database: appname_development
      ...
   ```
   * open _Gemfile_ and add ```gem 'dotenv-rails', :groups => [:development, :test]``` (it required to 🔐secure password usage)

   * create file _.env_ in root directory and add ```APPNAME_DATABASE_PASSWORD=<your_db_password``` (‼️add _.env_ to _.gitignore_)

   * run ```$ rails db:create``` in terminal
 
   * run ```$ rails s``` (if you have no errors go next, if you have an error use Google to solve it)
<br>

 ### 📝Configure RSpec 
 
   **🟢Rspec**
 
   * add to _Gemfile_ 
   ```
     group :development, :test do
      gem 'rspec-rails'
     end
   ```
   * run ```$ bundle install```

   * run ```$ rails generate rspec:install``` 
   <hr>

   **🔵Database Cleaner**

   * to keep project's test database clean between test runs add to _Gemfile_:
   ```
     group :test do
      gem 'database_cleaner-active_record'
     end
   ```
   * run ```$ bundle install```

   * configure _spec/rails_helper.rb_:
   ```
   RSpec.configure do |config|
    ...
    config.before(:suite) do
      DatabaseCleaner.strategy = :truncation
      DatabaseCleaner.clean_with(:truncation)
    end

    config.before do
      DatabaseCleaner.strategy = :transaction
      DatabaseCleaner.start
    end

    config.append_after do
      DatabaseCleaner.clean
    end
   end
   ``` 
   <hr>
 
  **🟣Shoulda**
   
  * to use methods provided in Ruby on Rails framework add to _Gemfile_:
  ```
  group :test do
    gem 'shoulda-matchers'
  end
  ```
  
  * configure _spec/rails_helper.rb_:
  ```
  Shoulda::Matchers.configure do |config|
    config.integrate do |with|
      with.test_framework :rspec
      with.library :rails
    end
  end
  ```
  <hr>

  **🟡FactoryBot**
  
  * to avoid db requests during testing we can use factories, add to _Gemfile_:
  ```
  group :development, :test do
    gem 'factory_bot_rails'
  end
  ```
  
  * configure _spec/rails_helper.rb_:
  ```
  RSpec.configure do |config|
    ...
    config.include FactoryBot::Syntax::Methods
  end
  ```
  * run ```$ rspec spec``` (if you have no errors go next, if you have an error use Google to solve it)
 <br>
 
### 🪄Configure Rubocop

 * add to _Gemfile_:
 ```
  gem 'rubocop-rails', require: false
 ```
 
 * create [**rubocop config files**](https://gist.github.com/dsandstrom/d9da0be5003c2217969a)
 
 * add to config files ```require: rubocop-rails```

 * run ```$ rubucop --require rubocop-rails``` and fix rubocop⚠️warnings (or edit config files)
 
 * to disable **missing frozen string literal comment** add to _.rubocop.yml_:
 ```
  Style/FrozenStringLiteralComment:
   Enabled: false
 ```

...✏️to be continued
