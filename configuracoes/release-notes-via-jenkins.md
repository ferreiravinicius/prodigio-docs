# Release Notes via Jenkins

## Entrada de Dados pelo Jenkins

A entrada de dados deve seguir as seguintes normas:

* Usar o delimitador `#&#` para separar os campos.
* Atualmente deve conter 5 campos na devida ordem, onde eles são:
  * Identificador do repositório \(e que deve ser cadastrado previamente em cada componente seus respectivos repositórios\)
  * Data do commit no formato `AAAA-MM-DD`
  * Autor do commit \(nome do usuário\)
  * Identificador do commit \(SHA\)
  * O texto em si dos commits
* Antes de qualquer commit que indique novidade ou correção de bugs **deve haver** ao menos um commit indicando a versão, isso quer dizer que os commits de release irão ser atrelados ao commit de versão mais próximo \(que vier antes\).

## Anotações

Para que um commit seja identificado como release ou versão, o mesmo deve ser **explicitamente** anotado seguindo as seguintes normas:

| Anotação | Descrição |
| :--- | :--- |
| @release | Novas funcionalidades, alterações ou qualquer tipo de novidade. |
| @bugfix | Correção de bugs. |
| @versao | Anotação explicita de que o commit é referente a uma nova versão. Utilizado para versões geradas manualmente. |

{% hint style="info" %}
Os commits gerados pelo **usuário do Jenkins já são considerados como commit de versão**, sem a necessidade de inserir a anotação explicitamente. Os commits **sem anotação** alguma serão **ignorados**.
{% endhint %}

## Exemplos

{% tabs %}
{% tab title="Entrada" %}
```markup
ssc-admin #&# 2018-06-19 #&#  Teste Jenkins #&# 4fbd397 #&# 1.0-SNAPSHOT 
ssc-admin #&# 2018-06-19 #&#  Vinícius Ferreira #&# 4fbd397 #&# Novo componente autenticador X @release
ssc-admin #&# 2018-06-19 #&#  Vinícius Ferreira #&# 381ffe3 #&# Trocando import do util.Arrays @bugfix 
```

**Notas**  
**Linha 1:** O primeiro commit não foi explicitamente anotado com @versao pois foi gerado pelo usuário do Jenkins. 
{% endtab %}

{% tab title="Dados Persistidos" %}
A seguinte estrutura foi persistida:

* `ReleaseVO`_1.0-SNAPSHOT_ 
  * `NoteVO` Novo componente autenticador X 
  * `NoteVO` Trocando import do util.Arrays
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Entrada" %}
```text
ssc-admin #&# 2018-06-19 #&#  Jenkins #&# 4fbd397 #&# 2.0 
ssc-admin #&# 2018-06-19 #&#  Vinícius Ferreira #&# 381ffe3 #&# Trocando import do util.Arrays @bugfix 
ssc-admin #&# 2018-06-14 #&#  Vinícius Ferreira #&# 7392186 #&# Refatorando a FieldValidator e consequentemente Decimalboxbind 
ssc-admin #&# 2018-06-19 #&#  Vinícius Ferreira #&# d34f397 #&# 1.0-SNAPSHOT @versao
ssc-admin #&# 2018-06-18 #&#  p061992 #&# 7a157e5 #&# Corrige mensagem de erro para campos especificos @bugfix 
ssc-admin #&# 2018-06-14 #&#  Vinícius Ferreira #&# 24858de #&# Removendo propriedade não utilizada das máscaras  @bugfix
ssc-admin #&# 2018-06-14 #&#  Vinícius Ferreira #&# b0efe33 #&# Novo componente Listboxbind @release
```

**Notas**  
**Linha 1:** Não foi explicitamente anotado com `@versao` pois foi gerado pelo usuário do Jenkins.  
**Linha 3:** Será **ignorado** uma vez que não possuí anotação nenhuma.  
**Linha 4:** Versão Snapshot gerada manualmente foi necessário anotar com `@versao` uma vez que queremos explicitamente indicar o commit como identificador de nova versão. 
{% endtab %}

{% tab title="Dados Persistidos" %}
A seguinte estrutura foi persistida:

* `ReleaseVO`_2.0_
  * `NoteVO` Trocando import do util.Arrays 
* `ReleaseVO` _1.0-SNAPSHOT_
  * `NoteVO` Corrige mensagem de erro para campos especificos
  * `NoteVO` Removendo propriedade não utilizada das máscaras
  * `NoteVO` Novo componente Listboxbind
{% endtab %}
{% endtabs %}

