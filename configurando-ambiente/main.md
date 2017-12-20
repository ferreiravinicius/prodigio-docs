# Configurando o Ambiente

1. [Maven](#maven)
    - [Configurando a Instalação](#configurando-a-instalação)
    - [Setando o Arquivo de Configuração (XML)](#setando-o-arquivo-de-configuração)
2. [JBoss (Servidor de Aplicação)]()
3. [PostgreSQL (Banco de Dados)]()


## Maven

#### Configurando a Instalação

Na barra de menu superior do Eclipse Luna selecione `Window > Preferences`.  
No submenu `Maven > Installations` clique em `Add`.

![Instalação do Maven](/imagens/maven/1-preferences-maven-installations-add.png)

No modal aberto, em `Installation Home` procure pelo diretório do Maven na pasta do Prodígio.  
_Como padrão em `C:\Prodemge\maven\apache-maven-x.x`._

![Selecionar o diretório do Maven](/imagens/maven/2-preferences-maven-new-runtime-directory.png)

Uma vez de volta no submenu `Installations` certifique-se de selecionar o runtime adicionado na etapa anterior.

![Marcar o runtime correto](/imagens/maven/3-maven-seleciona-runtime-aplica.png)

#### Setando o Arquivo de Configuração

Na janela `Window > Preferences` selecione o submenu `Maven > User Settings`.  
No campo `User Settings` selecione o arquivo `settings.xml` no diretório do Maven disponibilizado na pasta do Prodígio.  
_Como padrão em `C:\Prodemge\maven\apache-maven-x.x\conf\settings.xml`_

![Selecione o XML de configuração](/imagens/maven/4-preferences-usettings-browser.png)

<!--   -->
