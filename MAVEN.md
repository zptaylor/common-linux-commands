# Maven

## create report of updates

```cmd
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
