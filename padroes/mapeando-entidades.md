# Mapeando Entidades

### CAMPOS COM COLEÇÕES

1. **Utilizar Set** Evita [MultipleBagFetchException](https://thoughts-on-java.org/hibernate-tips-how-to-avoid-hibernates-multiplebagfetchexception/) e algumas abstrações do framework dependem da implementação do Set. 
2. **Inicializar as coleções na declaração** O framework utliza "Reflexão" para recuperar os campos, logo precisamos das coleções inicializadas para saber qual classe esta associada à coleção. 
3. **Anotações com parâmetros obrigatórios** 
   1. **FetchType**  Utilizar **LAZY** sempre que possível para buscar informações sob demanda e evitar problemas de performance. 
   2. **Cascade** Utilizar **ALL**, **MERGE** ou **PERSIST**. O framework  só inicializa as listas sob demanda, de forma automatica,  quando estão anotadas com um desses tipos de _cascade._  
   3. **MappedBy** Necessário especificar o nome do campo na classe filho que referencia o parente \(anotado no filho como @ManyTo\*\). 

{% hint style="info" %}
Para relacionamentos de **muitos-para-muitos** é recomendado mapear uma entidade para a tabela associativa e mapear um relacionamento de **@OneToMany** nas entidades das tabelas relacionadas para a entidade da tabela associativa  \(ao invés de mapear com **@ManyToMany**\). 
{% endhint %}

Exemplos:

{% tabs %}
{% tab title="@OneToMany" %}
{% code-tabs %}
{% code-tabs-item title="ParenteVO.java" %}
```java
@Entity
@Table(name = "TB_PARENTE")
public class ParenteVO extends AppBaseVO {
	
	private Set<FilhoVO> filhos = new LinkedHashSet<>(); // (1) e (2) set inicializado

	@OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.ALL, mappedBy = "nomeAtributoParente", orphanRemoval = true) // (3) anotação parametrizada
	public Set<FilhoVO> getFilhos() {
		return filhos;
	}

	public void setFilhos(Set<FilhoVO> filhos) {
		this.filhos = filhos;
	}
	
	// demais campos e métodos ocultados para brevidade
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="@ManyToMany" %}
Mapeamento Tabela Associativa

{% code-tabs %}
{% code-tabs-item title="AutorLivroVO.Java" %}
```java
@Entity 
@Table(name = "TB_AUTOR_LIVRO")
public class AutorLivroVO extends AppBaseVO {
    
    private AutorVO autor;
	private LivroVO livro;

    @ManyToOne
	@JoinColumn(name = "ID_AUTOR")
	public AutorVO getAutor() {
		return autor;
	}
	
	@ManyToOne
	@JoinColumn(name = "ID_LIVRO")
	public LivroVO getLivro() {
		return livro;
	}    
	
	// demais campos e métodos ocultados para brevidade
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Mapeamento Tabelas Relacionadas

```java
@Entity
@Table(name = "TB_AUTOR")
public class AutorVO extends AppBaseVO {
	
	private Set<AutorLivroVO> livros = new LinkedHashSet<>();

	@OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.PERSIST, mappedBy = "autor")
	public Set<AutorLivroVO> getLivros() {
		return livros;
	}

	@Transient
	public Set<LivroVO> getEntidadesLivros() {
		// método auxiliar para recuperar apenas as entidades da tabela livro 
		// ao invés das entidades da tabela associativa
		return getLivros().stream()
			.map(AutorLivroVO::getLivro)
			.collect(Collectors.toSet());
	}

	// demais campos e métodos ocultados para brevidade
}
```

{% code-tabs %}
{% code-tabs-item title="LivroVO.java" %}
```java
@Entity
@Table(name = "TB_LIVRO")
public class LivroVO extends AppBaseVO {
	
	private Set<AutorLivroVO> autores = new LinkedHashSet<>();

	@OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.PERSIST, mappedBy = "livro")
	public Set<AutorLivroVO> getAutores() {
		return autores;
	}
	
	@Transient
	public Set<AutorVO> getEntidadesAutores() {
		// método auxiliar para recuperar apenas as entidades da tabela autor 
		// ao invés das entidades da tabela associativa
		return getAutores().stream()
			.map(AutorLivroVO::getAutor)
			.collect(Collectors.toSet());
	}

	// demais campos e métodos ocultados para brevidade
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

### CAMPOS COM ENUMS

1. **Utilizar EnumType.STRING** A Prodemge padroniza que os campos do tipo Enum sejam mapeados como String. Caráter obrigatório.

Exemplo:

{% tabs %}
{% tab title="Mapeamento" %}
```java
@Enumerated(EnumType.STRING)
@Column(name = "CD_SITUACAO")
public SituacaoEscalaEnum getSituacao() {
	return situacao;
}

```
{% endtab %}

{% tab title="Enum" %}
```java
public enum SituacaoEscalaEnum {

	EXCLUIDA("Excluída"), 
	EXPIRADA("Expirada"), 
	NAO_INICIADA("Não Iniciada"), 
	VIGENTE("Vigente");

	private String descricao;

	private SituacaoEscalaEnum(String descricao) {
		this.descricao = descricao;
	}

	public String getDescricao() {
		return descricao;
	}

	public void setDescricao(String descricao) {
		this.descricao = descricao;
	}
}
```
{% endtab %}
{% endtabs %}

### CAMPOS COM DATAS

1. **Utilizar Date \(java.util.Date\)** Algumas abstrações do framework dependem de que as datas sejam mapeadas com Date.

{% hint style="info" %}
É recomendado que se utilize temporal **TIMESTAMP**. No entanto, não existe uma regra. O importante é manter padronizado e consistente com o que foi escolhido.
{% endhint %}

Exemplo:

```java
@Temporal(TemporalType.TIMESTAMP)
@Column(name = "DT_ATENDIMENTO")
public Date getDataAtendimento() {
	return dataAtendimento;
}
```

### CAMPOS COM BOOLEANOS

1. **Utilizar o prefixo "GET"** Algumas abstrações do framework dependem que os campos mapeados tenham **sempre** o prefixo "get". Caso seja necessário utilizar o prefixo "is" deve-se criar um campo transiente.

Exemplo:

```java
@Column(name = "IS_ENCAIXE")
public Boolean getEncaixe() {
	return encaixe;
}

public void setEncaixe(Boolean encaixe) {
	this.encaixe = encaixe;
}
	
@Transient
public Boolean isEncaixe() {
	return Boolean.TRUE.equals(getEncaixe());
}
```

