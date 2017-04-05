# Lamp for php development env
---
## Directories structure
```
path/to/project
|-- ansible     // for deploy and rollback
|-- docker      // for build docker images
|   |-- apache2
|   |-- mysql
|   |-- ubuntu
|
`-- source      // for source folder : laravel
```
## Usage
Using composer to launcher development env
```
docker-compose up
```