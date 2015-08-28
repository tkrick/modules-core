#ModulesV2 (spiffy name later)

## Back story
P--man and S--rise are historic systems that are internal modularity systems.

We would like to create a modularity system (with default implementation in msbuild) that provides the union and more functionality of these internal systems.

## Engine/Format Goals

- **Decoupling**: 
    - no need to traverse to generate exports implementation (project is specified explicitly, like S--rise). Decouples from specific build system's traversal method.
    - not coupled to a source repository or build system. engine implementation should be pluginable (msbuild) but format is decoupled from msbuild.
    - format/engine not coupled to binary form versioning, like P**man. Only the checkout process is.
- **Open Source**
    - this project will be open source, and can do so because of the decoupling from specific systems (msbuild is available)
- **Substitutability**: can substitute binary (NuGet) and source form packages. 
    - good for open sourcing without the need to transform the project files
    - all references (binary or source) look the same
- **New Support**:
    - miscfolder deployment
    - fine grained modularity dependency visibility controls
    - migrating features from pacman adds:
        - native code support
        - custom abstractions (mostly for shipping toolsets as modules)
        - symbolic exports
        - artifact deployment (gathering the dependency closure of artifacts)
    - dependency mapping between export symbols now look better
- **Retain Valuable S--rise Features**:
    - break build on underdeclared (and add overdeclared)
    - break build on going around abstractions
    - break build on taking illegal dependency (though S--rise implementation needs adjusting)
- **S--rise BackCompat**: maintain backwards compatibility with skyrise modules (convention is that there is a default export if none is supplied, see moduleFormat-SimpleByConvention.xml for example)
- **P--man BackCompat/Migratability**: map P--man's dependency types and granularity on to this format, so the migration granularity is practically 1:1
    - only major change is source form project location is explicit, not derived from a traversal.
    - technically could generate P--man modules as well from ModulesV2 format.


## Feature Breakdown

| Feature | P--man | S--rise | ModulesV2 (projected) | 
|-----------|------|---------|----------|
| Custom Abstractions/Toolsets | Yes | No | Yes |
| Native | Yes | No | Yes |
| Underdeclared Break Build | No (might break publish) | Yes | Yes |
| Around Abstractions Break Build | No (might break late on form switching) | Yes | Yes |
| Illegal Dependency Break Build | No (only breaks upgrade) | Needs Work | Yes | 
| Artifact Deployment | Yes | No | Yes |
| Symbolic exports | Yes | No | Yes |
| Substitutability | Yes | No | Yes |