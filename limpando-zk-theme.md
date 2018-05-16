# Limpando ZK Theme

No arquivo `zk.xml`, remover os `molds` do arquivo de configuração do projeto.

```markup
<!-- Exemplo de mold a ser removido -->
<library-property>
	<name>org.zkoss.zul.Button.mold</name>
    <value>bs</value>
</library-property>
```

Adicionar a configuração de desativar o tema principal.

```markup
<!-- Adicionar às configurações -->
<desktop-config>
    <disable-theme-uri>~./zul/css/zk.wcs</disable-theme-uri>
</desktop-config>
```

Exemplo da configuração final do **`zk.xml`**

{% code-tabs %}
{% code-tabs-item title="zk.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<zk>
	<device-config>
		<device-type>ajax</device-type>
		<timeout-uri>/index.zul</timeout-uri>
		<!-- An empty URL can cause the browser to reload the same URL -->
	</device-config>
	<system-config>
		<response-charset>UTF-8</response-charset>
		<upload-charset>ISO-8859-1</upload-charset>
		<disable-event-thread>false</disable-event-thread>
		<max-upload-size>81920</max-upload-size>
	</system-config>
	<language-config>
		<addon-uri>/WEB-INF/lang-addon.xml</addon-uri>
	</language-config>
	<system-config>
		<id-generator-class>br.gov.prodigio.visao.helper.ProIdGenarator</id-generator-class>
	</system-config>
	<!-- Configuração necessária para não reaproveitar uuID's de elementos removidos da Árvore DOM. Esse reaproveitamento (reciclagem) gera bugs quando trabalhamos com os uuID's. -->
	<library-property>
		<name>org.zkoss.zk.ui.uuidRecycle.disabled</name>
		<value>true</value>
	</library-property>
	<desktop-config>
    	<disable-theme-uri>~./zul/css/zk.wcs</disable-theme-uri>
	</desktop-config>
</zk>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

No projeto do prodigio, remover os temas do `pom.xml` do pacote `prodigio-web-zk`.

```markup
<!-- ZK theme -->
<dependency>
	<groupId>org.zkoss.theme</groupId>
	<artifactId>sapphire</artifactId>
	<version>${version.zk}</version>
</dependency>
<dependency>
	<groupId>org.zkoss.theme</groupId>
	<artifactId>silvertail</artifactId>
	<version>${version.zk}</version>
</dependency>
<dependency>
	<groupId>org.zkoss.addons</groupId>
	<artifactId>zk-bootstrap</artifactId>
	<version>1.0.0</version>
	<exclusions>
		<exclusion>
			<artifactId>zul</artifactId>
			<groupId>org.zkoss.zk</groupId>
		</exclusion>
		<exclusion>
			<artifactId>zkmax</artifactId>
			<groupId>org.zkoss.zk</groupId>
		</exclusion>
		<exclusion>
			<artifactId>zkex</artifactId>
			<groupId>org.zkoss.zk</groupId>
		</exclusion>
	</exclusions>
</dependency>
```

