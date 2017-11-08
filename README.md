# readium-shared-js-deploy

## Introduction

This repository is used for packing an unmodified version of readium-shared-js
into a JAR and deploying the result to NYPL's Nexus instance. These JARs are
used by versions of the Android application that depend upon readium-shared-js
0.27.0 or newer. (Older versions of the application depend upon [a forked
version of
readium-shared-js](http://github.com/NYPL-Simplified/readium-shared-js).)

## Deploying

1. If you have not already done so, configure `~/.m2/settings.xml` so Maven has
   your Nexus credentials. It should look like the following—replace `USERNAME`
   and `PASSWORD` appropriately:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <settings
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xsi:schemaLocation=
        "http://maven.apache.org/SETTINGS/1.0.0
         http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <servers>
        <server>
          <id>nypl-nexus-group</id>
          <username>USERNAME</username>
          <password>PASSWORD</password>
        </server>
      </servers>
    </settings>
    ```

    **NOTE:** You must have deploy permissions on Nexus for deploying to work.

2. `git submodule update --init` (if you have not done so previously).

3. `cd readium-shared-js`, `git fetch` to be sure you are up-to-date with
   upstream, and `git checkout TAG` to get the version of readium-shared-js you
   wish to package and deploy. You can then `cd ..` back to the root of the
   repository. You may want to `git status` to ensure the submodule does not
   contain modified content: It should only say `(new commits)` next to the
   change.

4. `mvn deploy` to package the readium-shared-js build outputs into a JAR and
   deploy them to Nexus.

5. If deploying happened successfully, `git commit -a; git push` your changes
   and then `git tag VERSION; git push origin VERSION` (where version is the
  version of readium-shared-js, e.g. `0.27.0`). You are done!
