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

Atributos Principais 

| **Atributo** | **Tipo** | **Descrição** |
| :--- | :--- | :--- |
| `nomeDoObjeto` | Object | Especifica o objeto que será vinculado ao componente. Normalmente uma Entidade \(VO\).  |
| `atributoQueSeraVisualizado` | String | Define o nome da propriedade que será exibida no campo de texto. Caso a propriedade seja `null` no objeto, o mesmo **não é listado**. Padrão invoca o `toString()` do objeto. |
| `labelValorList` | String | Define o cabeçalho e suas respectivas propriedades a serem exibidas na listagem de itens ao expandir o componente. Utilizar seguindo o modelo  `Titulo1:propriedade1;Titulo2:propriedade2`. Padrão invoca o `toString()` do objeto. |

Atributos Secundários

| **Atributo** | **Tipo** | **Descrição** |
| :--- | :--- | :--- |
| `identificador` | String | Define um identificador único ao componente. Similar ao `id` porem permite mais de um identificador por escopo. Utilizado quando existe aninhamento/dependência de componentes. |
| `metodoFiltro` | String | Define o nome do método que alimenta o componente com a coleção de itens a serem listados. Normalmente utilizado para filtrar a listagem. |
| `dependeDoComponente` | String | Seta o identificador do componente que o mesmo depende. Normalmente utilizado em aninhamento de componentes.  |



