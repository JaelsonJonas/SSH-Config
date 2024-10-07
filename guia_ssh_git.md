# Configurando Repositório com Conexão SSH

## Gerar Par de Chaves SSH

Para gerar o par de chaves SSH, utilize o seguinte comando:

```bash
ssh-keygen -t ed25519 -C "nome_da_key" -f ~/.ssh/minha_chave
# RSA: ssh-keygen -b 4096 -C "nome_da_key" -f ~/.ssh/minha_chave
```

#### Explicação dos parâmetros utilizados:

- **`ssh-keygen`**: Esse é o utilitário padrão para gerar pares de chaves SSH.

- **`-t ed25519`**: Define o **tipo de algoritmo** a ser usado para gerar o par de chaves.

  - Aqui, o algoritmo **ED25519** é utilizado, que é altamente seguro e mais eficiente em termos de desempenho e tamanho de chave.

  - Alternativamente, você pode usar **RSA** com o comando comentado abaixo (não recomendado para novos projetos, mas amplamente compatível com sistemas legados).

- **`-C "nome_da_key"`**: Este parâmetro adiciona um **comentário** à chave pública gerada.

  - Esse comentário ajuda a identificar a chave posteriormente, como quando múltiplas chaves são usadas. Ele pode ser algo descritivo como seu e-mail ou uma descrição da chave, ex: `"minha_chave_trabalho"`.

- **`-f ~/.ssh/minha_chave`**: Especifica o **caminho e o nome do arquivo** para a chave privada gerada.

  - Nesse caso, a chave privada será salva em `~/.ssh/minha_chave` e a chave pública correspondente será gerada como `~/.ssh/minha_chave.pub`.

- **`-b 4096`**: Define o tamanho da chave em **bits**.
  - Este parâmetro só é usado com o algoritmo **RSA**. Um tamanho de chave de **4096 bits** é recomendado para uma maior segurança, pois oferece mais resistência a tentativas de quebra.
  - Um valor mais comum é **2048 bits**, mas 4096 bits oferece uma segurança mais robusta a longo prazo.

#### Dicas:
- É recomendado salvar suas chaves no diretório `~/.ssh` do seu usuário, pois é o local padrão onde o SSH espera encontrar essas chaves.
- Durante a criação, você será solicitado a definir uma **passphrase** (senha), o que adiciona uma camada extra de segurança. Se você não quiser definir uma senha, basta pressionar Enter para pular essa etapa.

Ao usar o comando **ED25519**, você estará optando por uma chave moderna, mais rápida e segura. Se preferir **RSA**, o segundo comando comentado oferece a alternativa, com uma chave de 4096 bits para maior segurança.


## Adicionar a Chave Pública ao Repositório

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
git clone github:caminho/do/repositorio
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
git remote set-url origin github:caminho/do/repositorio
```

Isso vinculará seu repositório local ao repositório remoto via SSH.
