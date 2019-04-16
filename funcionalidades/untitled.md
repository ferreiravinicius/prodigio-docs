# Listar Baseado no Exemplo

### Sobre

O método `listarBaseadoNoExemplo(...)` é um método utilitário que simplifica a pesquisa de entidades \(VO\) persistidas no banco. 

É acessível de qualquer camada, seja `CTR`, `RN`ou `DAO`. Consiste em receber dois objetos de exemplo que irão servir de criteria na pesquisa a ser efetuada no banco, retornando assim uma lista de objetos que são semelhantes \(que possuem propriedades iguais às que foram setadas nos objetos de exemplo\).

Nos permite também definir os campos/propriedades que vão ser recuperados e setados nos objetos retornados, assim como fazemos numa query nativa.

### Funcionamento

Para a seguinte `Entidade` mapeada que ira representar registros de um carro.

```java
@Entity
public class CarroVO extends MeuBaseVO {

    private String modelo;
    private CorEnum cor;
    private Integer ano;
    private MarcaVO marca;

    //Getters e Setters e Anotaçoes ocultados para brevidade do exemplo    
}
```

E para os seguintes registros já cadastrados no banco. 

| id | modelo | cor | ano | marca\_id \(FK\) |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Uno | PRETO | 2011 | 1 _\(Fiat\)_ |
| 2 | Ka | BRANCO | 2015 | 2 _\(Ford\)_ |
| 3 | Siena | PRATA | 2012 | 1 _\(Fiat\)_ |
| 4 | Gol | PRETO | 2011 | 3 _\(Volks\)_ |

Exemplos de pesquisar possíveis utilizando o `listarBaseadoNoExemplo(...)` :  
Nota: abreviaremos o nome do método para `lbne(...)` para brevidade do exemplo.

#### 1. Consultar todos os carros de cor "PRETO"

```java
CarroVO exemplo = new CarroVO();
exemplo.setCor(CorEnum.PRETO);
Set<CarroVO> resultado = lbne(exemplo, null, 0, Integer.MAX_VALUE, "id", "modelo");
//Retorna ["Uno", "Gol"]
```

#### 2. Consultar todos os carros entre o ano  2012 e 2015

```java
CarroVO exemplo1 = new CarroVO();
CarroVO exemplo2 = new CarroVO();
exemplo1.setAno(2012);
exemplo2.setAno(2015);
Set<CarroVO> resultado = lbne(exemplo1, exemplo2, 0, Integer.MAX_VALUE, "id", "modelo");
//Retorna ["Ka", "Siena"]
```

{% hint style="info" %}
Para campos do tipo `Number` e `Date` se o mesmo tiver sido setado em ambos objetos de exemplo é criado uma restrição de `BETWEEN` entre os valores setado nos objetos, respectivamente. 
{% endhint %}

#### 3. Consultar carros que possuem a letra "n" no nome do modelo

```java
String letraSolicitada = "n";
CarroVO exemplo = new CarroVO();
exemplo.setModelo("%" + letraSolicitada + "%");
Set<CarroVO> resultado = lbne(exemplo, null, 0, Integer.MAX_VALUE, "id", "modelo");
//Retorna ["Uno", "Siena"]
```

{% hint style="info" %}
Assim como em uma query nativa, se acrescentar `%` no início e/ou no fim em um campo String é criado uma restrição com `LIKE`. 
{% endhint %}

