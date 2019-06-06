# Mapeando Entidades

### CAMPOS COM COLEÇÕES

1. **Utilizar Set** Evita [MultipleBagFetchException](https://thoughts-on-java.org/hibernate-tips-how-to-avoid-hibernates-multiplebagfetchexception/) e algumas abstrações do framework dependem da implementação do Set. 
2. **Inicializar as coleções na declaração** O framework utliza "Reflexão" para recuperar os campos, logo precisamos das coleções inicializadas para saber qual classe esta associada à coleção. 
3. **Anotações com parâmetros obrigatórios** 
   1. **FetchType**  Utilizar **LAZY** sempre que possível para buscar informações sob demanda e evitar problemas de performance. 
   2. **Cascade** Utilizar **ALL**, **MERGE** ou **PERSIST**. O framework  só inicializa as listas sob demanda, de forma automatica,  quando estão anotadas com um desses tipos de _cascade._  
   3. **MappedBy** Necessário especificar o nome do campo na classe filho que referencia o parente \(anotado no filho como @ManyTo\*\). 

Exemplo:

```java
// (1) set
private Set<FilhoVO> filhos = new LinkedHashSet<>(); // (2) inicializado

@OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.ALL, mappedBy = "nomeAtributoParente", orphanRemoval = true) // (3) anotação parametrizada
public Set<FilhoVO> getFilhos() {
	return filhos;
}

public void setFilhos(Set<FilhoVO> filhos) {
	this.filhos = filhos;
}
```



### CAMPOS BOOLEANOS

1. **Utilizar o prefixo "GET"** Algumas funcionalidades do framework dependem que os campos mapeados tenham **sempre** o prefixo "get". Caso seja necessário utilizar o prefixo "is" deve-se criar um campo transiente.

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

