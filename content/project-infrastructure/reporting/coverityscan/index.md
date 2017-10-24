---
date: 2017-07-20T00:11:02+01:00
title: Coverity Scan
type: index
weight: 190
---

# (Security|Quality)

[Coverity Scan](https://scan.coverity.com/) service was initiated with the U.S. Department of Homeland Security as the largest public-private sector research project in the world, focused on open source software quality and security; it is now a free service to the open source community; you can sign up with your Github credentials and request access to a project's dashboard configuration [opening an INFRA issue](https://symphonyoss.atlassian.net/browse/INFRA); everytime that a scan is performed, Coverity sends a report to [dev@symphony.foundation](mailto:dev@symphony.foundation).

You can [browse the SSF projects](https://scan.coverity.com/projects?utf8=%E2%9C%93&search=symphony) scanned by Coverity and get a better idea of the Security and Quality aspects that get validated.

You can follow the Coverity instructions on [how to integrate with Travis CI](https://scan.coverity.com/travis_ci); make sure to review the [Coverity frequency build limits](https://scan.coverity.com/faq#frequency) and adjust Travis configuration to run builds only on stable branches.