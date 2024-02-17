# sqlserver-docker

Vou mostrar como podemos criar um container com o banco SQL Server usando duas formas: usando o Dockerfile e usando o Docker Compose.

## MODO 01: USANDO O Dockerfile

Nosso Dockerfile para criar uma imagem do SQL Server em Docker.

Um Dockerfile é um arquivo de texto que contém os comandos para construir uma imagem Docker.

Após isso, vamos criar nosso container SQL Server.

Um contêiner é uma instância isolada e portátil de uma imagem Docker.

Para criar um Dockerfile para o SQL Server, siga estes passos:

1. **Escolha uma imagem base do SQL Server** que atenda às suas necessidades. Você pode encontrar várias imagens oficiais do SQL Server no repositório da Microsoft ou no Docker Hub. Por exemplo, se você quiser usar a última versão do SQL Server 2019 no Linux, você pode usar a imagem `mcr.microsoft.com/mssql/server:2019-latest`.

2. **Abra um editor de código**, como o VSCode, e crie um arquivo chamado `Dockerfile`.

3. **Defina as variáveis de ambiente** que o SQL Server precisa para funcionar. As principais variáveis são `ACCEPT_EULA`, que indica que você aceita os termos de licença do SQL Server, e `MSSQL_SA_PASSWORD`, que define a senha do usuário `sa` (administrador do sistema). Por exemplo:

```Dockerfile
ENV ACCEPT_EULA=Y \
    MSSQL_SA_PASSWORD=Senha@123
```

4. **Exponha as portas necessárias** para o contêiner. Por padrão, o SQL Server usa a porta 1433 para se comunicar com os clientes. Por exemplo:

```Dockerfile
EXPOSE 1433
```

5. **Crie um volume** para persistir os dados do SQL Server fora do contêiner. Isso permite que você persista os dados mesmo se o contêiner for removido ou atualizado. Você pode especificar um caminho para o volume no seu sistema de arquivos ou deixar que o Docker crie um volume anônimo. Por exemplo:

```Dockerfile
VOLUME /var/opt/mssql
```

6. **Salve o arquivo Dockerfile**.

Meu arquivo ficou dessa forma:

```Dockerfile
# Use a imagem base do SQL Server
FROM mcr.microsoft.com/mssql/server:2019-latest

# Defina as variáveis de ambiente
ENV ACCEPT_EULA=Y \
    MSSQL_SA_PASSWORD=Senha@123

# Exponha a porta necessária
EXPOSE 1433

# Crie um volume para persistir os dados
VOLUME /var/opt/mssql
```

7. **No seu terminal**, navegue até o diretório onde está o arquivo Dockerfile e execute o comando `docker build -t sqlserver .` para construir a imagem Docker com o nome `sqlserver`. O ponto final indica que o Docker deve usar o arquivo Dockerfile no diretório atual.

8. **Após a construção da imagem**, execute o comando `docker run -d -p 1433:1433 --name sqlserver sqlserver` para criar e iniciar um contêiner com o nome `sqlserver` a partir da imagem `sqlserver`. O parâmetro `-d` indica que o contêiner deve rodar em segundo plano (modo detached). O parâmetro `-p 1433:1433` indica que a porta 1433 do contêiner deve ser mapeada para a porta 1433 do seu sistema. Você pode usar outras portas se preferir.

## MODO 02: USANDO O Docker Compose

O Docker Compose é uma ferramenta que permite definir e executar aplicativos Docker compostos por vários serviços. Ele utiliza um arquivo YAML chamado `docker-compose.yml` para definir a configuração dos serviços, incluindo imagens de contêiner, variáveis de ambiente, portas expostas, volumes e outras opções de configuração.


## Instale Docker Compose:

Para instalar o Docker Compose, você pode seguir os seguintes passos:

1. **Verifique os pré-requisitos**: Antes de instalar o Docker Compose, verifique se você tem o Docker Engine instalado em seu sistema. O Docker Compose é distribuído junto com o Docker Engine.

2. **Baixe o Docker Compose**: Você pode baixar a versão mais recente do Docker Compose do repositório oficial no GitHub. Para isso, você pode usar o seguinte comando:


sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Esse comando baixa a versão mais recente do Docker Compose e a salva no diretório `/usr/local/bin/`, que geralmente está incluído no PATH do sistema.

3. **Dê permissão de execução ao Docker Compose**: Após baixar o Docker Compose, você precisa dar permissão de execução ao arquivo baixado. Use o seguinte comando para isso:


sudo chmod +x /usr/local/bin/docker-compose


Isso permitirá que você execute o Docker Compose como um programa executável.

4. **Verifique a instalação**: Após dar permissão de execução, você pode verificar se o Docker Compose foi instalado corretamente digitando:


docker-compose --version

#
Esse comando exibirá a versão do Docker Compose instalada em seu sistema.

Agora que você instalou o Docker Compose, pode usar arquivos `docker-compose.yml` para definir e executar aplicativos Docker compostos por vários serviços, como no exemplo que forneci anteriormente. Com o Docker Compose, você pode gerenciar facilmente os contêineres de seus aplicativos com um único comando.




# USANDO DOCKER COMPOSE


Aqui está uma explicação detalhada do arquivo `docker-compose.yml`:

```yaml
version: '3.8'

services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2019-latest
    environment:
      ACCEPT_EULA: "Y"
      MSSQL_SA_PASSWORD: "Senha@123"
    ports:
      - "1433:1433"
    volumes:
      - mssql-data:/var/opt/mssql

volumes:
  mssql-data:
```

3. **Execução dos serviços**: Depois de definir o arquivo `docker-compose.yml`, você pode executar todos os serviços definidos nele com um único comando. Basta abrir um terminal, navegar até o diretório onde está o arquivo `docker-compose.yml` e executar o comando:

```bash
docker-compose up -d
```

Isso iniciará todos os serviços definidos no arquivo.



Agora você tem um contêiner do SQL Server rodando no seu sistema. Você pode se conectar ao SQL Server usando o usuário `sa` e a senha que você definiu no Dockerfile. Utilize qualquer ferramenta de cliente do SQL Server, como o Azure Data Studio ou o SQL Server Management Studio, para acessar e gerenciar seu banco de dados.