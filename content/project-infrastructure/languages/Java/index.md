---
date: 2017-07-20T00:11:02+01:00
title: Java
type: index
weight: 140
---

The Symphony Software Foundation [hosts several Java projects](https://github.com/symphonyoss?language=java) and provides a good level of support for building, testing and releasing Java code; you can also check the [released symphonyoss projects hosted by the Central Repository](http://search.maven.org/#search%7Cga%7C1%7Corg.symphonyoss).

The most adopted (68% in 2016, [according to RebelLabs](http://pages.zeroturnaround.com/RebelLabs-Developer-Productivity-Report-2016.html)) build tool for Java is [Apache Maven](https://maven.apache.org/), which provides a standard, modular, convention-based model to build and publish your Java projects; as such, the Foundation provides a Maven Module called [Foundation Parent POM](https://github.com/symphonyoss/ssf-parent-pom), containing build configurations that can be easily inherited by the single projects; in provides:

1. Plugin configuration for the most common Maven functionalities
2. Nightly-built (in Maven terms, *snapshots*) deployment into [Sonatype OSS public repository](https://oss.sonatype.org/index.html)
3. Release deployment on the Central Repository
4. Rules to validate [Central Repository code requirements](http://central.sonatype.org/pages/ossrh-guide.html#review-requirements)
5. Other integrations for checks and validations

You can also adopt other Java build tools, such as [gradle](https://gradle.org/), [ivy](http://ant.apache.org/ivy/), [ant](http://ant.apache.org/) or others, assuming that they are able to interact with the Central Repository.
Below are listed some of the most common processes that are involved in a Java project lifecycle and how they interact with the project infrastructure provided by the Foundation.

## Build
The build process aims to produce reusable Java binaries (in Maven terms, `artifacts`) in a reliable and reproducible way, from every contributor or consumer workstation; special build requirements should be documented in the project homepage.
The Maven command to build artifacts is `mvn package`

## Artifact publishing
All Java project hosted by the Foundation are expected to use a groupId that is (or prefixed by) `org.symphonyoss` and are published on the Central Repository (also known as Maven Central)
In order to publish artifacts (in Maven terms, *artifact deployment*), it's necessary to setup some accounts and configure the workstation accordingly; please note that these steps are not mandatory for all project teams, but only for those performing artifact deployment and releases (normally the project lead or a team member elected as release manager).

1. Sign up on [issues.sonatype.org](https://issues.sonatype.org) ; using the same credentials (id/password), you can access [oss.sonatype.org](http://oss.sonatype.org)
2. [Create an ODP Enable CI issue](https://symphonyoss.atlassian.net/secure/CreateIssue.jspa?pid=10000&issuetype=10002) following the JIRA Template reported below.
```
h4. Project coordinates
<_github project url and other useful info_>
   
h4. Sonatype JIRA ID
<_The username created on step #1 that grants you access to https://issues.sonatype.org and http://oss.sonatype.org_>
```

3. Locate (or create) and edit the [settings.xml file](https://maven.apache.org/settings.html) in your local file-system to add a `<server>` item; using Maven encrypted passwords is strongly advised, to avoid setting up the password in clear text

```settings.xml
<settings>
  <servers>
    <server>
      <id>ossrh</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
    <server>
      <id>ossrh-distro</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
  </servers>
</settings>
```

At this point, you can proceed with the deployment of the snapshot artifacts, by simply invoking `mvn deploy` from the project root folder; as a result, artifacts will be publicly available on [oss.sonatype.org](https://oss.sonatype.org/index.html) and usable by anyone as Maven dependencies.

You can find more info on the [Central Repository howto page for Maven](http://central.sonatype.org/pages/apache-maven.html#distribution-management-and-authentication).

A badge can be added at the top of the project's root-level `README.md` file, using the following Markdown syntax:

```Maven Central Badge
[![Maven Central](https://img.shields.io/maven-central/v/org.symphonyoss/symphonyoss.svg?maxAge=2592000)](http://search.maven.org/#artifactdetails%7Corg.symphonyoss%7Csymphonyoss%7C2%7Cpom)
```

This will appear as follows and link to the artifact on search.maven.org
[![Maven Central](https://img.shields.io/maven-central/v/org.symphonyoss/symphonyoss.svg?maxAge=2592000)](http://search.maven.org/#artifactdetails%7Corg.symphonyoss%7Csymphonyoss%7C2%7Cpom)

If you want to run this process continuously for each commit, you can [integrate with Travis CI](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/73564181/Continuous+Integration#ContinuousIntegration-ContinuousdeploymentinJava) or other Continuous Integration systems.

## Release

The Maven release process performs the following activities:
1. Verify that there are no uncommitted changes in the local workspace.
2. Prompt the user for the desired tag, release and development version names.
3. Modify and commit release information into the pom.xml file.
4. Tag the entire project source tree with the new tag name.
5. Extract file revisions versioned under the new tag name.
6. Deploy the versioned artifacts to appropriate local and remote repositories.

All the configurations documented in the section above (see snapshot deployment) also apply to the release process; additionally, it is required to:
1. [Install gpg](http://central.sonatype.org/pages/working-with-pgp-signatures.html) on the local workstation where the release process is invoked
2. Configure settings.xml by adding the following profile

```
<settings>
  ...
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg2</gpg.executable>
      </properties>
    </profile>
  </profiles>
</settings>
```

3. Test a snapshot deployment (see above), to make sure that you can authenticate against Sonatype servers

At this point you can ***proceed with the release***:

1. Run the maven command `mvn release:prepare release:perform -Dgpg.passphrase=your_gpg_passphrase`
2. Promote the staged artifacts by accessing the [Nexus staging repositories](http://central.sonatype.org/pages/releasing-the-deployment.html) to
  3. Identify which repository refers to the release process performed on step #1.  Look at the description column to identify the specific project (look towards bottom of list).
  4. Select the staging repository. To release the request, click the `Close` (button on top menu) the release request - this operation will trigger a validation of the artifacts to be released
  5. Click on the refresh button to update status of repository.  Click on `Release` (button on top menu) to sync artifacts with the Central Repository and remove staging repository. 

Upon release, your component will be published to Central: this typically occurs within 10 minutes, though updates to [search](https://search.maven.org/) can take up to two hours.
You can also [watch this youtube video](https://www.youtube.com/watch?v=aO-QFRsYxN4) to know more about the Nexus staging lifecycle.

## Announcements
Using service [Artifact Listener](https://www.artifact-listener.org/), we are currently notifying [dev-notify@symphony.foundation](mailto:dev-notify@symphony.foundation) for every release of the [Foundation projects on the Central Repository](http://search.maven.org/#search%7Cga%7C1%7Corg.symphonyoss); anyone can sign up and customise their notifications.

## Documentation
Maven allows to generate a documentation website in HTML format and provides different options to publish such content on remote servers; content is composed by:
- [Javadocs](http://www.oracle.com/technetwork/articles/java/index-137868.html) - can be easily generated using the [maven-javadoc-plugin](https://maven.apache.org/plugins/maven-javadoc-plugin/), which is already configured to run during the build by the [Foundation Parent Pom](https://github.com/symphonyoss/ssf-parent-pom); the documentation is delivered in HTML format and included in the `target/site/apidocs` sub-folder; to invoke the javadoc generation manually, you can invoke `mvn javadoc:javadoc`
- [Reporting](https://maven.apache.org/plugins/maven-site-plugin/examples/configuring-reports.html) - to collect different metrics from Maven build plugins and publish them as part of the docs
- [Free content](https://maven.apache.org/plugins/maven-site-plugin/examples/creating-content.html) - A free structure of documentation content, which supports different template languages (apt, fml, markdown, xdoc and xhtml)

The [Foundation Parent POM](https://github.com/symphonyoss/ssf-parent-pom) provides a  configuration that allows to publish such content on [symphonyoss.github.io](http://symphonyoss.github.io/) after the site creation phase; to enable it, the build must be invoked using the `-Ppublish-site` Maven profile enabled; for more info, follow the [Foundation Parent Pom documention](https://github.com/symphonyoss/ssf-parent-pom#github-plugins-configuration) and check the [symphony-java-sample-bots project](https://github.com/symphonyoss/symphony-java-sample-bots).