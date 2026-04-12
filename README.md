# WBM Shopware Project

A Shopware 6 e-commerce project with WBM Product Type Plugin.

## Overview

This project is a Shopware 6-based e-commerce platform that includes custom plugins for enhanced product management and filtering capabilities. The project utilizes Docker for development and includes the WBM Product Type plugin for advanced product categorization.

## Features

- **Shopware 6.7.8.2** - Latest stable Shopware platform
- **WBM Product Type Plugin** - Custom product type management with:
  - Dedicated product type model
  - Admin UI for product type management
  - Storefront filtering capabilities
  - Elasticsearch integration

## Requirements

- Docker & Docker Compose
- Composer
- PHP 8.2+
- Node.js (for frontend builds)
- MySQL (handled by Docker environment)

## Installation

### 1. Clone the Repository

```bash
git clone git@github.com:vitaliyberdylo/wbm-shopware.git
cd wbm-shopware
```

### 2. Environment Setup

Copy the environment file and configure as needed:

```bash
cp .env .env.local
```

### 3. Start Docker Environment

```bash
make up
```

### 4. Install Dependencies

```bash
make setup
```

This will:
- Install Composer dependencies
- Run Shopware system installation
- Create and configure the database

### 5. Complete Admin Setup Wizard

After installation, access the Shopware admin panel and complete the setup wizard:
- Navigate to `http://localhost:8000/admin`
- Follow the initial setup wizard
- Configure basic settings

## Activate and migrate WbmProductType Plugin

```bash
docker compose exec web bin/console plugin:refresh
docker compose exec web bin/console plugin:install --activate WbmProductType
```

The migration runs automatically on install and creates the `wbm_product_extension` table.

### Build frontend assets

```bash
docker compose exec web bin/build-js.sh 
docker compose exec web bin/console cache:clear
```

### Elasticsearch / OpenSearch

The plugin decorates storefront and admin ES indexers to index `productType`. Make sure ES/OpenSearch is enabled and rebuild the index:

```bash
docker compose exec web bin/console es:index --no-queue
```

### PHP Unit tests

Make simlink for tests:

```bash
docker compose exec web ln -s /var/www/html/vendor/wbm/product-type /var/www/html/custom/plugins/WbmProductType
```

Run plugin unit tests:

```bash
docker compose exec web bash -c 'APP_ENV=test ./vendor/bin/phpunit -c custom/plugins/WbmProductType/phpunit.xml'
```


