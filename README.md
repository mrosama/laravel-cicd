composer self-update --1

composer update --ignore-platform-reqs



// .github/workflows/laravel.yml


laravel-tests:


  runs-on: ubuntu-latest


  steps:
    - uses: actions/checkout@v2
    - name: Copy .env
      run: php -r "file_exists('.env') || copy('.env.example', '.env');"
    - name: Install Dependencies
      run: composer install -q --no-ansi --no-interaction --no-scripts --no-progress --prefer-dist
    - name: Generate key
      run: php artisan key:generate
    - name: Directory Permissions
      run: chmod -R 777 storage bootstrap/cache
    - name: Create Database
      run: |
        mkdir -p database
        touch database/database.sqlite
    - name: Execute tests (Unit and Feature tests) via PHPUnit
      env:
        DB_CONNECTION: sqlite
        DB_DATABASE: database/database.sqlite
      run: vendor/bin/phpunit
===============
.env.ci
    - name: Copy ENV Laravel Configuration for CI
      run: php -r "file_exists('.env') || copy('.env.ci', '.env');"

DB_CONNECTION=sqlite
DB_DATABASE=database/database.sqlite
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=laravel
# DB_USERNAME=root
# DB_PASSWORD=


 - name: Create DB and schemas
      run: |
        mkdir -p database
        touch database/database.sqlite
        php artisan migrate
==============================================

// .github/workflows/laravel.yml

build:
  runs-on: ubuntu-latest

  steps:

    - name: Checkout

      uses: actions/checkout@v2

    - name: Setup Node.js

      uses: actions/setup-node@v2-beta

      with:

        node-version: '12'

        check-latest: true

    - name: Install NPM dependencies

      run: npm install

    - name: Compile assets for production

      run: npm run production
==========================================================


// .github/workflows/laravel.yml

deploy:

  runs-on: ubuntu-latest

  steps:

    - name: Checkout

      uses: actions/checkout@v2

    - name: Deployment

      uses: appleboy/ssh-action@main

      with:

        host: ${{ secrets.SSH_HOST }}

        key: ${{ secrets.SSH_PRIVATE_KEY }}

        username: ${{ secrets.SSH_USERNAME }}

        script: |

          cd /var/www/html/

          git checkout -f 

          git pull
=================================

    - name: Run Migrations
# Set environment
      env:
        DB_CONNECTION: mysql
        DB_DATABASE: db_test_laravel
        DB_PORT: 33306
        DB_USER: root

      run: php artisan migrate
