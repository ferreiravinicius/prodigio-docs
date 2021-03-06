# Auditoria Simples

### Sobre

Cria auditoria simples de uma entidade específica. É criado uma entrada na tabela de auditoria para cada operação do tipo inclusão, alteração ou exclusão de registro. Cada campo anotado que tenha sofrido qualquer alteração é associado com seu respectivo objeto de auditoria. 

### Funcionamento

Para auditar uma entidade é necessário anotar a classe com `@AuditoriaSimples` e passar como parâmetro os campos a serem auditados com seus respectivos _labels._  

Ao declarar os campos a serem auditados utilize a notação `propriedade:label`.   
Em  `label` definimos um identificador para o campo, caso não seja especificado o mesmo será inferido com o mesmo nome da `propriedade`.   
Em `propriedade` declaramos o nome da propriedade existente na entidade que irá ser auditada. 

{% tabs %}
{% tab title="Entidade" %}
```java
@AuditoriaSimples(campos = { "nome:Nome", "supervisor.nome:Supervisor", "dependentes:Lista de Dependentes", "dependentes.nome:Nomes dos Dependentes:" })
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
Se a propriedade referenciar um objeto de outra entidade \(VO\) ou uma coleção, é utilizado o `toString()` do objeto na comparação e ao persistir no banco.
{% endhint %}

{% hint style="info" %}
É possível declarar propriedades de objetos aninhados em vários níveis, utilizando a notação `objeto.bjetoAninhado.propriedade`.
{% endhint %}

### Métodos

#### AuditoriaSimplesVO

| **Método** | **Retorno** |  |
| :--- | :--- | :--- |
|  `getId()` | `Long` |  Recupera o `Id` referente ao objeto da auditoria em si. |
| `getCampos()` | `Set<CampoAuditadoVO>` | Recupera a lista de campos alterados. |
| `getDataOperacao()` | `Date` | Recupera a data da auditoria. |
| `getEntidade()` | `String` | Recupera o nome completo da classe da entidade sendo auditada. |
| `getObjetoId()` | `Long` | Recupera o `Id` referente ao objeto da entidade que foi auditado. |
| `getUsuarioLogado()` | `String` | Recupera o identificador do usuário que efetuou a operação \(crud\). |
| `getTipoOperacao()` | `Character` | Recupera o caractere respectivo ao tipo da operação que iniciou a auditoria. Onde `A` representa alteração, `I` inclusão e `E` exclusão. |

#### CampoAuditadoVO

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>M&#xE9;todo</b>
      </th>
      <th style="text-align:left"><b>Retorno</b>
      </th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>getId()</code>
      </td>
      <td style="text-align:left"><code>Long</code>
      </td>
      <td style="text-align:left">Recupera o <code>Id</code> referente ao objeto do campo auditado em si.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getLabelCampo()</code>
      </td>
      <td style="text-align:left"><code>String</code>
      </td>
      <td style="text-align:left">Recupera o <code>label</code> associado ao campo auditado (declarado em <code>campos</code> na
        anota&#xE7;&#xE3;o).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getNomeCampo()</code>
      </td>
      <td style="text-align:left"><code>String</code>
      </td>
      <td style="text-align:left">Recupera o nome do campo/propriedade que foi auditada (declarado em <code>campos</code> na
        anota&#xE7;&#xE3;o).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getValorAntigo()</code>
      </td>
      <td style="text-align:left"><code>String</code>
      </td>
      <td style="text-align:left">Recupera o valor do campo antes da altera&#xE7;&#xE3;o, se houver.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getValorNovo()</code>
      </td>
      <td style="text-align:left"><code>String</code>
      </td>
      <td style="text-align:left">Recupera o valor do campo ap&#xF3;s a altera&#xE7;&#xE3;o, se houver.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getEntidadePai()</code>
      </td>
      <td style="text-align:left"><code>String</code>
      </td>
      <td style="text-align:left">
        <p>Recupera o nome completo da classe referente ao objeto ao qual o campo/propriedade
          auditada pertence.</p>
        <p></p>
        <p>Exemplo: para <code>objetoAninhado.outroObjeto.propriedade</code> retornaria
          a classe respectiva ao <code>outroObjeto</code>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getIdObjetoPai()</code>
      </td>
      <td style="text-align:left"><code>Long</code>
      </td>
      <td style="text-align:left">
        <p>Recupera o <code>Id</code> referente ao objeto ao qual o campo/propriedade
          pertence.</p>
        <p></p>
        <p>Exemplo: para <code>objetoAninhado.outroObjeto.propriedade</code> retornaria
          o <code>Id</code> respectivo ao<code>outroObjeto</code>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>getAuditoria()</code>
      </td>
      <td style="text-align:left"><code>AuditoriaSimplesVO</code>
      </td>
      <td style="text-align:left">Recupera o objeto de auditoria no qual o campo auditado esta associado.</td>
    </tr>
  </tbody>
</table>

