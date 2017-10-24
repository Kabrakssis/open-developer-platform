---
date: 2017-07-20T00:11:02+01:00
title: ODP Technologies
type: index
weight: 60
---

...

## The Foundation Developer Pod

The Foundation's Developer Pod is a hosted Symphony Pod (Developer tier) that allows project teams to develop and test against the [Symphony APIs](http://developers.symphony.com).

Once granted access to the ODP you will receive one or more of the following accounts:

1. A **User Account**, that grants you access to [https://foundation-dev.symphony.com](https://foundation-dev.symphony.com) with `username/password` credentials; credentials are personal and cannot be shared; password is initially emailed after registration and needs to be changed during the first login, which can happen via:
  2. Web Interface
  3. Mobile App (Android/iPhone)
  4. Desktop App (for Windows)
5. A **Service Account**, that allows your Symphony bots/apps to interact with the ODP using certificate-based credentials; certificates are per-project, distributed to and then managed by the Project Lead,
6. A **Custom App ID**, that allows Symphony extensions (using the Symphony Extensions API) to run against the ODP; this token is per-project and can be shared across different Developers that work on the same codebase

### Endpoints
For service accounts, below are reported the HTTPS endpoints that deliver all Symphony API endpoints:
- [https://foundation-dev-api.symphony.com/keyauth](https://foundation-dev-api.symphony.com/keyauth)
- [https://foundation-dev-api.symphony.com/sessionauth](https://foundation-dev-api.symphony.com/sessionauth)
- [https://foundation-dev.symphony.com/agent](https://foundation-dev.symphony.com/agent) (prior to 1.46.0 API version, it was [https://foundation-dev-api.symphony.com/agent](https://foundation-dev-api.symphony.com/agent))
- [https://foundation-dev.symphony.com/pod](https://foundation-dev.symphony.com/pod)

### Validate access
[TODO - point to different sample projects, depending on the language]
If you want to validate access, you can test using Java (recommended) or using the Agent SDK.

### Service Level
This environment is not intended to be used as a Production or Staging environment, therefore it enforces some platform restrictions:
1. The contents of this environment are not backed up, therefore they cannot restored under any circumstances.
2. This is a shared environment - there is no guarantee of privacy or secrecy between developers - test messages and content sent by one developer may be visible to all other developers using the environment
3. There is no guaranteed uptime, the platform could go down at any time

### Default User entitlements
The following default entitlements are assigned to user and service accounts by default; you can request entitlement to be changed at any time by opening an ODP issue.
1. Can have delegates - No
2. Can edit profile picture - Yes
3. Can create internal public rooms - Yes
4. Can send files internally - Yes
5. Can chat in external IM/MIMs - No
6. Can chat in private external rooms - No
7. Can share files externally - No
8. Can create push signals - No

### User Terms
Each ODP user:
1. Must agree to and comply with the Symphony Business User Terms & Conditions (BUTC);
2. May only use the ODP for testing and development, not for production use;
3. May only use the ODP for its own internal business purposes (as set out in the BUTC) or for purposes relating to the Foundation, its operations, and its corporate mission; and
4. May not (a) use applications developed on the ODP in another Symphony environment or (b) sell or distribute those applications to third parties, unless it has entered into a separate written commercial agreement with Symphony Communication Services, LLC. The sole exception to this restriction is that ODP users may contribute their applications to the Foundation to be released as open source software under the Apache License, v.2.

***The Foundation reserves the rights to revoke/deactivate credentials at any time.***