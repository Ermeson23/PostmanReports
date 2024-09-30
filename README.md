# 🚀 Pipeline de Testes com GitHub Actions e Newman

## 📖 Introdução

Este repositório foi criado para implementar uma pipeline de testes automatizados utilizando **GitHub Actions** e **Newman**. O objetivo é garantir a qualidade das APIs por meio da execução de testes contínuos e geração de relatórios de resultados.

## 📂 Estrutura do Repositório

Dentro do diretório `.github/workflows`, você encontrará dois arquivos de workflow:

### 1. 🧪 **Newman Tests (Workflow 1)**
Este workflow é responsável por executar os testes da coleção Postman sempre que houver um `push` ou `pull_request`.

   ```yaml
   name: "Newman Tests"
   on: [push, pull_request]

   jobs:
     build:
       runs-on: ubuntu-latest
       container:
         image: postman/newman
       steps:
         - name: Checkout
           uses: actions/checkout@v2

         - name: Run API Tests
           run: newman run "Restful Booker BVT.postman_collection.json" -e Production.postman_environment.json
   ```
- Checkout: Clona o repositório para que o workflow tenha acesso aos arquivos necessários.
- Run API Tests: Executa os testes da coleção Postman especificada.
### 2. 📊 **Newman Tests com Relatório HTML (Workflow 2)**
Este workflow não só executa os testes, mas também gera um relatório HTML dos resultados.

  ```yaml
  name: Newman Tests
  on: [push, pull_request]
  
  jobs:
    test-api:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - name: Install Node
          uses: actions/setup-node@v4
          with: 
            node-version: '20.x'
        - name: Install newman
          run: |
            npm install -g newman
            npm install -g newman-reporter-htmlextra
        - name: Make Directory for results
          run: mkdir -p testResults
        - name: Run API Tests
          run: newman run "Restful Booker BVT.postman_collection.json" -e Production.postman_environment.json -r htmlextra --reporter-htmlextra-export testResults/htmlreport.html || true
        - name: Output the run Details
          uses: actions/upload-artifact@v4
          with: 
            name: RunReports
            path: testResults
  ```
  - Instalação do Node.js: Configura o ambiente com Node.js.
  - Instalação do Newman: Instala o Newman e o reporter HTML extra para geração de relatórios.
  - Criação de Diretório: Cria um diretório para armazenar os resultados dos testes.
  - Execução de Testes: Executa a coleção Postman com o reporter HTML, gerando um relatório.
  - Upload de Artefatos: Armazena o relatório gerado como artefato para referência futura.
## ⚙️ **Como usar**
  - Clone o repositório: ```git clone https://github.com/Ermeson23/PostmanReports.git```.
  - Adicione suas coleções e ambientes Postman conforme necessário.
  - Personalize os workflows, se necessário.
  - Realize um `push` ou crie um `pull_request` para acionar os testes.
## 🤝 **Contribuições**
Sinta-se à vontade para contribuir com melhorias, correções ou sugestões. Abra uma issue ou envie um pull request!
