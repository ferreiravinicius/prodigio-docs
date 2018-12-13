# Decimalboxbind

## Sobre

Componente de texto utilizada para a manipulação de números decimais.

## Utilização

{% tabs %}
{% tab title="XML" %}
```markup
<decimalboxbind nomeDoObjeto="classecontrole.objetoAtual.minhaPropriedade" 
    numCasasDecimais="2" mascara="true" isDouble="true" />
```
{% endtab %}

{% tab title="Entidade" %}
```java
public class MinhaEntidadeVO extends TutorialBaseVO {
    ...
    private Double minhaPropriedade;

    public Double getMinhaPropriedade() {
        return minhaPropriedade;
    }

    public void setMinhaPropriedade(Double minhaPropriedade) {
        this.minhaPropriedade= minhaPropriedade;
    }
    ...
    //Demais propriedades ocultadas para brevidade do exemplo
}
```
{% endtab %}
{% endtabs %}

## Sumário

| **Atributo** | **Tipo** | **Descrição** |
| :--- | :--- | :--- |
| `nomeDoObjeto` | `BigDecimal` ou `Double` | Seta o nome do objeto a partir da classe controle que sera vinculado \(_binded_\) ao componente. |
| `isDouble` | `Boolean` | Seta se o objeto vinculado ao componente é do tipo `Double`. Por padrão `false` . |
| `mascara` | `Boolean` | Seta se o componente irá receber a máscara em tempo real no lado do cliente \(Javascript\). Por padrão `true`. |
| `separadorDecimal` | `String` | Define o caractere que irá separar as casas decimais. Padrão `","`. |
| `separadorCentena` | `String` | Define o caractere que irá separar as centenas. Por padrão não foi definido nenhum caractere.  |

