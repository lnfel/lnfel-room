---
title: Staging
date: 2022-07-26 11:44:00 PM
categories: [Laravel, Deployment]
tags: [changelog]     # TAG names should always be lowercase
---

# Changes
These are the recent changes pushed to [staging](https://www-staging.pingsailor.com).

## Laravel

- [x] Override ModelMakeCommand default namespace to App\Models
- app/Console/Kernel.php
- app/Console/Commands/

- [x] Make RESELLER_ACCOUNT env naming consistent
- app/Http/Controllers/PricingController.php

- [x] Update and singularize Users model and delete app/User.php
- app/Models/Users.php
- app/User.php
- app/Models/User.php
- database/factories/ModelFactory.php
- database/migrations/2014_10_12_000000_create_users_table.php

- [x] Update GuzzleHttp from ^6.3 to ^7.4
- composer.json
- composer.lock

- [x] Update phpunit.xml
- phpunit.xml

- [x] Fix duplicate sending of headers when testing with phpunit
- bootstrap/app.php

- [x] A silly attempt to centralize and manage env variables
- config/env.php

- [x] Implement blade layout template
- resources/views/layouts/
- resources/views/index.blade.php
- resources/views/errors/

## Vue

- [x] Update package.json lodash, vue-cookie and vue/runtime-dom
- package.json
- package-lock.json

- [x] Update assets
- public/css/app.css
- public/js/app.js

- [x] Fix axios api calls
- resources/assets/js/api/countries.js
- resources/assets/js/api/pricing.js

- [x] Fix mysql 8 error on laravel 5
- config/database.php

- [x] Catch undefined routes and redirect to 404 page
- resources/assets/js/app.js
- resources/assets/js/components/Example.vue
- resources/assets/js/components/NotFound.vue

- [x] Update Home.vue text, links, method, country detect
- resources/assets/js/views/Home.vue

- [x] Fix relative path to image
- resources/assets/js/views/Policy.vue
- resources/assets/js/views/Terms.vue

## Git

- [x] Specify gitignore files and directories
- .gitignore
- storage/framework/cache/

## Workflows
#### Github Actions
- [x] Github Actions automated testing
- .github/laravel.yml

- [x] Update changelog automatically using release notes
- .github/workflows/update-changelog.yml
- CHANGELOG.md

- [x] Draft release automatically when pull requests are merged into default branch
- .github/workflows/release-drafter.yml
- .github/release-drafter.yml

- [x] Clean workflow logs manually
- .github/workflows/clean-workflow-logs.yml

#### Terminal based workflow
- [x] Deployment using deployphp/deployer
- deploy.php

## Work in Progress

  
### Laravel

- [ ] Reseller account feature
- app/Http/Controllers/ResellerController.php
- [ ] Implement Unit tests for Controllers
- tests/Unit/AccountControllerTest.php
- tests/Unit/ResellerControllerTest.php
- tests/Feature/Example.php

### Workflow
- [ ] Deployment workflow for staging environment using deployphp/deployer (currently testing)
- .github/workflows/deploy-staging.yml