# Sonarqube plugin for Sellcheck
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=emerald-squad_sonar-shellcheck-plugin&metric=alert_status)](https://sonarcloud.io/dashboard?id=emerald-squad_sonar-shellcheck-plugin)

This plugin will add the shell language to your Sonarqube installation, with the followinf benefits :

- Shell scripts (bash and others) are added to your project and counted as valid source files
- [The shellcheck tool](https://github.com/koalaman/shellcheck) will be run on your shell files, giving you meaningfull hindsight about your scripts quality

## Installation

Just copy the jar file to your `$SONAR_HOME/extensions/plugins`

## Configuration

- *Shell suffixes* : The suffixes (separated by commas) of the files that are to be considered shell scripts. Default value : `.sh`

## Behavior

This plugin will download a specific version of shellcheck, extract it and run it on the machine that runs the sonarqube scanner.

If this is unaceptable for any reason, you can run shellcheck yourself before starting sonar scanner. To do so, you must run shellcheck
from your project root directory and save a json-formatted report to a file, as so :

```bash
shellcheck -f json [path/to/file1] [path/to/file2] ... [path/to/fileN] > shellcheck-report.json
```

Then, you must include the report in your `sonar-project.properties` file:

```
sonar.shellcheck.reportPath=shellcheck-report.json
```

## Usage behind a proxy

Since the default behavior is to download shellcheck from the internet, you may need to configure a proxy with sonar-scanner. To do so, you can use the `SONAR_SCANNER_OPTS`environment variable to set proxy settings in the JVM.

```bash
export SONAR_SCANNER_OPTS="-Dhttps.proxyHost=<host> -Dhttps.proxyPort=<port>"
```

## Credits

All the magic is done by [the shellcheck tool](https://github.com/koalaman/shellcheck). This plugin only imports shellcheck repports into sonarqube.

Furthermore, all issues documentations presented in the sonarqube interface has been shamelessly "borrowed" from [shellcheck's wiki](https://github.com/koalaman/shellcheck/wiki).

## Caveats

Sonarqube will ignore any file that doesn't have an extension. So your shell scripts must have an extension to be correctly analysed.

