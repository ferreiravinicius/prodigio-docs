---
description: Funcionalidade de auditoria simples.
---

# @AuditoriaSimples \(alpha\)

Para auditar uma entidade é necessário anotar a classe com `@AuditoriaSimples` e passar como parâmetro os campos a serem auditados com seus respectivos _labels. _ 

Ao declarar os campos a serem auditados utilize a notação `label:propriedade`.   
Em  `label` definimos um identificador para o campo, caso não seja especificado o mesmo será inferido com o mesmo nome da `propriedade`.   
Em `propriedade` declaramos o nome da propriedade existente na entidade que irá ser auditada. 

{% tabs %}
{% tab title="Entidade" %}
```java
@AuditoriaSimples(campos = { "Nome:nome", "Supervisor:supervisor.nome", "Lista de Dependentes:dependentes", "Nome do Dependente:dependentes.nome" })
public class FuncionarioVO extends ArenaBaseVO {

    private String nome;
    private String telefone;
    private FuncionarioVO supervisor;
    private Set<DependenteVO> dependentes = new LinkedHashSet<DependenteVO>(); 
    
    //Getters e Setters ocultados para brevidade do exemplo    
}
```
{% endtab %}

{% tab title="Controller" %}
```java
public class FuncionarioCtr extends ArenaBaseCtr<FuncionarioVO> {

    public Set<?> getAuditoria()  throws Exception {
        return repositorio().recuperarAuditoriaSimples(getObjetoAtual());
    }
    //try-catch ocultados para brevidade do exemplo
}
```
{% endtab %}

{% tab title="XML \(Opção 1\)" %}
```markup
<grid model="@{classecontrole.auditoria}" mold="paging" pageSize="5">
    <columns>
        <column width="2%" />
        <column width="8%" label="Id" />
        <column label="Data" />
        <column label="Usuário" />
        <column label="Operação" />
    </columns>
    <rows>
        <row self="@{each=aud}" value="@{aud}">
            <label value="@{aud.id}" />
            <label value="@{aud.dataOperacaoFormatado}" />
            <label value="@{aud.usuarioLogado}" />
            <label value="@{aud.tipoOperacaoFormatado}" />
            <detail open="false">
                <grid model="@{aud.campos}">
                    <columns>
                        <column label="Campo" />
                        <column label="Propriedade" />
                        <column label="Valor Antigo" />
                        <column label="Valor Novo" />
                    </columns>
                    <rows>
                        <row self="@{each=campo}" value="@{campo}">
                            <label value="@{campo.labelCampo}" />
                            <label value="@{campo.nomeCampo}" />
                            <label value="@{campo.valorAntigo}" />
                            <label value="@{campo.valorNovo}" />
                        </row>
                    </rows>
                </grid>
            </detail>
        </row>
    </rows>
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
{% endtabs %}

{% hint style="info" %}
Para as propriedades que referenciam outros objetos não primitivos ou coleções é considerado o `toString()` do mesmo. Também é possível declarar propriedades de objetos aninhados, utilizando a notação `objetoAninhado.propriedade`.
{% endhint %}

