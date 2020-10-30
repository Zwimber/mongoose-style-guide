# Naming convention environments

## Production environment

Branch: master
Domain: [https://randomcompanyname.nl]
Database: [wl-rcn-production]

## Release environment

Branch: release
Domain: [https://release.randomcompanyname.nl]
Database: [wl-rcn-production]

## Develop environment

Branch: develop
Domain: [https://develop.randomcompanyname.nl]
Database: [wl-rcn-develop]

# Local environments

## Develop environment

Branch: develop / goal-branch
Domain: [https://develop.localhost:4200]
Database [wl-rcn-develop]

# Naming motivation

- Branch name: _based upon naming in git flow_
- Domain: _given by organisation, environments match subdomain_
- Database: _`wl-rcn` can be custom per organisation, database name matches environment label_

<!--
# For later on

 - Put components in packages

## Components should be in a package

- Table
- Flightplan
- .do / production.yaml
- Error handling / interceptor-service

### Server

- CRUD
- Get-from.js
- Table
- Image
- Extend-smart
- Model-Ref

Not ready

- Database seed -->
