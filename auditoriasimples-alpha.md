---
description: Funcionalidade de auditoria simples.
---

# @AuditoriaSimples \(alpha\)

{% tabs %}
{% tab title="Entidade" %}
```java
@AuditoriaSimples(campos = { "Nome:nome", "Supervisor:supervisor.nome", "Lista de Dependentes:dependentes", "Nome do Dependente:dependentes.nome" })
public class FuncionarioVO extends ArenaBaseVO {

    private String nome;
    private String telefone;
    private  FuncionarioVO supervisor;
    private Set<DependenteVO> dependentes = new LinkedHashSet<DependenteVO>(); 
    
    //Getters e Setters ocultados para brevidade do exemplo    
}
```
{% endtab %}

{% tab title="XML" %}
```markup
<grid model="@{classecontrole.auditoria}" mold="paging" pageSize="5">
    <columns>
        <column width="2%" />
        <column width="8%" label="Id" />
        <column label="Data" />
        <column label="Usuário" />
        <column label="Operação" />
    </columns>
    <template name="model">
        <row>
            <label value="${each.id}" />
            <label value="${each.dataOperacaoFormatado}" />
            <label value="${each.usuarioLogado}" />
            <label value="${each.tipoOperacaoFormatado}" />
            <detail open="false">
                <grid>
                    <columns>
                        <column label="Campo" />
                        <column label="Propriedade" />
                        <column label="Valor Antigo" />
                        <column label="Valor Novo" />
                    </columns>
                    <rows>
                        <row forEach="${each.campos}">
                            <label value="${each.labelCampo}" />
                            <label value="${each.nomeCampo}" />
                            <label value="${each.valorAntigo}" />
                            <label value="${each.valorNovo}" />
                        </row>
                    </rows>
                </grid>
            </detail>
        </row>
    </template>
</grid>
```
{% endtab %}

{% tab title="Controller" %}
```java
public class FuncionarioCtr extends ArenaBaseCtr<FuncionarioVO> {

    public List<Object> getAuditoria()  throws Exception {
        List<Object> retorno = new ListModelList<Object>(repositorio().recuperarAuditoriaSimples(getObjetoAtual()));
		return retorno;
    }
    //try-catch ocultados para brevidade do exemplo
}
```
{% endtab %}
{% endtabs %}



