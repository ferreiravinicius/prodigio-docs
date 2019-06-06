# Mapeando Entidades

### CAMPOS COM COLEÇÕES

1. **Utilizar Set \(java.util.Set\)** [Evita MultipleBagFetchException](https://thoughts-on-java.org/hibernate-tips-how-to-avoid-hibernates-multiplebagfetchexception/) e algumas funcionalidades do framework dependem da implementação do Set. 
2. **Inicializar as coleções na declaração** O framework utliza "Reflexão" para recuperar os campos, logo precisamos das coleções inicializadas sempre para que seja possível recuperar o tipo de retorno das mesmas. 
3. **Parametrizar anotação com FetchType, MappedBy e Cascade**  
    a. 

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

