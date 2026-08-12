# Jenkins to GitHub Actions migration report

## Source and destination

The declarative Docker-agent pipeline formerly in `Jenkinsfile` is archived as
[`Jenkinsfile`](Jenkinsfile). Its GitHub Actions equivalent is
[`../workflows/maven.yml`](../workflows/maven.yml).

## Behavior mapping

| Jenkins behavior | GitHub Actions implementation |
| --- | --- |
| `maven:3.5.0-jdk-8` Docker agent | Job container using the same image |
| `MAVEN_OPTS=-Xmx2048m` | Job environment variable |
| Build, test, and package shell steps | Same Maven commands in order |
| Always publish Surefire XML | `actions/upload-artifact` with `if: always()` |
| Archive JAR files | `actions/upload-artifact` after packaging |

The Jenkins pipeline defined no trigger, so the workflow is intentionally
manual (`workflow_dispatch`). The unused Jenkins Docker bind mount and port
mapping are not needed by any pipeline command and have no workflow
equivalent.

## Actions and security

`actions/checkout` and `actions/upload-artifact` are pinned to immutable
commit SHAs. The workflow grants only read access to repository contents.

## Secrets and variables

No Jenkins credential bindings or secrets were present. No GitHub secrets or
additional repository variables are required.
