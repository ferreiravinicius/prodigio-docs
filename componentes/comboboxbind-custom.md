# Comboboxbind \(custom\)

### Sobre

Este componente é uma caixa de edição especial com uma lista de seleção em _dropdown_ embutida. Similar a tag `select` do HTML.

{% hint style="info" %}
Atualmente este componente não faz parte do Prodígio. Necessário importar as classes no projeto.
{% endhint %}

### Utilização

{% tabs %}
{% tab title="XML" %}
```java
<comboboxbind 
    nomeDoObjeto="classecontrole.objetoAtual.objetoExemplo" 
    model="@{classecontrole.listaModelo}"
    atributoQueSeraVisualiazado="nome"
    labelPadrao="Selecione" />
```
{% endtab %}

{% tab title="Controller" %}
```java
public class ExemploCtr extends ExemploBaseCtr<ExemploVO> {

    private Set<ExemploVO> listaModelo;

    public Set<ExemploVO> getListaModelo()  throws Exception {
        return listaModelo;
    }

    //logica de recuperação da lista e setter ocultos para brevidade do exemplo
}
```
{% endtab %}

{% tab title="Entidade" %}
```java
public class ExemploVO extends ExemploBaseVO {

    private OutraEntidadeVO objetoExemplo;
    
    //Getters e Setters ocultados para brevidade do exemplo    
}
```
{% endtab %}
{% endtabs %}

### Sumário

| Atributo | Descrição |
| :--- | :--- |
| `nomeDoObjeto` \(Obrigatório\) | Especifica o caminho do objeto que será vinculado ao componente. |
| `model` \(Obrigatório\) | Especifica o caminho do método que recupera a lista modelo dos possíveis itens que poderão ser selecionados. Atualmente utilizando atributo nativo do ZK, logo é **obrigatório** manter a anotação de bind `@{ ... }` |
| `atributoQueSeraVisualizado` | Especifica o nome do atributo do objeto que sera exibido na tela ao renderizar o componente. Por padrão, se não especificado, utiliza o `toString()` |
| `labelPadrao` | Especifica a label utilizada no item padrão \(caso não selecionado nenhum item ou ao setar `null` no objeto\). Por padrão "selecione".  |

