
# Configurando Repositório com Conexão SSH

## 1. Gerar Par de Chaves SSH

Para gerar o par de chaves SSH, utilize o seguinte comando:

```bash
ssh-keygen -t rsa -b 4096 -C "nome_da_key"
```

- Salve a chave no diretório `~/.ssh` do seu usuário.

## 2. Adicionar a Chave Pública ao Repositório

Após gerar as chaves, copie a chave pública e adicione-a nas configurações de SSH do seu repositório.

## 3. Configurar o Arquivo `~/.ssh/config`

Crie (ou edite) o arquivo `~/.ssh/config` para associar sua chave privada ao repositório.

### Exemplo de Configuração para GitHub:

```bash
Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/nome_da_key
```

> O valor de `Host` pode variar conforme o provedor do repositório.

### Exemplo de Configuração para Azure Repos:

```bash
Host azure
  HostName ssh.dev.azure.com
  User git
  IdentityFile ~/.ssh/nome_da_key
```

## 4. Testar a Conexão SSH

Após configurar o arquivo `config`, teste a conexão com o repositório utilizando o comando:

```bash
ssh -T github
```

> `github` é o nome do host definido no arquivo de configuração.

## 5. Clonar o Repositório Usando SSH

Se o teste de conexão for bem-sucedido, você pode clonar o repositório utilizando o seguinte comando:

```bash
git clone github:caminho/do/repositorio
```

## 6. Alterar um Repositório de HTTPS para SSH

Caso já tenha um repositório configurado com HTTPS e deseje alterar para SSH, siga os passos abaixo.

### Verificar o Remote Atual:

```bash
git remote -v
```

### Atualizar o Remote para SSH:

Utilize o comando abaixo para atualizar a URL do repositório remoto:

```bash
git remote set-url origin github:caminho/do/repositorio
```

Isso vinculará seu repositório local ao repositório remoto via SSH.
