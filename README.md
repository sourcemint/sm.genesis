**Status: DEV**

Sourcemint.Genesis
==================

> Sourcemint.Genesis is a **100% open and hackable software dependency management system** that can be used for collaborative realtime distributed system development.


Overview
--------

Sourcemint.Genesis is based on [PINF.Genesis](http://genesis.pinf.org) and elevates the [Semantic Web](http://en.wikipedia.org/wiki/Semantic_Web) and your local filesystem to be a distributed, non-conflicting package registry and repository.

The diagram below illustrates the **Package Installation and Distribution Aspects of Sourcemint.Genesis** by showing the:

  * Stages of the package management lifecycle
  * Data and interaction spaces managed by system
  * **Codebase-wide Event Loop** capturing **ALL** modifications to source code

![Sourcemint.Genesis Overview Diagram](https://raw.githubusercontent.com/sourcemint/sm.genesis/master/docs/2015-04%20-%20Overview.jpg)


*TODO: Illustrate the Package Dependency Management Aspect of Sourcemint.Genesis using a Diagram showing the `sm` components.*


Discussion
----------

While the overall design of Sourcemint.Genesis as well as various specific implementation details have been validated in past work, the exact plan for Sourcemint.Genesis is still evolving. I am keeping a loose definition as to what will be covered by Sourcemint.Genesis as I am not yet completely sure about all the final pieces and their arrangement in this new PINF.Genesis based implementation.

One major lesson is the identification of **aspects** and **forms** for each dependency. A dependency has **one or more aspects** and each aspect has a **set of common forms**.

#### Aspects

The *only required aspect* for any dependency is called `source`. Sourcemint is used to work on the source code of a system. Thus you are typically working with code dependencies in source form and thus the term **source** in its typical usage. **However**, even if you are using a third party library, and you only have the compiled distribution, it still falls under the source aspect as it encompasses the **source logic** that you are embedding within your system.

Think of `source` as **THE UNIQUE LOGIC YOU WANT TO EMBED** where *unique* is scoped to your system. i.e. The same logic only exists in the one place. It exists in the *logic source* you have an instance of and have embedded in your system. Following this reasoning, Sourcemint, mints (composes) a system out of many source logic pieces.

An example of a secondary aspect could be `sourcemap`. The `sourcemap` aspect contains code that covers all entities in the `source` aspect (i.e. module files) and adds something new, in this case a mapping of obfuscated names to original names and more. Any additional aspect to *source* is OPTIONAL but often desired at various stages of the software lifecycle.

Other examples of additional aspects are *Documentation*, *Specification*, *API Reference*. *Mailing List*, *Live Support*, *Issues*, etc... all *scoped to the source aspect*. While a list of common aspects will be compiled and specified in time, the addition of any new aspect must be supported generically by any implementation.

#### Forms

The *form* of an aspect describes a specific arrangement/format/manifestation of the logic contained within the aspect. The set of *forms* supported by a system is tied to the composition needs of the system throughout its development, distribution and operational lifecycles. Sourcemint.Genesis targets modern web software and specifically JavaScript and PHP based implementations and its supported *forms* have been validated within this context. The chosen *forms* seem to be sufficiently generic to support a large variety of complex composition scenarios and ultimatly can be used to model, for dynamic composition, most software systems.

Sourcemint.Genesis implements the following *forms* and it is expected that only very few forms will be added in the future. **Each aspect must exist in most forms** before a system may boot and thus a **dependency is considered provisioned as soon as it is present in all essential forms**. Sourcemint.Genesis will derive each form (if it does not already exist) based on very specific rules that transform the aspect from one *form* to another by embuing it with more detail or stripping detail away. Not all *forms* may be derived from or recovered from other *forms* and the possibilities of this actually represent very important transparency and security boundaries.

  * **sync** - The optional *form* of the *aspect* that contains ad-hock changes made by a developer while working on the system. If, for example, a developer has included jQuery in their project and needs to make a change to the implementation they would do so in some editing environment which would sync the changed file (or a patch) to this container. It is termed *sync* as it is designed to stay in sync with all your source logic changes as fast as you can type them in your chosen editing environment.

  * **vcs** - The *form* of the *aspect* that contains the entire history of the code of the *aspect* using a version control system. By default this holds a *git* repository of the *aspect* with the latest frozen commit checked out. If this *form* is missing it cannot be derived from other *forms* as no other *form* captures the history of the code for an *aspect*. 

  * **snapshot** - The *form* of the *aspect* that contains an export of the latest frozen commit. It may also optionally include a shallow clone of the *vcs form*. This *form* is used to obtain a snapshot of the *aspect* at a given commit and is typically distributed in the form of a release archive as github generates it for download for any given commit or branch. This *form* may be symlinked from the *vcs form* if it is available.

  * **prepared** - The *form* of the *aspect* that contains a *copy* of the *snapshot form* **without any vcs info** and with optional patches applied to adapt the *aspect* to a given system environment. It must contain only code that is consistent across the entire system. i.e. one must be able to create an unlimited number of instances of this code in the various nodes of the system without needing to modify it further than what happend in this container.

  * **built** - The *form* of the *aspect* that contains a **generic build** of the *prepared form* and all its dependencies. All **dependencies are installed and source code is compiled** to binary form in this container if applicable. The compiled binaries must be shared libraries that just like the *parpared form* may be used in any part of the system without modification. The *built form* represents the most optimized stage of all pre-built assets that a system may draw upon to provision itself most optimally and speedily.

  * **configured** - The *form* of the *aspect* that contains *built forms* **configured for a specific instance or node of a system**. This is the container that holds any modifications to the *built form* needed to load the *built aspects* into a specific runtime. Most typically this includes configuration information for the *aspect* to allow it to slot into a container in a running system. If the binary built assets must be modified using binary patches because they are not clean sharable libraries this must happen in this container. If the *prepared form* must be re-compiled because the *built form* is not generic the re-build happens in this container. This latter use-case is less than optimal as the sourcemint design calls for superficial config-based modifications only by the time this *form* is reached.

  * **installed** - The *form* of the *aspect* that holds a symlink to the latest **configured** form that is live as far as the runtime is concerned.

The *forms* above flow from one to the next and instances of each *aspect + form* are generated by Sourcemint.Genesis on the fly as commit references and filechecksums, capturing latest state of modifications, change. e.g. The *configured form* **revision** is determined by a checksum across all files comntained within the *built form + configured form changeset*. This ensures that the same input data will lead to the same *revision* allowing *form instances of each aspect* to be cached in the REST web.


Implementation
--------------

*TODO: Outline the exact rules used to derive each form instance for each aspect.*


Usage
=====

*TODO*


License
=======

[UNLICENSE](http://unlicense.org/)

