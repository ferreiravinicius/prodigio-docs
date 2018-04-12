---
description: >-
  Componente utilizado para listagem e/ou seleção múltipla de uma coleção de
  objetos.
---

# Listboxbind

![Exemplo visual do componente.](.gitbook/assets/image%20%282%29.png)

### Sobre

Normalmente utilizada para representar uma relação [`@ManyToMany`](https://docs.oracle.com/javaee/7/api/javax/persistence/ManyToMany.html). Pode ser utilizado também apenas para listar e recuperar valores selecionados de uma coleção qualquer.

### Utilização

{% tabs %}
{% tab title="XML" %}
```java
<listboxbind
	nomeDoObjeto="classecontrole.objetoAtual.colecaoPersistida"
	colecaoModelo="classecontrole.colecaoModelo"
	propriedadeVisualizada="nomePropriedade"
	titulo="Título" /> 
```
{% endtab %}

{% tab title="Entidade" %}
```java
public class MinhaEntidadeVO extends TutorialBaseVO {
	...
	private Set<OutraEntidadeVO> colecaoPersistida =new LinkedHashSet<OutraEntidadeVO>();
	
	@ManyToMany(fetch = FetchType.EAGER)
	public Set<OutraEntidadeVO> getColecaoPersistida() {
		return colecaoPersistida;
	}

	public void setColecaoPersistida(Set<OutraEntidadeVO> colecaoPersistida) {
		this.colecaoPersistida = colecaoPersistida;
	}
	...
	//Demais propriedades ocultadas para brevidade do exemplo
}
```
{% endtab %}

{% tab title="Controller" %}
```java
public class MinhaEntidadeCtr extends TutorialBaseCtr<MinhaEntidadeVO> {
	...
	public Set<OutraEntidadeVO> getColecaoModelo() {
		try {
			return repositorio().listar(new OutraEntidadeVO());
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
	...
	//Demais propriedades ocultadas para brevidade do exemplo
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Todos atributos e suas respectivas descrições podem ser visualizados no [sumário](listboxbind.md#sumario).
{% endhint %}

### Sumário

| **Atributo** | **Tipo** | **Descrição** |
| --- | --- | --- | --- | --- |
| ` nomeDoObjeto` | Collection | Vincula um objeto da classe entidade que representa uma coleção `many-to-many` persistida.  |
| `colecaoModelo` | Collection | Seta a coleção que irá listar todos os possíveis itens que possam vir a ser selecionados. |
| `propriedadeVisualizada` | String | Seta o nome da propriedade da classe entidade que será exibida em cada item listado no componente. Padrão `toString()` da classe. |
| `titulo` | String | Define o título que será exibido no cabeçalho da caixa que envolve os itens listados. Não irá possuir cabeçalho se não for definido. |



