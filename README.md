# apigee-config-maven-plugin

Maven plugin to create/update Apigee config like Cache, KVM, Target Server, API Products, Developers and Developer Apps.

Help teams follow API development best practices with Apigee.
  * Prefer config like Cache, Target Servers over hard-coded alternatives in the API
  * Scope config to the least visibility level required (API, env, org)
  * Track Config in source control
  * Deploy changes to config alongwith API deployment

A new config file edge.json is used to hold all the config data to create the corresponding entities in Apigee Edge.

This is the plugin source code project. Refer to samples folder for plugin usage samples.

## Plugin Usage
```
mvn install -Ptest -Dapigee.config.options=create -DencryptPwd=<your-encryption-password>

  # Options

  -P<profile>
    used to pick a profile in the parent pom.xml (shared-pom.xml in the example). Apigee org and env
    information comes from the profile.

  -Dapigee.config.options
    none   - No action (default)
    create - Create when not found. Pre-existing config is NOT updated even if it is different.
    update - Update when found; create when not found. Refreshes all config to reflect edge.json.
    delete - Delete all config listed in edge.json.
    sync   - Delete and recreate.

  -Dapigee.config.dir=<dir>
     dir containing individual json files.

  -DencryptPwd=<your-encryption-password>
     password that is used to encrypt and decrypt the consumer credentials. This is required only if you want to setup specifi client credentials in your developer apps configurations

  # Individual goals
  You can also work with individual config class using goals directly. The available goals are,
  apiproducts
  developerapps
  caches
  developers
  kvms
  targetservers
  maskconfigs

  To delete all apps mentioned in edge.json use the following.
  mvn apigee-config:apps -Ptest -Dapigee.config.options=delete
```
The default "none" action is a NO-OP and it helps deploy APIs (using [apigee-deploy-maven-plugin](https://github.com/apigee/apigee-deploy-maven-plugin)) without affecting config.

## Sample project
Refer to an example API project at [/samples/EdgeConfig](https://github.com/apigee/apigee-config-maven-plugin/tree/master/samples/EdgeConfig)

This project demonstrates the creation and management of Apigee Edge Config and performs the following steps in sequence.
  - Creates Caches
  - Creates Target servers
  - Creates KVM
  - Creates API products
  - Creates Developers
  - Creates Developer Apps

To use, edit samples/EdgeConfig/shared-pom.xml, and update org and env elements in all profiles to point to your Apigee org, env. You can add more profiles corresponding to each env in your org.

      <apigee.org>myorg</apigee.org>
      <apigee.env>test</apigee.env>

To run jump to samples project `cd /samples/EdgeConfig` and run

`mvn install -Ptest -Dusername=<your-apigee-username> -Dpassword=<your-apigee-password> -Dapigee.config.options=create -DencryptPwd=pwd`

**NOTE: All the encrypted client credentials used in the sample was encrypted using `-DencryptPwd=pwd`**

Refer to [samples/APIandConfig/HelloWorld](https://github.com/apigee/apigee-config-maven-plugin/tree/master/samples/APIandConfig/HelloWorld) for config management alongwith API deployment using [apigee-deploy-maven-plugin](https://github.com/apigee/apigee-deploy-maven-plugin). More info at [samples/README.md](https://github.com/apigee/apigee-config-maven-plugin/blob/master/samples/README.md)

## edge.json - v1.0
edge.json contains all config entities to be created in Apigee Edge. It is organized into 3 scopes corresponding to the scopes of config entities that can be created in Edge.
   ```
     envConfig
     orgConfig
     apiConfig
   ```
For example, API product is an orgConfig whereas Cache is an envConfig. The envConfig is further organized into individual environments as follows.
   ```
     envConfig.test
     envConfig.dev
     envConfig.pre-prod
   ```
The environment name is intended to be the same as the profile name in pom.xml (parent-pom.xml or shared-pom.xml).

The final level is the config entities.
   ```
     orgConfig.apiProducts[]
     orgConfig.developers[]
     orgConfig.developerApps[]
     envConfig.test.caches[]
     envConfig.test.targetServers[]
   ```

Each config entity is the payload of the corresponding management API. The example project may not show all the attributes of config entities, but any valid input of a management API would work.

Config entities like "developerApps" are grouped under the developerId (email) they belong to.

## ROADMAP
  - Support management API OAuth and MFA
  - KVM (CPS)
  - Keystore, Truststore support
  - Virtual host (on-prem)
  - Custom roles
  - User role association

Please send feature requests using [issues](https://github.com/apigee/apigee-config-maven-plugin/issues)

## Contributing
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Latest Release
[Version 1.1.3](https://github.com/apigee/apigee-config-maven-plugin/releases/tag/apigee-config-maven-plugin-1.1.3) - Adds support for individual config files and maskconfig.
[Older Releases](https://github.com/apigee/apigee-config-maven-plugin/releases)

## License
* see [LICENSE](https://github.com/apigee/apigee-config-maven-plugin/blob/master/LICENSE) file

## Support
* Post a question in [Apigee community](https://community.apigee.com/index.html)
* Create an [issue](https://github.com/apigee/apigee-config-maven-plugin/issues/new)

## Credits
Madhan Sadasivam, Prashanth K S, Meghdeep Basu, Sai Saran Vaidyanathan
