---
date: 2017-07-20T00:11:02+01:00
title: SonarCloud
type: index
weight: 210
---

# (Security|Quality)

[SonarCloud](https://sonarcloud.io/) is a service operated by [SonarSource](http://www.sonarsource.com/), the company that develops and promotes open-source code quality products *SonarQube* and *SonarLint*; SonarSource provides SonarCloud for open source projects, free of charge.

All Foundation projects using Sonar are listed in the [Symphony Software Foundation organisation page](https://sonarcloud.io/organizations/symphonyoss/projects).

There are different ways to enable Sonar in your project, follow the [Getting Started guide](https://about.sonarcloud.io/get-started/) to know more; for Maven projects, the [Foundation Parent POM](https://github.com/symphonyoss/ssf-parent-pom) provides a sonar profile that includes all configurations needed, all projects using this POM as parent can just add -Psonar to the build command; and example is the [Travis build for symphony-java-api project](https://github.com/symphonyoss/symphony-java-api/blob/master/.travis.yml).

When the project configuration is in place, open an INFRA issue to request the Sonar authentication token and access to the SonarCloud configuration (you need to [sign up to SonarCloud.io first](https://github.com/symphonyoss/symphony-java-api/blob/master/.travis.yml))
