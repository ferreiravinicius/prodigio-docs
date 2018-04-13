# Bandboxbind

## Sobre

Este componente é uma caixa de texto especial que incorpora uma janela de popup customizada \(_dropdown window_\). A janela de popup é aberta automaticamente com o atalho `Alt + ↓` ou ao pressionar o ícone de lupa. Normalmente utilizado para listar objetos.

## Utilização

{% tabs %}
{% tab title="XML" %}
```java
<bandboxbind nomeDoObjeto="classecontrole.objetoAtual.objetoExemplo"
             atributoQueSeraVisualizado="nome" 
             labelValorList="Nome:nome" />
```
{% endtab %}

{% tab title="Entidade" %}
```java
	public class EntidadeVO extends ProjetoBaseVO {
		
		private String nome;
		private EntidadeVO objetoExemplo; //Objeto do tipo VO
		
		//Getters e Setters
		public EntidadeVO getObjetoExemplo() {
			return objetoExemplo;
		}
		
		public void setObjetoExemplo(EntidadeVO valor) {
			this.objetoExemplo = valor;
		}
		
		public String getNome() {
			return nome;
		}

		public void setNome(String nome) {
			this.nome = nome;
		}
		//Demais propriedade e métodos ocultados para brevidade do exemplo
	}
```
{% endtab %}
{% endtabs %}

### Sumário

| **Atributo** | **Tipo** | **Descrição** |
| --- | --- | --- | --- |
| `nomeDoObjeto` | Object | Especifica o objeto que será vinculado ao componente. Normalmente uma Entidade \(VO\).  |
| `atributoQueSeraVisualizado` | String | Define o nome da propriedade que será exibida no campo de texto. Caso a propriedade seja `null` no objeto, o mesmo **não é listado**. |
| `labelValorList` | String | Define o cabeçalho e suas respectivas propriedades a serem exibidas na listagem de itens ao expandir o componente. Utilizar no modelo `Titulo1:propriedade1;Titulo2:propriedade2`. |



