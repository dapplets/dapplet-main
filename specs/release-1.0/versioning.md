[working copy](https://hackmd.io/8LP2bqUqQ8OjpYSa2fkHgA?both)

## General Design Choices made.
1. Extension, Adapters, Features and any other contiously developed objects should have versioning, denoted by `VersionId`, defined and managed as defined in [Semantic Versioning Scheme](https://semver.org/).

## UseCase Story:
1. The Extension itself becomes a *Version* like any other object published on the *DappletPlatform*.
2. Developer sets a *VersionRange* for every dependency managed  by *DappletPlatform* inclusive the dependency to `Core`.
3. Developer tags an every new published version the new *Version*.
4. Publishing process creates two objects in the same version: Class and its Interface (which acts as the Metadata, actually). 
5. Tester verifies and ensures a bug free functionality for particular dependency set or reports a bug there.
6. User may allow authomatic mode for dependency management based on public test and audit results or he manages particular dependencies in manually.
 
## Requirments:
1. For user script the version should be denoted by `PublicName` decorator.
```typescript
    @PublicName("CommonLib.dapplet-base.eth","1.1.0-alfa+10")
    class TwitterFeature implements ITwitterFeature {
    ...
    }
```
or
```typescript
    @PublicName({name:"CommonLib.dapplet-base.eth",version:"1.1.0-alfa+10"})
    class TwitterFeature implements ITwitterFeature {
    ...
    }
```
2.  A `package.json` evaluates dependency versions at design-time. A `@Load` evaluates dependency information at runtime (time of loading and dynamic linking). In some cases dependency version information may be transfered from `package.json` to `@Load` decorator automatically as part of build process. 
ToDo for later releases. (`Milestone >1.0`).
 
3.  An npm [semver module](https://www.npmjs.com/package/semver) should be used in the extension's background service as an implementation of version management.
4. An `@Load` injector may supply a `semver range expression` as defined by [semver](https://www.npmjs.com/package/semver#ranges), defining a compatible dependency vesions as designed by developer. 
```typescript
    @Load("CommonLib","~v1.1.0")
    public library : any;
```
or
```javascript
    @Load({name:"CommonLib",version:"~v1.1.0"})
    public library : any;
```
4. A dependency version gets "locked" at the time of deployment, like "package-lock.json" and reflects actually audited and then deployed version. A dependency "upgrade" to newer version (even inside the valid range) is a subject of separate audit and approval workflow, which may be authomatic or manual depending on user settings. 

5. There will be a dependency optimizer, calculating most appropriate set of dependency versions for given set. (`Milestone >1.0`) 
