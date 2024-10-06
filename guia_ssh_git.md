
# Configurando Repositório com Conexão SSH

## 1. Gerar par de chaves para autenticação

Utilize o comando abaixo para gerar o par de chaves SSH:

```bash
ssh-keygen -t rsa -b 4096 -C "nome_da_key"
```

Salve a chave no diretório `~/.ssh` do seu usuário.

## 2. Adicionar a chave pública ao repositório

Após gerar as chaves, adicione a chave pública ao seu repositório.

## 3. Configurar o arquivo `~/.ssh/config`

Crie o arquivo de configuração `~/.ssh/config` e configure para atrelar a chave privada ao repositório.

### Exemplo de configuração:

```
Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/nome_da_key
```

> Lembre-se de que o host pode variar conforme o provedor do repositório.

Para AzureRepos, por exemplo, a configuração seria:

```
Host azure
  HostName ssh.dev.azure.com
  User git
  IdentityFile ~/.ssh/nome_da_key
```

## 4. Testar a conexão

Com o arquivo `config` preenchido, teste a conexão usando o comando:

```bash
ssh -T github
```

> Esse `github` é o host que você preencheu no arquivo de configuração.

## 5. Clonar o repositório

Caso o teste tenha sido bem-sucedido, faça o clone do repositório com:

```bash
git clone github:caminho/do/repositorio
```

