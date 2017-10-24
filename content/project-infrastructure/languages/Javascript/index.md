---
date: 2017-07-20T00:11:02+01:00
title: Javascript
type: index
weight: 150
---

In the last decade, Javascript (JS) have been increasingly used for backend software development, especially since the first appearance of NodeJS; below are listed - for each JS eco-system/platform - some of the most common processes that are involved in a Javascript project lifecycle and how they interact with the project infrastructure provided by the Foundation.

You can browse the Foundation hosted projects that use [javascript](https://github.com/symphonyoss?language=javascript) and [coffescript](https://github.com/symphonyoss?language=coffeescript), or check the NodeJS released packages on [symphonyoss profile](https://www.npmjs.com/~symphonyoss).

## NodeJS

NodeJS is a (widely adopted) JavaScript runtime which allows to run JS code; the node command line is easy to install and provides a rich set of options to manage the entire project lifecycle.

### Publishing on npm

!! An incubating project is allowed to deploy releases while incubating, provided version numbers are < 1.0.0.  Only active projects are entitled to label themselves with versions 1.0.0 and above.

**[npm](https://docs.npmjs.com/)** is the NodeJS package manager, enabling developers to fetch and publish nodeJS packages; the Foundation provides:
A [Foundation npm organisation (symphonyoss)](https://www.npmjs.com/org/symphonyoss) that lists all Foundation packages published on npm
A [Foundation npm user (ssf-admin)](https://www.npmjs.com/~ssf-admin) that can be used via CI environments to publish packages on npm
Project members can use their own npm users to publish packages (locally or via CI), assuming that the package is listed in the symphonyoss npm organisation and the npm user is a member of it; you can open an [ODP Enable CI](https://symphonyoss.atlassian.net/secure/CreateIssue.jspa?pid=10400&issuetype=10500) issue to the Foundation that will take care of it, see issue template below.
```
h4. Project coordinates
<_github project url and other useful info_>
 
h4. NPM user
<NPM user ID(s) that the project member(s) use to publish the package>
  
h4. Previously published npms
<_Links to previously published npms on npmjs.org (or any other publicly available repository_>
```

Packages can be published using
- [username/password authentication](https://docs.npmjs.com/getting-started/publishing-npm-packages)
- NPM tokens, best option for CI environments; you can use [get-npm-token](https://www.npmjs.com/package/get-npm-token) to generate and register a token; alternatively you can use [semantic-release](https://github.com/semantic-release/semantic-release) to automate the entire release process using Travis CI; read more on the [Continuous Integration page](/continuous-integration)

### Using package scoping
All npm packages released under the Symphony Software Foundation should define the `@symphonyoss` scope, in order to point to the npm organisation; if you're not familiar, [read more about scoped packages](https://docs.npmjs.com/misc/scope).

There are some situations where it is not possible to specify the scope of a package, for example when [building a hubot adapter](https://github.com/symphonyoss/hubot-symphony/issues/53); in that case, it is important to use ssf-admin as the npm user that performs the package publishing (see above).