# Listar Baseado no Exemplo

### Sobre

O método `listarBaseadoNoExemplo(...)` é um método utilitário que simplifica a pesquisa de entidades \(VO\) persistidas no banco. 

É acessível de qualquer camada, seja `CTR`, `RN`ou `DAO`. Consiste em receber dois objetos de exemplo que irão servir de criteria na pesquisa a ser efetuada no banco, retornando assim uma lista de objetos que são semelhantes \(que possuem propriedades iguais às que foram setadas nos objetos de exemplo\).

Nos permite também definir os campos/propriedades que vão ser recuperados e setados nos objetos retornados, assim como fazemos numa query nativa.

### Funcionamento

Suponhamos que tenhamos a seguinte `Entidade` mapeada.

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

E tivéssemos os seguintes registros no banco. 

| id | modelo | cor | ano | marca\_id \(FK\) |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Uno | PRETO | 2011 | 1 _\(Fiat\)_ |
| 2 | Ka | BRANCO | 2015 | 2 _\(Ford\)_ |
| 3 | Siena | PRATA | 2012 | 1 _\(Fiat\)_ |
| 4 | Gol | PRETO | 2011 | 3 _\(Volks\)_ |

Utilizando o `listarBaseadoNoExemplo(...)` conseguiríamos efetuar as pesquisas.

TODO



