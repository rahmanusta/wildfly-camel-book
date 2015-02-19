# Camel Components

This chapter details information about supported camel components

## Aavailable Components 

All the supported Camel components will be available to user deployments for use.  The list of the default components made available to your deployments can found in the `modules/system/layers/fuse/org/apache/camel/component/main/module.xml` file.  

### The jboss-camel-components.properties file

If you need more or fewer components made available to specifc applicaiton deployments, you can add a `META-INF/jboss-camel-components.properties` file your application.  This file should list all the camel components you want to make available to your applicaiton.
Each component should be listed on a new line.  You can identify components by:
 
 * the short camel component names like `camel-jms` 
 * the full module name of the component by prefixing the module name with `module\:`

Example `META-INF/jboss-camel-components.properties` file:

    camel-rss
    module\:org.apache.camel.component.ftp

## Adding Components

Adding support for additional Camel Components is easy

#### Add your modules.xml definition 

A modules.xml descriptor defines the class loading behavior for your component. It should be placed together with the component's jar in `modules/system/layers/fuse/org/apache/camel/component`. Module dependencies should be setup for direct compile time dependencies. 

Here is an example for the camel-ftp component

```xml
<module xmlns="urn:jboss:module:1.1" name="org.apache.camel.component.ftp">
  <resources>
    <resource-root path="camel-ftp-2.14.0.jar" />
  </resources>
  <dependencies>
    <module name="com.jcraft.jsch" />
    <module name="javax.xml.bind.api" />
    <module name="org.apache.camel.core" />
    <module name="org.apache.commons.net" />
  </dependencies>
</module>
```

Please make sure you don't duplicate modules that are already available in WildFly and can be reused.

#### Add a reference to the component 

To make this module visible by default to arbitrary JavaEE deployments add a reference to `modules/system/layers/fuse/org/apache/camel/component/main/module.xml` 

```xml
<module xmlns="urn:jboss:module:1.3" name="org.apache.camel.component">

  <dependencies>
    ...
    <module name="org.apache.camel.component.ftp" export="true" services="export"/>
  </dependencies>

</module>
```





