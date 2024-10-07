# Configurando Repositório com Conexão SSH

Este guia foca principalmente em comandos **bash**. No entanto, se você estiver utilizando o **PowerShell** no Windows, o fluxo será um pouco diferente:

- O caminho para a pasta **home** do usuário deve ser especificado usando **`$env:USERPROFILE\`** em vez de **`~/`**.

- Certifique-se de usar o **`\`** como separador de diretórios, conforme o padrão do Windows.

## Gerar Par de Chaves SSH

Para gerar o par de chaves SSH, utilize o seguinte comando:

```bash
ssh-keygen -t ed25519 -C "nome_da_key" -f ~/.ssh/minha_chave
# RSA: ssh-keygen -b 4096 -C "nome_da_key" -f ~/.ssh/minha_chave
```

### Parâmetros do comando `ssh-keygen`:

- **`ssh-keygen`**: Utilitário padrão para gerar pares de chaves SSH.

- **`-t ed25519`**: Define o algoritmo ED25519, mais seguro e eficiente. Alternativamente, o RSA pode ser usado para compatibilidade com sistemas legados.

- **`-C "nome_da_key"`**: Adiciona um comentário à chave pública para facilitar a identificação, como um e-mail ou descrição.

- **`-f ~/.ssh/minha_chave`**: Especifica o caminho e nome do arquivo para a chave privada. A chave pública será gerada no mesmo local.

- **`-b 4096`**: Define o tamanho da chave em bits (aplicável apenas ao RSA). Um valor de 4096 bits é recomendado para maior segurança.

### Dicas:

- É recomendado salvar suas chaves no diretório ~/.ssh do seu usuário, pois é o local padrão onde o SSH espera encontrar essas chaves.

- Durante a criação, você será solicitado a definir uma **_passphrase_** (senha), o que adiciona uma camada extra de segurança. Se você não quiser definir uma senha, basta pressionar Enter para pular essa etapa.

## Adicionar a Chave Pública no provedor do repositorio

Após gerar as chaves, copie a chave pública e adicione-a nas configurações de SSH do seu repositório.

## Configurar o Arquivo `~/.ssh/config`

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

## Testar a Conexão SSH

Após configurar o arquivo `config`, teste a conexão com o repositório utilizando o comando:

```bash
ssh -T github
```

> `github` é o nome do host definido no arquivo de configuração.

## Clonar o Repositório Usando SSH

Se o teste de conexão for bem-sucedido, você pode clonar o repositório utilizando o seguinte comando:

```bash
git clone github:UsuarioGithub/caminho/do/repositorio.git
```

## Alterar um Repositório de HTTPS para SSH

Caso já tenha um repositório configurado com HTTPS e deseje alterar para SSH, siga os passos abaixo.

### Verificar o Remote Atual:

```bash
git remote -v
```

### Atualizar o Remote para SSH:

Utilize o comando abaixo para atualizar a URL do repositório remoto:

```bash
git remote set-url origin github:UsuarioGithub/caminho/do/repositorio.git
```

Isso vinculará seu repositório local ao repositório remoto via SSH.
