> Artigo escrito e publicado em: [https://warcontent.com/branch-git-no-terminal/](https://warcontent.com/branch-git-no-terminal/)

# Como exibir a Branch atual do Git no Terminal - Dica de Produtividade

Você já trabalhou em um projeto com muitas Branches? Já se perdeu fazendo coisas na Branch errada?

Pois nesse artigo vou compartilhar uma dica muito bacana do [Glauco Custódio](https://glaucocustodio.github.io/2013/03/15/exibir-branch-atual-em-repositorios-git-no-terminal/) para exibir a branch atual no diretório do projeto.

Dessa forma você não precisa rodar `git status` toda hora para saber em qual branch está.

Note que, quando acessamos um projeto Git no Terminal, por padrão, obtemos apenas o usuário, host e path atual:

```
gabriel@war-note ~/Projects/warcontent $ 
```

Porém, podemos exibir a branch atual nessas informações, alterando a variável `PS1`…

Para isso adicione o seguinte código no final do arquivo `.bashrc` ou `.bash_profile` (ambos encontrados na home do usuário `~/`):

```
export PS1='\u@\h\[\033[01;34m\] \w\[\033[0;32m\]$(__git_ps1 " (%s)")\[\033[01;34m\]$\[\033[00m\] '
```

Por último, rode o comando source `~/.bashrc` ou source `~/.bash_profile`  para recarregar e ver a alteração sem precisar reiniciar o terminal.

E pronto! Assim você obterá um resultado parecido com:

```
gabriel@war-note ~/Projects/warcontent (master) $ 
```

Agora a branch atual, ou seja a `master`, é exibida entre parênteses…

> No caso de aparecer o erro __git_ps1: command not found, você vai precisar [configurar o Git Prompt](https://blog.jasonmeridth.com/posts/seeing-which-git-branch-i-m-on-via-my-console-prompt-git-ps1/?utm_source=pocket_mylist).

Além disso, você pode customizar à vontade. Modificando as cores e os valores exibidos.

Veja alguns valores que podemos definir na variável `PS1` para alterar nossa saída:

### Valores

* `\u:` Usuário Atual;
* `\h:` Nome da Máquina (host);
* `\H:` Nome da Máquina Completo;
* `\w:` Diretório de Trabalho Atual;
* `\W:` Diretório de Trabalho Atual com o nome base (último segmento apenas);
* `$(__git_ps1 "%s"):` Branch Atual, se estiver em um repositório git, caso contrário não exibe nada.

### Cores

* Azul: `\[\033[0;34m\]`
* Vermelho: `\[\033[0;31m\]`
* Vermelho Fluorescente: `\[\033[1;31m\]`
* Verde: `\[\033[0;32m\]`
* Verde Fluorescente: `\[\033[1;32m\]`
* Branco Forte: `\[\033[1;37m\]`
* Cinza: `\[\033[0;37m\]`
* Padrão: `\[\033[0m\]`

> Atenção, esse tutorial foi testado no Ubuntu 12.04, 16.04, 18.04 e 20.04.

Porém deve funcionar também em outras distribuições Linux e no Mac OS.

Se gostou dessa dica (ou não haha), **diga o que achou nos comentários abaixo:**


