# Release notes

## Automated release notes

Pull requests are squash merged into the main branch. Following this the FCP CI pipeline will automatically create an immutable GitHub release.

This release will automatically include the version and a release note.

Teams should therefore consider the content of the squash commit.

## Manual release notes

For scenarios where automated release notes are not applied, follow the below template.

#### Title
semver Version (e.g. `1.0.0`)

#### Description
##### Major
- bullet list of major changes

##### Minor
- bullet list of minor changes

##### Patch
- bullet list of patch changes

Each of the above will be optional and dependant on the changes that have been made.

For complex changes include code snippets in the relevant description heading.

## Production deployments

For all production deployments, an approved change request in myIT (ServiceNow) is required.  Each change request should include the microservice name and version being deployed.

Teams typically maintain their own release calendar to track their daily deployments to Production.
