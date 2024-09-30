
# MyApp API (.NET Core)

## Visão Geral

**MyApp API** é uma aplicação RESTful desenvolvida em .NET Core, cujo objetivo é gerenciar um **sistema de cadastro de carros**. A API permite criar, atualizar, listar e remover informações sobre veículos. É ideal para concessionárias, locadoras de veículos ou qualquer aplicação que necessite de um controle detalhado sobre um inventário de carros.

## Funcionalidades

- **Cadastro de carros**: Permite registrar novos veículos no sistema com informações como marca, modelo, ano, e cor.
- **Atualização de dados**: Possibilita a modificação dos dados de um carro já registrado.
- **Listagem de carros**: Recupera informações sobre todos os veículos cadastrados ou de um veículo específico.
- **Remoção de carros**: Permite excluir carros do sistema.
- **Autenticação com JWT**: A API usa autenticação baseada em tokens JWT para garantir a segurança das operações.

## Tecnologias Utilizadas

- **ASP.NET Core 6.0**
- **Entity Framework Core**
- **PostgreSQL (ou outro banco de dados)**
- **JWT Authentication**
- **Swagger** para documentação da API
- **AWS EC2, S3, RDS** para deploy

## Requisitos

Antes de rodar o projeto, certifique-se de ter:

- **.NET SDK** (versão 6.0 ou superior) instalado
- **Banco de dados** configurado (PostgreSQL recomendado)
- **AWS CLI** configurada para deploy

## Configuração do Projeto

1. **Clone o repositório:**

   ```bash
   git clone https://github.com/usuario/MyAppApi.git
   cd MyAppApi
   ```

2. **Configurar variáveis de ambiente:**

   Defina as variáveis de ambiente necessárias para conectar ao banco de dados, como `ConnectionString`, e para configurar a autenticação JWT.

   ```bash
   export ConnectionStrings__DefaultConnection="Server=myServer;Database=myDB;User Id=myUser;Password=myPassword;"
   export JWT__SecretKey="minha-chave-secreta"
   ```

3. **Restaurar pacotes:**

   Execute o comando para restaurar os pacotes NuGet necessários:

   ```bash
   dotnet restore
   ```

4. **Atualizar o banco de dados (migrations):**

   Use o Entity Framework para aplicar as migrations e atualizar o banco de dados:

   ```bash
   dotnet ef database update
   ```

5. **Executar a aplicação localmente:**

   Para rodar a API localmente, use o seguinte comando:

   ```bash
   dotnet run
   ```

   A API estará disponível em `http://localhost:5000`.

## Documentação da API

A documentação da API foi gerada usando **Swagger**. Para acessá-la localmente, vá para:

```
http://localhost:5000/swagger
```

## Testes

Execute os testes unitários utilizando o comando:

```bash
dotnet test
```

## Deploy na AWS

### Estrutura de Deploy

Esta API foi implantada na **AWS** utilizando os seguintes serviços:

- **EC2**: Servidor para hospedar a aplicação.
- **RDS (PostgreSQL)**: Banco de dados gerenciado.
- **S3**: Armazenamento de arquivos (opcional, dependendo da aplicação).
- **Elastic Load Balancer (ELB)**: Para balanceamento de carga (opcional).

### Passo a Passo para Deploy

1. **Criação da Instância EC2**:
   - Criei uma instância EC2 com **Amazon Linux 2** e configurei as permissões de segurança para permitir o tráfego HTTP e HTTPS.
   - Instalei o **.NET Core Runtime** na instância para suportar a execução do aplicativo.

2. **Configuração do RDS**:
   - Criei um banco de dados PostgreSQL no **AWS RDS** e configurei o grupo de segurança para permitir conexões a partir da instância EC2.

3. **Configuração do Ambiente**:
   - Defini as variáveis de ambiente necessárias diretamente na instância EC2 para conectar ao RDS e configurar o JWT.

4. **Deploy da Aplicação**:
   - Fiz o upload do código da API para a instância EC2 via **SCP**.
   - Executei o aplicativo em segundo plano usando **systemd** ou **tmux**.

### Dificuldades no Deploy

Durante o processo de deploy na AWS, enfrentei algumas dificuldades que exigiram paciência e perseverança para resolver. Aqui estão alguns dos desafios e como consegui superá-los:

1. **Problema de Conexão com o RDS**:
   - **Erro**: A aplicação não conseguia se conectar ao banco de dados RDS.
   - **Solução**: Descobri que o grupo de segurança da instância EC2 não estava configurado corretamente para permitir tráfego na porta do PostgreSQL (5432). Corrigi isso ajustando as permissões de entrada no **Security Group**.

2. **Configuração de HTTPS no Nginx**:
   - **Erro**: O site estava rodando apenas via HTTP, o que causava problemas de segurança.
   - **Solução**: Instalei e configurei o **Nginx** como proxy reverso e usei o **Certbot** para configurar certificados SSL com o **Let's Encrypt**, garantindo que o tráfego fosse redirecionado de HTTP para HTTPS.

3. **Timeouts durante o deploy**:
   - **Erro**: A aplicação levava muito tempo para fazer deploy e, em alguns casos, o processo era interrompido.
   - **Solução**: Após algumas investigações, percebi que era necessário ajustar a configuração de tempo de inatividade e fazer tuning no grupo de segurança e na VPC da instância EC2. Além disso, otimizar o uso de CPU da instância ajudou a melhorar a performance.

---
