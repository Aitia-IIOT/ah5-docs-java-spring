---
name: "🚀 AH Release Checklist"
about: "Use this checklist for tracking a new Arrowhead FW release"
---

# 🚀 Release Checklist

## Release-plan
- [ ] Create a GitHub milestone in every repository with the release version as the title, to which the issues can be linked
- [ ] Create a release plan on [Eclipse Fundation Project Site](https://projects.eclipse.org/projects/iot.arrowhead)
- [ ] Publish TESTBUILD executables on the documentation site at least one week prior to the planned release and link to the eclipse release plan
- [ ] Create new changelog on the documentation site that can be maintained and linked already to the TESTBUILDs
- [ ] Make sure that new 3rd party libraries (if any) have proper linceses (use eclipse dash-lincense tool)
- [ ] Make sure that every contributor have valid ECA

## Pre-release
- [ ] Update documentation site with relevant contents (SysD, SD, API Page, Tutorials, etc.)
- [ ] Finalize the changelog on the documentation site and refer it from Release Versions page
- [ ] Make sure that every Core/Support systems have working docker build files (system and database)
- [ ] Update the docker compose examples to refer the proper version number and make new compose examples for new systems if any
- [ ] Update the NOTICE file in every repository with new repositories (if any) and new 3rd party libraries if any
- [ ] Make sure that every pom contains the proper version
- [ ] Make sure that every system configuration file contains the default config
- [ ] Update the known issues in the documentation site if any has been fixed in the actual release
- [ ] Update the repo list in the Code Contribution page in case of any new repo
- [ ] Update the repo list in the Bug Report page in case of any new repo

## Release
- [ ] Make sure that every contribution planned to the actual release have been merged to the developemnt branches in every repo
- [ ] Create a PR from development branch to the master branch with the release version as title and link to the associated GitHUB milestone in every repo
- [ ] Merge the release PRs to the master branches in every repo
- [ ] Label the issues with `vX.Y.Z release` and close them in every repo
- [ ] Create GitHUB releases with version tag and zipped executables in every repo
- [ ] Close the GitHUB Milestones in every repo
- [ ] Create a download section for the actual release on the documentation site
- [ ] Release and publish the new version of the documentation site
- [ ] Build and publish new docker images with the version number of the actual release
- [ ] Write overviews and add category tags in DockerHUB for the new systems
- [ ] Update the [Eclipse Fundation Project Site](https://projects.eclipse.org/projects/iot.arrowhead) release plan with the links to the changelog on the documentation site and the to the release download section
- [ ] Schedule a review with EMO in case of a major release, or if a review hasn’t been done for a year (dash-license tool with automatic issue creation has to be executed too)

## Post-release
- [ ] Email to the project leaders
- [ ] Start a new release plan for the next release cycle
