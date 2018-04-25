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

{% tab title="XML \(Opção 2\)" %}
```markup
<listbox model="@{classecontrole.auditoria}">
    <listhead>
        <listheader label="Id" />
        <listheader label="Data" />
        <listheader label="Usuário" />
        <listheader label="Operação" />
        <listheader label="Campos Alterados" width="10%" />
    </listhead>
    <listitem self="@{each=item}" value="@{item}">
        <listcell label="@{item.id}" />
        <listcell label="@{item.dataOperacaoFormatado} " />
        <listcell label="@{item.usuarioLogado}" />
        <listcell label="@{item.tipoOperacaoFormatado}" />
        <listcell>
            <button label="Exibir" onClick="((Popup)self.getNextSibling()).open(self)"></button>
            <popup id="tblcampos" width="80%">
                <listbox model="@{item.campos}">
                    <listhead>
                        <listheader label="Campo" />
                        <listheader label="Propriedade" />
                        <listheader label="Valor Antigo" />
                        <listheader label="Valor Novo" />
                    </listhead>
                    <listitem self="@{each=campo}" value="@{campo}">
                        <listcell label="@{campo.labelCampo}" />
                        <listcell label="@{campo.nomeCampo}" />
                        <listcell label="@{campo.valorAntigo}" />
                        <listcell label="@{campo.valorNovo}" />
                    </listitem>
                </listbox>
                <button label="Fechar" onClick="((Popup)self.getParent()).close()" />
            </popup>
        </listcell>
    </listitem>
</listbox>
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



