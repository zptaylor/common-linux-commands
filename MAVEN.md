# Maven

## create report of updates

```cmd
mvn versions:dependency-updates-report

mvn versions:display-dependency-updates

mvn versions:display-plugin-updates

mvn versions:plugin-updates-report

mvn versions:property-updates-report
```

## update pom.xml versions

```cmd
mvn versions:use-latest-releases

mvn versions:use-latest-versions
```

## create report of dependency convergences

```cmd
mvn project-info-reports:dependency-convergence
```

## manually install jars in the repo (example is VistALink 1.6.1.010)

```cmd
mvn install:install-file -DgroupId=gov.va.med.vistalink -DartifactId=vljConnector -Dpackaging=jar -Dversion=1.6.1.010 -Dfile=vljConnector-1.6.1.010.jar
mvn install:install-file -DgroupId=gov.va.med.vistalink -DartifactId=vljFoundationsLib -Dpackaging=jar -Dversion=1.6.1.010 -Dfile=vljFoundationsLib-1.6.1.010.jar
mvn install:install-file -DgroupId=gov.va.med.vistalink -DartifactId=vljSecurity -Dpackaging=jar -Dversion=1.6.1.010 -Dfile=vljSecurity-1.6.1.010.jar

```
