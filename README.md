# java-parent
Maven parent POM for java projects.

Note that this might be used as a reference to copy/paste necessary parts into other projects which are using other parents, such as spring boot projects.

## What is included?
- Plugin version management
- Dependency management for
    - JUnit Jupiter
    - Mockito
    - AssertJ
- Plugin configurations
    - Default to Java version 21
    - Fail on compiler warnings
    - Attaches sources
    - Setup failsafe when `src/integration` exist in the project
    - Configure Mockito agent for surefire and failsafe
    - Setup and configure Spotless to fail on build when incorrect formatting is present
      - Remove unused imports
      - POM files
      - Follow Google Java format
    - Setup enforcer
      - For the configured Java version
      - Requires at least Maven 3.9.0
    - Setup SpotBugs and FindSecBugs

## Working with Spotless
Full documentation is found here: https://github.com/diffplug/spotless/tree/main/plugin-maven

Use `./mvnw spotless:check` to verify that all formatting is correct.  
Use `./mwnw spotless:apply` to apply formatting to project.

The decision to connect the check goal instead of the apply goal to the execution is to detect when a developer setup has not properly configured formatting.

## How to use
In order to use the deployed versions you need to configure Maven to access the GitHub packages.
See https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry for more information.

An example setup looks like this:
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <activeProfiles>
    <activeProfile>github</activeProfile>
  </activeProfiles>

  <profiles>
    <profile>
      <id>github</id>
      <repositories>
        <repository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
        </repository>
        <repository>
          <id>github</id>
          <url>https://maven.pkg.github.com/Groland-Studio/java-parent</url>
        </repository>
      </repositories>
    </profile>
  </profiles>

  <servers>
    <server>
      <id>github</id>
      <username>USERNAME</username>
      <password>TOKEN</password>
    </server>
  </servers>
</settings>
```

## Workflows
### PR
This project requires PR in order to contribute towards the code base. Each PR triggers a verification of the POM.

### Push to main
Triggers a verification of the POM.

### Release
Use the following tag format when releasing: vMajor.Minor.Patch. (eg: v1.0.0).
Upon a release the POM is automatically distributed as a package.
