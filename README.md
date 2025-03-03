# Documentation Repository for Tanzu Application Platform (TAP)

## Overview

This repo contains the content for Tanzu Application Platform docs.

## Branches

<table>
<thead>
<tr>
<th>Branch</th>
<th>Staging</th>
<th>Production</th>
</tr>
</thead>
<tbody>
<tr>
<td>main</td>
<td><a href="https://docs-staging.vmware.com/en/draft/VMware-Tanzu-Application-Platform/1.5/tap/overview.html">Staging</a> (Pre-release v1.5 docs)</td>
<td><em>n/a</em></td>
</tr>
<tr>
<td>1-4-1</td>
<td><a href="https://docs-staging.vmware.com/en/draft/VMware-Tanzu-Application-Platform/1.4.1/tap/overview.html">Staging</a> (Pre-release v1.4.1 docs)</td>
<td><em>n/a</em></td>
</tr>
<tr>
<td>1-4-0</td>
<td><a href="https://docs-staging.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/overview.html">Staging</a></td>
<td><a href="https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/overview.html">Production</a></td>
</tr>
<tr>
<td>1-3-4</td>
<td><a href="https://docs-staging.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-overview.html">Staging</a></td>
<td><a href="https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-overview.html">Production</a></td>
</tr>
<tr>
<td>1-3-3</td>
<td>Not in use. Do not PR to this branch.</td>
<td>Not in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-3-2</td>
<td>Not in use. Do not PR to this branch.</td>
<td>Not in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-3-1</td>
<td>Not in use. Do not PR to this branch.</td>
<td>Not in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-3-0</td>
<td>No longer in use. Do not PR to this branch.</td>
<td>No longer in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-2-2</td>
<td><a href="https://docs-staging.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-overview.html">Staging</a></td>
<td><a href="https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-overview.html">Production</a></td>
</tr>
<tr>
<td>1-2-1</td>
<td>No longer in use. Do not PR to this branch.</td>
<td>No longer in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-2-0</td>
<td>No longer in use. Do not PR to this branch.</td>
<td>No longer in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-2</td>
<td>No longer in use. Do not PR to this branch.</td>
<td>No longer in use. Do not PR to this branch.</td>
</tr>
<tr>
<td>1-1</td>
<td><a href="https://docs-staging.vmware.com/en/VMware-Tanzu-Application-Platform/1.1/tap/GUID-overview.html">Staging</a></td>
<td><a href="https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.1/tap/GUID-overview.html">Production</a></td>
</tr>
<tr>
<td>1-0</td>
<td><a href="https://docs-staging.vmware.com/en/VMware-Tanzu-Application-Platform/1.0/tap/GUID-overview.html">Staging</a></td>
<td><a href="https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.0/tap/GUID-overview.html">Production</a></td>
</tr>
</tbody>
</table>

## Components with their own repositories

Some components have docs that introduce them in this repository, but the rest of their docs are
stored in dedicated repositories.

| Component | Repo |
|-----------|------|
| API Auto Registration | https://gitlab.eng.vmware.com/tap-api-first/api-auto-registration |
| API portal | https://github.com/pivotal-cf/docs-api-portal |
| Carvel | https://github.com/vmware-tanzu/carvel/tree/develop/site/content |
| Cloud Native Runtimes | https://gitlab.eng.vmware.com/daisy/cloud-native-runtimes-for-vmware-tanzu |
| Services Toolkit | https://gitlab.eng.vmware.com/services-control-plane/documentation |
| Tanzu Build Service | https://github.com/pivotal-cf/docs-build-service/tree/v1.5 |

## Product Names

Use the complete product name at first use:

- Cloud Native Runtimes for VMware Tanzu
- Application Accelerator for VMware Tanzu
- Application Live View for VMware Tanzu
- VMware Tanzu Build Service

And thereafter:

- Cloud Native Runtimes
- Application Accelerator
- Application Live View
- Tanzu Build Service (not Service**s**)
- Tanzu Application Platform (not TAP)

## Word List

Use this table to keep a running list of terms used and how they should be defined.

