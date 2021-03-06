# Atomos
Atomos - A Java Module Framework using OSGi and Equinox

Atomos enables the Equinox OSGi framework implementation to be loaded as a Java Module (as in Java Platform Module System)
on the module path.  When launched this way the built-in Java Module loader is used to load all classes contained
in the bundles on the module path.

# Build

Java 11 must be used to build Atomos. Atomos build and tests require using some SNAPSHOT builds of a few different projects
on git hub.  The following repositories must be cloned and built locally before building Atomos.

- https://github.com/apache/felix - To fix https://issues.apache.org/jira/browse/FELIX-5958 the gogo component needs to be
built to get the latest org.apache.felix.command bundle built
- https://github.com/moditect/moditect.git - To get the latest SNAPSHOT.  This plugin provides some cool utilities for adding
module-infos to existing dependency JARs and building `jlink` images.

Once you have the above built and installed into your local maven `.m2` repository you can then build the Atomos framework with:

`mvn clean install`

This should create a jlink image under `atomos/atomos.tests/service.image/target/atomos`.  Executing the following command
against the jlink image should produce a gogo shell prompt:

`atomos/bin/atomos`

You should see the following output:

```
Registered Echo service from activator.
____________________________
Welcome to Apache Felix Gogo

g! 
```

In order to successfully build a jlink image all bundles included in the image must contain a `module-info.class`,
they cannot be automatic modules.
The `atomos.tests/service.image` example uses the latest `1.0.0-SNAPSHOT` version of the `moditect-maven-plugin` to
add `module-info.class` as necessary to the bundles used in the image.

You can also load additional modules into atomos by using the `atomos.modules` option when launching `atomos`.
For example:

```
atomos/bin/atomos atomos.modules=/path/to/more/modules
```
When doing that the additional modules will be loaded into a child layer where the Atomos OSGi Framework
will control the class loaders.  This will produce a class loader per module bundle installed.  This has
advantages because it allows the module class loader for the bundle to implement the
`org.osgi.framework.BundleReference` interface.

