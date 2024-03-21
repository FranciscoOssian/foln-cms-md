# SonarQube-Simple-Guide
Guia simples para uso do SonarQube com Docker

Este guia oferece instruções passo a passo para configurar e executar a análise de código do seu projeto usando o SonarQube e o SonarScanner, com base no ambiente Docker.

## Pré-requisitos

- Docker instalado no sistema.

## Configuração do SonarQube

1. **Inicie um Contêiner do SonarQube:**
   
   ```bash
   docker run -d -p 9000:9000 sonarqube
   ```

   Certifique-se de que o SonarQube está sendo executado acessando `http://localhost:9000` no seu navegador.

2. **DashBoard**
   
   Faça login no SonarQube com as credenciais padrão (admin/admin) e altere a senha após o login inicial.

3. **Projeto**

   Crie um novo projeto no SonarQube para o projeto e obtenha um token de acesso para a análise.

## Configuração e Execução do SonarScanner

1. **Executando o SonarScanner e declarando sua configuração ao mesmo tempo**

   ```bash
   docker run --rm --network=host \
   -e SONAR_HOST_URL="http://127.0.0.1:9000" \
   -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=${key} -Dsonar.sources=." \
   -e SONAR_TOKEN="seu_token_de_acesso" \
   -v "${YOUR_REPO_PATH}:/usr/src" \
   sonarsource/sonar-scanner-cli
  ```

 Nota: Não podemos especificar diretamente o caminho do projeto,
 pois estamos usando Docker.
 Em vez disso, precisamos vincular o caminho do projeto local
 a um caminho dentro do contêiner.

  E precisamos dizer que `sonar-scanner-cli` irá usar a mesma rede do host, para poder ter ascesso ao web server.


   
