# Fortify

Common Links from Wiki:

Documenting Fortify errors, see General Guidance (<https://wiki.mobilehealth.va.gov/display/OISSWA/How+to+resolve+scanning+issues+reported+by+Fortify>)

Merging FPR files(<https://wiki.mobilehealth.va.gov/display/OISSWA/How+to+merge+scan+files>)
  
Trusting configuration files/app.properties (<https://wiki.mobilehealth.va.gov/display/OISSWA/How+to+know+if+configuration+files+should+be+trusted>)

Installing maven plugin

```cmd
(Fortify installed directory)\plugins\maven\maven-plugin-bin\install.bat
```

## Steps to get a clean scan

```cmd
clean the fortify scan plugin
set app_version=APP_BUILD_14_01
sourceanalyzer -b %app_version% -clean
mvn com.fortify.sca.plugins.maven:sca-maven-plugin:clean
```

build again

```cmd
mvn clean install
```

set up translation for scan

```cmd
set app_version=APP_BUILD_14_01
sourceanalyzer -b %app_version% -debug-verbose -logfile %app_version%.log -cp "target/**/*.jar" mvn com.fortify.sca.plugins.maven:sca-maven-plugin:translate -Dfortify.sca.exclude="src/test/**/*"
```

delete files if they exist

```cmd
del scan_APP_BUILD_14_01.fpr
del APP_BUILD_14_01.log
```

run scan

```cmd
set app_version=APP_BUILD_14_01
sourceanalyzer -b %app_version% -debug-verbose -logfile %app_version%.log -scan -f scan_%app_version%.fpr -cp "target/**/*.jar"
```

## combined clean, set up & running scan

```cmd
set app_version=APP_BUILD_14_01
mvn com.fortify.sca.plugins.maven:sca-maven-plugin:clean && sourceanalyzer -b %app_version% mvn && sourceanalyzer -b %app_version% -scan -f scan_%app_version%.fpr -debug -logfile log_%app_version%.log
```
