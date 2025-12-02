# Code Contribution

## Eclipse Contributor Agreement

Since Arrowhead project is governed within the [Eclipse Foundation](https://projects.eclipse.org/projects/iot.arrowhead), the authors of any contribution must agree and accept the [Eclipse Contributor Agreement ("ECA")](https://www.eclipse.org/legal/ECA.php).

Technically it means that contributors need an [Eclipse Fundation Account](https://accounts.eclipse.org/user/register?destination=user/edit) which makes possible to submit a signed ECA! GitHub Pull Requests could be accepted only when all the authors own an Eclipse Foundation Account with the signed ECA!

### ECA verification

The existence of the signed ECA is verified based on the authors commits. Your local Git client should hold and push the commits with the same email address like the Eclipse Foundation Account was created with. When the email address is different, than the verification process will fail and the Pull Request is not possible to merge. 

#### Configuring Git client

Use the command below to configure your git client globally:

`$ git config --global user.email anybody@example.com`

Whithout the `--global` flag you can set the email address only for that specific project where the command was executed.

[Learn more from Eclipse Handbook.](https://www.eclipse.org/projects/handbook/#resources-commit)

## Way of contributing

1) **Fork (and not clone) the project repository.**

   - [Working with forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks)
   - For developing a completely new components, [open a ticket](https://github.com/eclipse-arrowhead/roadmap/issues/new)

2) **Create a new branch in your fork and from the `development` branch for your contribution.**

_(Creating and pushing new branches within a cloned repository is allowed only for official [Committers](https://projects.eclipse.org/projects/iot.arrowhead/who))_

3) **Take care of coding clean!**

   - Stick to the [Implementation Structure](../home/implementation.md#implementation-structure)
   - [Clean coding in Java](https://www.baeldung.com/java-clean-code)
   - [A short summary of Java coding best practices](https://rhamedy.medium.com/a-short-summary-of-java-coding-best-practices-31283d0167d3)

4) **Make sure you have a working contribution**

   - Write unit tests
   - Run several test scenario

5) **Make sure you are not using restricted third party library**

   - Build the project with `mvn clean install -P license-check -DskipTests`. After the project build has been finished, you will find a summary about the used libraries in the `\target\dash\summary` file. Look for your newly added libraries and make sure they are not restricted. 

6) **Synchronize the `development` branch in your fork and merge it into your contribution branch** 

7) **Rise a Pull Request from your fork to the `development` branch of the original project repository.**

   - [Creating a pull request from a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)

8) **Wait for the review.**

   - Pull requests with ECA issue, merge conflict, unit test issues or running issues will be closed without review

9) **Deal with the requested fixes if any.**

10) **When your Pull Request is approved, then it will be merged by a committer and will be contained by the coming release**

## GitHUB Repositories

**Core Systems**

:material-github: [eclipse-arrowhead/ah5-common-java-spring](https://github.com/eclipse-arrowhead/ah5-common-java-spring) <br />
:material-github: [eclipse-arrowhead/ah5-core-java-spring](https://github.com/eclipse-arrowhead/ah5-core-java-spring)

**Supporting Systems**

:material-github: [eclipse-arrowhead/ah5-blacklist-java-spring](https://github.com/eclipse-arrowhead/ah5-blacklist-java-spring) <br />
:material-github: [eclipse-arrowhead/ah5-translation-manager-java-spring](https://github.com/eclipse-arrowhead/ah5-translation-manager-java-spring) <br />
:material-github: [eclipse-arrowhead/ah5-device-qos-evaluator-java-spring](https://github.com/eclipse-arrowhead/ah5-device-qos-evaluator-java-spring)  <br />