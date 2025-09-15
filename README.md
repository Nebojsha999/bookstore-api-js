# Online bookstore testing automation

Tests are located in test/. There are tests for authors and books.

## Prerequisites
  * Node.js & npm
  * Docker (for running containers)
  * Jenkins (for CI/CD)

## Installation

How to clone and set up the project:

```bash
git clone https://github.com/Nebojsha999/bookstore-api-js.git
cd bookstore-api-js
npm install
```

## Running Tests

Local:
```bash
npm test
```

Dockerized
```bash
docker build -t bookstore-api-js-tests .
```

## CI/CD

Jenkins pipeline configured to build Docker image and run API tests.