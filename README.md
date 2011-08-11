# EY Yaml Persister

Spring Yaml properties persister to read AppCloud configuration properties
and expose them as Spring placeholders.

# Maven install

Add dependency:

```xml
<dependency>
  <groupId>com.engineyard.appcloud</groupId>
  <artifactId>yaml-persister</artifactId>
  <version>0.1.0-SNAPSHOT</version>
</dependency>
```

- Note: This artifact is not in any public repository yet, you can download the source code and install it on your local machine with `mvn install`

# Usage

Add the persister to your Spring configuration:

```xml
<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="location">
        <value>file:///data/petclinic/current/config/database.yml</value>
    </property>
    <property name="propertiesPersister" ref="persister"></property>
</bean>
<bean id="persister" class="com.engineyard.appcloud.yaml.YamlPropertiesPersister"></bean>
```

The database file for your app should be located under `/data/YOUR_APP_NAME/current/config/database.yml'

properties exposed:

* ENVIRONMENT.adapter
* ENVIRONMENT.database
* ENVIRONMENT.username
* ENVIRONMENT.password
* ENVIRONMENT.host
* ENVIRONMENT.reconnect

Where ENVIRONMENT is the environment that you chose, for instance `production` or `staging`.

Use those variables to configure you datasource:

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close"
      p:driverClassName="com.mysql.jdbc.Driver" p:url="jdbc:mysql://${production.host}:3306/${production.database}"
      p:username="${production.username}" p:password="${production.password}"/>
```
