# Git-Flow Maven Plugin

The Maven plugin for Vincent Driessen's [successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/).

Currently a Java implementation of Git version control system [JGit](https://github.com/eclipse/jgit) doesn't support [`.gitattributes`](http://git-scm.com/book/en/Customizing-Git-Git-Attributes).

This plugin runs Git and Maven commands from the command line ensuring that all Git features work properly.


# Installation

The plugin is available from Maven central.
    
    <build>
        <plugins>
            <plugin>
                <groupId>com.amashchenko.maven.plugin</groupId>
                <artifactId>gitflow-maven-plugin</artifactId>
                <version>1.0.1-alpha4</version>
                <configuration>
                    <!-- optional configuration -->
                </configuration>
            </plugin>
        </plugins>
    </build>


# Goals Overview

- `gitflow:release-start` - Starts a release branch and updates pom(s) with release version.
- `gitflow:release-finish` - Merges a release branch and updates pom(s) with next development version.
- `gitflow:feature-start` - Starts a feature branch and updates pom(s) with feature name.
- `gitflow:feature-finish` - Merges a feature branch.
- `gitflow:hotfix-start` - Starts a hotfix branch and updates pom(s) with hotfix version.
- `gitflow:hotfix-finish` - Merges a hotfix branch.


# Plugin Common Parameters

All parameters are optional. The `gitFlowConfig` parameters defaults are the same as in below example.
Maven and Git executables are assumed to be in the PATH, if executables are not available in the PATH or you want to use different version use `mvnExecutable` and `gitExecutable` parameters.
   
    <configuration>
        <mvnExecutable>path_to_maven_executable</mvnExecutable>
        <gitExecutable>path_to_git_executable</gitExecutable>
                  
        <gitFlowConfig>
            <productionBranch>master</productionBranch>
            <developmentBranch>develop</developmentBranch>
            <featureBranchPrefix>feature/</featureBranchPrefix>
            <releaseBranchPrefix>release/</releaseBranchPrefix>
            <hotfixBranchPrefix>hotfix/</hotfixBranchPrefix>
            <versionTagPrefix></versionTagPrefix>
        </gitFlowConfig>
    </configuration>

## Additional goal parameters

The `gitflow:release-finish` and `gitflow:hotfix-finish` goals have `skipTag` parameter. This parameter controls whether the release/hotfix will be tagged in Git.
The default value is `false` (i.e. the release/hotfix will be tagged).

All `-finish` goals have `keepBranch` parameter which controls whether created support branch will be kept in Git after the goal finishes.
The default value is `false` (i.e. the supporting branch will be deleted).


# Non-interactive Release

Releases could be performed without prompting for the release version during `gitflow:release-start` goal by telling Maven to run in non-interactive (batch) mode.
When `gitflow:release-start` is executed in the Maven batch mode the default release version will be used.

To put Maven in the batch mode use `-B` or `--batch-mode` option.

    mvn -B gitflow:release-start gitflow:release-finish

This gives the ability to perform releases in non-interactive mode (e.g. in CI server).