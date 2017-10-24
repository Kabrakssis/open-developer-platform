---
date: 2017-07-20T00:11:02+01:00
title: VersionEye
type: index
weight: 220
---

# (Legal|Security)

Versioneye notifies you about out-dated dependencies, security vulnerabilities and license violations in your Git repositories, providing a security and legal reporting tool for Foundation projects; it supports several package managers and build tool plugins (check the list on [versioneye.com](https://www.versioneye.com/)).

## Getting Started
Versioneye can be enabled on any github public project by simply [creating a project from Github](https://www.versioneye.com/user/projects/github_repositories) (you must [signed in](https://www.versioneye.com/signin)), however, it is recommended to configure your build tool using a VersionEye plugin in order to trigger a scan continuously and provide a deeper analysis.
VersionEye allows to

1. scan multiple branches of the same project, creating separate project entries
2. integrate with multiple build tools on the same project, that will be accessible via the dashboard `Dependencies` tab (see below).

Below are reported the configuration steps for `Maven(Java)` and `NPM(NodeJS)` integration; regardless of the method, you must define a `VERSIONEYE_API_KEY` environment variable containing the value found in the [API Key settings page](https://www.versioneye.com/settings/api); make sure to keep the API Key private; for CI environments, use encryption.

### Using Maven(Java)
The [VersionEye Maven Plugin](https://github.com/versioneye/versioneye_maven_plugin) provides - among others - the following goals:

1. `versioneye:update` submits updates to the VersionEye server for scanning; if the project doesn't exist yet on VersioneEye, it will create it and store the projectId in `src/main/resources/versioneye.properties`
2. `versioneye:securityAndLicenseCheck` breaks the build if security vulnerabilities or license violations are found

The [Foundation Parent POM](https://github.com/symphonyoss/ssf-parent-pom) embeds the VersionEye plugin configuration in a `versioneye` profile (binding the 2 goals above to the `verify` phase); running `mvn package -Pversioneye` will trigger the VersionEye update and scanning.

### Using NPM(NodeJS)
Use the [Versioneye Update package](https://www.npmjs.com/package/versioneye-update) to submit updates for NodeJS projects, although it supports many other [build tools](https://github.com/OnwerkGmbH/node-versioneye-update#supported-project-files).

Follow these steps to trigger the VersionEye update and scanning; the `-l` and `-s` flags ensure that the command will fail if any security vulnerability or license violation is found.
```
npm install -g versioneye-update
versioneye-update --apikey $VERSIONEYE_API_KEY --projectid $VERSIONEYE_PROJECT_ID -l -s
```

## Configuration
VersionEye project configuration can be accessed via the Settings tab, in the project's dashboard; only the project creator and the organisation owner can access it.
The following fields must be set; for each of them, make sure you hit the Save button close to it.
Visibility of the project: Visible to everybody who knows the project URL
Choose a License Whitelist: SSF License Whitelist ; this list implements the Foundation legal requirements (Category A and B) for contribution
Choose a component whitelist: ssf-component-whitelist ; includes OSS components (of a specific version) that are known to cause false negatives
Transfer ownership to an organisation: ssf (press Transfer)
If your project includes multiple modules (ie Maven multi-module projects), each module will be mapped as a separate project in VersionEye; each module can point to a parent one, using the Merge this project as subproject into another project field.
You can Assign one or multiple teams of VersionEye users that will get notified with warning and summary report emails related to this project.

## Dashboard

!!Although the license/security violation count is zero, it is strongly advised to drill-down the dependency report and try to spot some red marks.

The VersionEye can be accessed via the URL `https://www.versioneye.com/user/projects/<VERSIONEYE_PROJECT_ID>`; it provides:
- a project summary information at the top, including a total/oudated dependencies and license violation (based on the `SSF License Whitelist` configured above) count; the badge can be easily [embedded in the project's README.md file](https://blog.versioneye.com/2013/05/08/versioneye-dependency-badges/), although it only reports if there are outdated dependencies, it doesn't provide a result based on license violations or security vulnerabilities found
- a `Re Parse Now` button, that will trigger another parsing of VersionEye of the last data submitted; please note that this will not trigger a new build, it will only scan again the last data that have been submitted
- `Dependencies` and `Security` tabs, described more in details below; each of them allow to drill-down all dependency versions and licenses; the `Files` field provides one entry for each build-related file (ie `pom.xml` or `package-json.lock`) configured in the project (see [Getting Started](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/80944215/VersionEye#VersionEye-GettingStart)) and optionally any sub-module that points to this project (see configuration to setup a multi-module project).

Below is example of project dashboard.

![Versioneye project Dashboard](/versioneye-dashboard-1.png)

### Dependencies
!!You may find some dependencies that are not part of your project, maybe fetched by the build system of choice (check https://github.com/versioneye/versioneye-core/issues/105)

For each dependency, the current and newer version will be reported; this is quite a useful feature, as it can potentially prevent future issues.
Licenses will be marked as orange, if unknown, or red, if not part of the SSF License Whitelist; please note that - if a dependency is dual licensed, it will show up twice in the list; as such, a red mark doesn't imply a license violation.
Make sure to scroll the page all the way down and press "Licenses of transitive dependencies" in order to get the full list.

### Security
VersionEye uses [different vulnerability databases](https://github.com/versioneye/versioneye-security#versioneye-security), depending on the build tools configured in the project; all vulnerabilities found are listed in this tab and linked to a resolution web page.