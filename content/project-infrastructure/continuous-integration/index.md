---
date: 2017-07-20T00:11:02+01:00
title: Continuous Integration
type: index
weight: 80
---

Continuous Integration (CI) allows to run different type of processes to identify problems earlier and offload developers with manual processes to update collateral resources, like test results, documentation, binaries and metrics; each language and eco-system needs a specific configuration.

## Travis CI
Travis CI is a hosted, distributed continuous integration service used to build and test software projects hosted at GitHub.
To enable Travis CI on a Foundation hosted project:
1. [Sign in to Travis CI](https://travis-ci.org/) with your Github credentials
2. Access the [Travis CI profile](https://travis-ci.org/profile/symphonyoss) and sync your project
3. Drop a `.travis.yml` file in the root folder of your github repo (check [this .travis.yml](https://github.com/symphonyoss/ssf-parent-pom/blob/master/.travis.yml) as example)
  4. (optional) If you need to perform variable encryption and other deployment commands, install the [Travis CI Client](https://github.com/travis-ci/travis.rb) using `gem install travis`
5. Access `https://travis-ci.org/symphonyoss/<repository-name>` and validate the build

Please check the [Travis supported languages](https://docs.travis-ci.com/user/languages/) and how to [customize your build](https://docs.travis-ci.com/user/customizing-the-build) depending on the project's needs.

A badge can be added at the top of the project's root-level `README.md` file, using the following Markdown syntax:
```
[![Build Status](https://travis-ci.org/symphonyoss/symphony-java-sample-bots.svg)](https://travis-ci.org/symphonyoss/symphony-java-sample-bots)
```

### Continuous (artifact) deployment in Java
When using Java, Travis CI can be easily configured to publish new snapshot artifacts on commit; to enable this feature, a project committer can follow these simple steps:

1. Follow the Java Snapshot deployment configuration ; as a result, you should have username/password credentials (mentioned below as `CI_DEPLOY_USERNAME` and `CI_DEPLOY_PASSWORD`) to access [issues.sonatype.org](issues.sonatype.org)
2. [Open a Github issue]() to grant permission to deploy artifacts having groupId `org.symphonyoss` on Maven remote repositories; this action is not required if the user has already granted access
3. Add a `settings.xml` file in the project root folder, defined as follows:
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>ossrh</id>
      <username>${env.CI_DEPLOY_USERNAME}</username>
      <password>${env.CI_DEPLOY_PASSWORD}</password>
    </server>
  </servers>
</settings>
```
4. Define `CI_DEPLOY_USERNAME` and `CI_DEPLOY_PASSWORD` [variables with Travis CI](https://docs.travis-ci.com/user/environment-variables/); make sure that they're encrypted and hidden during the build process; the credentials to use are the ones defined on step 1; if you don't want to share your username/password credentials, you can [request and use Nexus tokens](https://books.sonatype.com/nexus-book/reference/usertoken.html)
5. Change the `mvn` build command to
  6. Invoke the `deploy` goal
  7. Append `--settings settings.xml` at the end of the build command

[This tutorial](https://coderwall.com/p/9b_lfq/deploying-maven-artifacts-from-travis) walks through the mentioned steps, with more details and configuration options; a working example of this configuration is the [symphony-java-sample-bots project](https://github.com/symphonyoss/symphony-java-sample-bots/blob/dev/.travis.yml)

### Continuous (artifact) deployment in NodeJS
Travis CI can easily publish packages to npm using [semantic-release.org](http://semantic-release.org/), which delegates release operations to your CI environment and allows you to control versioning [using commit messages (commitizen)](https://www.npmjs.com/package/commitizen); as a result, npm packages will be continuously released on each code repository change.

Follow the [instructions on the Javascript language page]() to register a package and user in the [Foundation npm organisation](https://www.npmjs.com/org/symphonyoss).

### Continuous (artifact) deployment in Python
Travis CI can be configured to use [python-semantic-release](http://python-semantic-release.readthedocs.io/en/latest/), a package that simplifies and automates versioning for python projects; a project committer can [open an ODP Enable CI issue]() to request the Foundation staff to apply the proper Travis CI project settings; packages will be published on behalf of symphonyoss [pypi](https://pypi.python.org/) user.

### Integration testing
TODO - this is specific for Symphony, so it should be moved into Technologies

```Download certificate before the build script runs
before_script: "curl -s https://raw.githubusercontent.com/symphonyoss/contrib-toolbox/master/scripts/download-files.sh | bash"
```

```Define environment variables with Symphony Pod endpoints
env:
 global:
  - FOUNDATION_API_URL=https://foundation-dev-api.symphony.com
  - FOUNDATION_POD_URL=https://foundation-dev.symphony.com
  - SESSIONAUTH_URL=$FOUNDATION_API_URL/sessionauth
  - KEYAUTH_URL=$FOUNDATION_API_URL/keyauth
  - POD_URL=$FOUNDATION_POD_URL/pod
  - AGENT_URL=$FOUNDATION_POD_URL/agent
```

At this point, the certificates are in place and downloaded into the build box; integration tests can resolve User Identity certificates by accessing environment variables.

#### Example

The [symphony-java-sample-bots](https://github.com/symphonyoss/symphony-java-sample-bots/tree/dev) is the first project that was enabled to run integration tests from Travis CI; the [Travis build logs](https://travis-ci.org/symphonyoss/symphony-java-sample-bots/branches) show two interesting items:
1. The (hidden) environment variables that identify User Identity certificates (in this case 2, one for the message sender, one for the message receiver)

```Travis environment variables
Setting environment variables from repository settings
$ export DOWNLOAD_PATH=[secure]
$ export DOWNLOAD_ITEMS=[secure]
$ export TRUSTSTORE_FILE=[secure]
$ export TRUSTSTORE_PASSWORD=[secure]
$ export BOT_USER_EMAIL=[secure]
$ export BOT_USER_CERT_FILE=[secure]
$ export SENDER_USER_EMAIL=[secure]
$ export SENDER_USER_CERT_FILE=[secure]
$ export DOWNLOAD_HOST=[secure]
$ export SENDER_USER_CERT_PASSWORD=[secure]
$ export BOT_USER_CERT_PASSWORD=[secure]
```

2. The integration test logs of a successful run

```Maven Surefire plugin log
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running org.symphonyoss.simplebot.EchoBotIT
chat initialised
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.132 sec - in org.symphonyoss.simplebot.EchoBotIT
 
Results :
 
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
```

#### Known certificated issues
```
Caused by: javax.crypto.BadPaddingException: Given final block not properly padded at
org.symphonyoss.simplebot.EchoBotIT.sendAndReceiveEcho(EchoBotIT.java:64)
```

This basically means that the [certificate password is wrong](http://stackoverflow.com/questions/8049872/given-final-block-not-properly-padded); to validate the certificate using the following command:
```
openssl pkcs12 -info -nodes \
-in <certificate_path> \
-passin pass:<certificate_password>
```

*Reminder for Foundation staff: every password symbol must be escaped by prefixing it with character `\`, before setting it as Travis environment variable.*

### Branch specific build commands
Travis CI does not provide a syntax to specify branch-specific build commands; however, it is possible to use the following `.travis.yml` syntax to work around it; in this example below, different Maven commands are used depending on the branch used and whether the trigger was a Pull Request or not.

```
script:
- "[[  \"$TRAVIS_PULL_REQUEST\" = \"false\" && $TRAVIS_BRANCH =~ master ]] && mvn clean deploy -Pintegration-testing,versioneye --settings settings.xml || true"
- "[[  \"$TRAVIS_PULL_REQUEST\" = \"false\" && $TRAVIS_BRANCH =~ dev ]] && mvn clean deploy -Pintegration-testing --settings settings.xml || true"
- "if [[ \"$TRAVIS_PULL_REQUEST\" != \"false\" ]]; then mvn clean install ; fi"
```

## MyGet
[Myget](http://myget.org/) is a friction-free continuous integration & delivery for your nuget, npm, bower and vsix packages; the Foundation provides [**symphonyoss** package repository](https://www.myget.org/gallery/symphonyoss), which allows:
1. **Pre-release build and publishing**, using build environments that are equipped with .NET Framework and Visual Studio (headless); it also features source code tagging and version updates on source code based on incremental build number, to fully automate the package publishing pipeline
2. **Sync with NuGet**, that can be either manual (promoting pre-releases to releases using Myget web interface) or automatic; a committer must create a personal account on MyGet and communicate it to the Foundation (see JIRA Template below) in order to be granted rights to push feed packages
In order to request a project to be integrated with MyGet, a project committer can sign up to MyGet and [open a Github issue]() following the template below.

```
h4. Project coordinates
<_github project url and other useful info_>
 
h4. MyGet username
<_the MyGet username to add permission to symphonyoss feed_>
 
h4. CSProj and CS files
<_Where project descriptors are located_>
 
h4. Packages
<_A list of the packages that are published; by default all packages are taken into account_>
 
h4. Publishing strategy to NuGet
<_ Whether to enable automatic publishing based on source code branching or rely on manual package pushing using the MyGet interface; if the latter, a MyGet personal account username must be provided _>
```

An example of project building with MyGet is [RestApiClient](https://symphonyoss.atlassian.net/wiki/github.com/symphonyoss/RestApiClient), which is also published on the [`ssf-restapiclieny` MyGet](https://www.myget.org/feed/ssf-restapiclient/package/nuget/SymphonyOSS.RestApiClient) and [NuGet](https://www.nuget.org/packages/SymphonyOSS.RestApiClient/) feeds.

Badges can be added at the top of the project's root-level `README.md` file, using the following Markdown syntax:
1. Build status
[![MyGet Build Status](https://www.myget.org/BuildSource/Badge/ssf-restapiclient?identifier=b0a0940f-c25c-4070-a127-6cccf74ae5ab)](https://www.myget.org/feed/ssf-restapiclient/package/nuget/SymphonyOSS.RestApiClient)
```
[![MyGet Build Status](https://www.myget.org/BuildSource/Badge/ssf-restapiclient?identifier=b0a0940f-c25c-4070-a127-6cccf74ae5ab)](https://www.myget.org/feed/ssf-restapiclient/package/nuget/SymphonyOSS.RestApiClient)
```

2. Latest Pre-Release package published
[![MyGet Pre Release](https://img.shields.io/myget/ssf-restapiclient/v/SymphonyOSS.RestApiClient.svg)](https://www.myget.org/feed/ssf-restapiclient/package/nuget/SymphonyOSS.RestApiClient)
```
[![MyGet Pre Release](https://img.shields.io/myget/ssf-restapiclient/v/SymphonyOSS.RestApiClient.svg)](https://www.myget.org/feed/ssf-restapiclient/package/nuget/SymphonyOSS.RestApiClient)
```

## Integration testing against the Foundation Developer Platform
A Continuous Integration environment can be integrated to run integration tests against the [Symphony Foundation Developer Pod](/technologies#symphony), in order to provide an automated end-to-end testing process.

### Requesting CI integration
Any committer can [open a Github issue]() to request integrating CI with a hosted project, by providing some mandatory info, please follow the template below.
```
h4. Project coordinates
<_github project url and other useful info_>
 
h4. Integration tests
<_List and describe (what to expect, how long it takes to run, the integration tests that you want to run in a CI environment_>
 
h4. Local run
<_Describe how to setup a local environment and run the integration tests_>
 
h4. Environment variables
<_List all environment variables needed to run the integration tests_>
 
h4. Current environment
<_Describe the current build environment, if any_>
```

The Foundation staff will configure all build variables that are needed to download the User Identity certificates (.p12 files) and run the integration tests.