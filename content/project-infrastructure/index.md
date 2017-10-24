---
date: 2017-07-20T00:11:02+01:00
title: Project infrastructure
type: index
weight: 70
---

The Symphony Software Foundation provides a service-based infrastructure to support committers throughout the entire [project lifecycle](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/3211338/Project+Lifecycle).

This page helps project leads (or any other project member) to setup a project pipeline for software development following the team's preferences and leveraging the infrastructure provided by the Foundation.

## Contribution
When the code transfer is completed, a new project will be publicly hosted in the [Foundation’s Github Organisation](https://github.com/symphonyoss), [granting write access to project team members....]

### Source Code Management (SCM)
To know more about Github, you can browse through [their list of guides](https://guides.github.com/).

### Issue tracking
Managing issues (or tickets) across a team is a very important project activity, especially at the beginning; the Foundation strongly advises to choose and setup an issue tracking system as soon as the [project is transferred](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/62783509/Contribution#Contribution-CodeTransfer).

**Github Issues** is available for all hosted projects; alternatively, the project team can request the Foundation to host the project on the [JIRA Foundation instance](https://symphonyoss.atlassian.net/issues), by simply creating an [INFRA Task issue](https://symphonyoss.atlassian.net/secure/CreateIssue.jspa?pid=10000&issuetype=10002).

Additional configuration tasks for issue tracking include:

- Define a taxonomy of issue labels (i.e. Bug, Enhancement, Question, Task, ...)
- Define an initial list of milestones

Follow the [Mastering Issues guide](https://guides.github.com/features/issues/) to get started in 10 minutes.

## Incubating (and beyond)
When setting up the software development process, there are different common configuration steps that the team need to take care of; below are listed the most important ones, linking to the specific pages that describe the infrastructure configuration depending on the language and eco-system of choice.

### Building and testing
The build is an end-to-end process that converts source code into reusable artifacts, something that we will refer to as **deployable units**, which is developed by the project team and hosted in the github repository. It is a particularly important task, as it can centralise and trigger several automated sub-tasks, such as version control, code testing, quality and compliance reports and more.

A working build process is key to implement more automated processes, such as release, [Continuous Integration]() and automated deployments; it is also a requirement for [project activation](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/62783520/Activation#Activation-RequirementsforActivation).

To know more about build configuration, check the [Languages page]().

### Version Control
Version control is the process to establish a format to a project version and the rules to update it, preferably integrating with automated build and release systems; the Foundation mandates the user of [Semantic Versioning](http://semver.org/) ("semver") throughout a project's lifecycle:
- for **incubating projects**, it is allowed to deploy releases with version numbers ***< 1.0.0***
- for **released projects**, it is allowed to deploy releases with version ***1.0.0 and above***

Every project team is encouraged to define more specific MAJOR, MINOR and PATCH definitions, in order to provide a better understanding for consumers on what to expect when adopted and to follow language-specific best practice (ie the [Version Identification and Dependency Specification Guide](https://www.python.org/dev/peps/pep-0440/) for Python projects)

### Documentation
As part of the activation process, the Foundation requires several documentation items, to make sure that the release of an active project complies with high documentation standards.
There are different ways documentation can be written and published; the easiest (and most visible), is to use Markdown files hosted on Github (i.e. README.md); additionally, you can
- Use [Github Wiki](https://help.github.com/articles/about-github-wikis/)
- Use Foundation's [Github Pages](https://pages.github.com/), hosted on `http://symphonyoss.github.io/<project_name>`; for more info on how to configure it, checkout [Github help](https://help.github.com/articles/user-organization-and-project-pages/)
- Use the [Foundation instance of Atlassian Confluence](https://symphonyoss.atlassian.net/wiki); you can request a dedicated Confluence space by [opening a Github issue]().

Some languages or build systems provide support for automated documentation publishing; browse the [Languages page]() to know more about automated configurations for a given language.

### Release
The release process allows to publish deployable units into a publicly available artifact repository, by invoking the build process and applying version control to increment the project's version; the Foundation collects guides and best practices on how to release a project, depending on the language and eco-system of choice; browse the [Languages page]() to know more about automated configurations for a given language.

### Continuous Integration (CI)
CI allows to automatically verify every code change that allows to detect problems earlier ([read more here](https://www.thoughtworks.com/continuous-integration)); the Foundation facilitates the [configuration of CI systems](), depending of the language and eco-system of choice.

The Foundation does not mandate CI at any project phase, though it strongly advises to investigate automating any process that can improve software quality, security and compliance.

### Reporting
In order to measure and report the level of quality, security and legal compliance, the Foundation facilitates the integration of build processes with external systems specialised in code analysis; the [Reporting page]() also guides a project team on how to produce and publish measurements that are required by project activation.