| Word or phrase | Explanation |
|----------------|-------------|
| API Auto Registration | Both __Auto__ and __Registration__ are separated from each other, and capitalized |
| API portal | The p _is_ supposed to be lowercase. [Pull request #73](https://github.com/pivotal/docs-tap/pull/73#issue-1011334602).|
| components | The individual products available in the TAP SKU. For example, Cloud Native Runtimes is a component of TAP. Not: add-ons or capabilities.|
| convention controller | Lowercase; this is one of the minor components that make up Convention Service |
| convention server | Lowercase; this is one of the minor components that make up Convention Service |
| Convention Service | Title case; this is a component with its own TanzuNet product page |
| Default Supply Chain | singular |
| kubectl| First use in a topic: Kubernetes command-line tool (kubectl). Subsequent uses: kubectl. |
| Tanzu Insight | This CLI plug-in is named simply Insight in the v1.0 documentation because it is separate from the Tanzu CLI |
| packageRepository | Is a definition. Variations found in original doc (Package repository, PackageRepository, packagerepository) but standardize on the one shown. 2021.08.26 |
| PackageRepositories | Don't use. There is really only one packageRepository of interest for this page. |
| packageRepository custom resource | Because we don't use CR in other Kubernetes docs, spell out custom resource here too. An example of the packageRepository custom resource is given in the YAML file named `tap-package-repo.yaml`.|
| packageRepository pull | Just means pulling the packages from the repository|
| Services Toolkit for VMware Tanzu |  (First use in body text) No longer Services Control Plane Toolkit 2021.10.26 Ed Cook|
| Services Toolkit|  (In headings and after first use in body text) No longer SCP Toolkit 2021.10.26 Ed Cook|
| Source Controller | Title case; this is a component with its own TanzuNet product page |
| Supply Chain Security Tools - Store | Not SCST - Store |
| Supply Chain Security Tools - Scan | Not SCST - Scan |
| Tanzu Application Platform GUI | Not Tanzu Application Platform Graphical User Interface, nor TAP GUI, nor TAP UI, etc. Also not "the Tanzu Application Platform GUI" because Tanzu Application Platform GUI is considered a product name. |
|Tanzu Developer Tools for IntelliJ| The long form is VMware Tanzu Developer Tools for IntelliJ. It can be referred to as "the extension" in a topic after introducing it as "the Tanzu Developer Tools for IntelliJ extension" when there is no risk of confusion. |
|Tanzu Developer Tools for VS Code| The long form is VMware Tanzu Developer Tools for Visual Studio Code. It can be referred to as "the extension" in a topic after introducing it as "the Tanzu Developer Tools for VS Code extension" when there is no risk of confusion. |
| Tanzu Kubernetes Grid | Never use TKGm or TKG in customer facing documentation. |
| TAP repo bundle | Decided on lowercase and not "TAP Repo Bundle".|
| TAP packages | Right now there are three packages: one for each component. The three packages make up the bundle. The bundle is stored in the the TAP package repository. Although "Tanzu Application Platform packages" is in the original google doc, let's use "TAP packages" for consistency.|
| TAP package repository |  How is this different from the other package repositories? (Are there non-TAP package repositories discussed on this page?) Changed from TAP to Tanzu Application Platform, Sept 24, 2021.|
| .yaml and YAML file | Standardize on using the "a", not `.yml` |

## Placeholder List

| Placeholder | Definition |
|-------------|------------|
| `TANZU-NET-USER` and `TANZU-NET-PASSWORD` | Where `TANZU-NET-USER` and `TANZU-NET-PASSWORD` are your credentials for Tanzu Network |
| `PACKAGE-REPO-NAME`| Where `PACKAGE-REPO-NAME` is the name of the packageRepository from step 1 above.|
| `VERSION` | Where `VERSION` is the version of the package listed in step 7|

## Other style stuff

Top tips:

- Keep line lengths short, around 110 chars.
- Start each sentence on its own line. Markdown only creates new paragraphs after a blank line.
- We've changed our heading style to match the more modern style: sentence case not title case.
  So going forward, we only need to capitalize the first word in a title. E.g. "Other style stuff"
  instead of "Other Style Stuff."
- UI elements are bolded and the widget type not mentioned.
  For example, "Click on the `NEXT STEPS` button." is rewritten as "Click **NEXT STEPS**."
- Outside of code, Kubernetes API objects are written in lowercase and the words are separated by
  spaces, like for other common nouns. This is in contrast to the Kubernetes docs, where upper camel
  case is sometimes used and sometimes not.

## Autogenerated Docs

If a doc file is autogenerated, any direct edits are overwritten at the next autogeneration event.
The files in these folders are autogenerated:

- scst-store/cli_docs

## Troubleshooting Markdown

| Problem | List displays as a paragraph |
|---------|-----------|
| Symptom:| Bulleted or numbered lists look fine on GitHub but display as a single paragraph in HTML.|
| Solution: | Add a blank line after the stem sentence and before the first item in the list.|

| Problem | List numbering is broken: every item is `1.` |
|---------|-----------|
| Symptom:| Each numbered item in a list is a `1.` instead of `1.`, `2.`, `3.`, etc|
| Solution: | Try removing any blank newlines within each step.|

| Problem | Code boxes not showing |
|---------|-----------|
| Symptom:| VMware publishing system doesn't accept code tags after the three back ticks.|
| Solution: | Make sure you're not using `shell` or `bash` or `console` or `yaml` after back ticks.|

## Creating a Pull Request

To create a pull request, see [Creating pull requests and merge requests](https://confluence.eng.vmware.com/display/MIX/Creating+pull+requests+and+merge+requests)
in Confluence.

## Publishing Docs

Staging docs:

- [docworks](https://docworks.vmware.com/) is the main tool for managing docs used by writers.
- [docsdash](https://docsdash.vmware.com/) is a deployment UI which manages the promotion from
staging to pre-prod to production. The process below describes how to upload our docs to staging,
replacing the publication with the same version.

### Prepare Markdown Files

- Markdown files live in this repo.
- Each page requires an entry in [toc.md](docs/toc.md) for the table of contents.
- Images should live in an `images` directory at the same level and linked with a relative link.

### Create the ZIP File

Starting from the repo root, this will create a new `docs.zip` with no root folder and show its
contents.

```console
git pull ; rm *.zip ; zip -r tap-1-0 *
```

### Upload the ZIP File to Docworks

- Go to https://docworks.vmware.com/md2docs/publish
- Fill in the fields exactly as below. Repeat this every time - the browser can help to remember form fields.
- Click on upload, and when prompted, enter your VMware AD password (for docsdash)
- If you see invalid path errors in the yellow box on the right there are broken links, but the site will still be published.
- If the toc.md is invalid then the site will not build, but there will be no indication that something is wrong.

### Form Fields

Form fields for beta-1: [VMwarePub.yaml](https://github.com/pivotal/docs-tap/blob/beta-1/VMwarePub.yaml)
Form fields for main (beta-2?): [VMwarePub.yaml](https://github.com/pivotal/docs-tap/blob/main/VMwarePub.yaml)

### In Docsdash

1. Wait about 1 minute for processing to complete after uploading.
2. Go to https://docsdash.vmware.com/deployment-stage

   There should be an entry with a blue link which says `Documentation` and points to staging.

### Promoting to Pre-Prod and Prod

**Prerequisite** Needs additional privileges - reach out to Paige Calvert on the docs team [#tanzu-docs](https://vmware.slack.com/archives/C055V2M0H) or ask a writer to do this step for you.

1. Go to Staging publications in DocsDash
  https://docsdash.vmware.com/deployment-stage

2. Select a publication (make sure it's the latest version)

3. Click **Deploy selected to Pre-Prod** and wait for the pop to turn green (refresh if necessary after about 10s)

4. Go to Pre-Prod list
   https://docsdash.vmware.com/deployment-pre-prod

5. Select a publication

6. Click **Sign off for Release**

7. Wait for your username to show up in the **Signed off by** column

8. Select the publication again

9. Click **Deploy selected to Prod**

### Landing Page and Publications

General information about landing pages:

- Every product has a landing page
  (Not exactly true: every umbrella product, such as Tanzu Application Platform should have a landing page.)
- The landing page is a container for all the "publications" for a product.
- Typically there will be a new docs publication for each minor release but not each point release.
  This version number become part of the URL e.g. Our first release was version `0.1` (see form section above for the current release).
- Some products, such as
  [Tanzu Kubernetes Grid](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/index.html),
  publish separate release notes publications for each point release.
- For comparison see https://docs.vmware.com/en/VMware-Tanzu-Application-Catalog/index.html

## Partials

For information about how we use partials, see our
[Working with partials Confluence page](https://confluence.eng.vmware.com/display/MIX/Working+with+TAP+partials).
