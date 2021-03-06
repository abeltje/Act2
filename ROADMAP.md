# ACT REDESIGN ROADMAP

The goal is to redesign Act entirely, based on the experience on running
150+ Perl conferences over a decade.

The main requirements for the redesign are:

* maintain the old URL for the past conferences running the legacy Act
* keep the old conferences running at all times
* use modern Perl libraries and design
* add new and powerful features that were too complex (or impossible)
  to add to the legacy Act
* enable new contributors to join the project


## Timeline

The Act redesign is object-driven, centered around the "conference data",
managed by way of the entity objects (`Act::Entity::`).

The following definitions will be used:

* `mod_perl`: the Apache application that is running since 2004,
  built on top of `Act::Object`
* `legacy URL`: the URL served by `mod_perl`, and that are linked from
  many places over the Internet
* `old_schema`: the database schema used by the `mod_perl` application
  (using it also implies using the `act.ini` files to get some of the data)
* `Act2`: the newer version of Act running on entities
* `new_schema`: the improved database schema that might be need to support
   new features in `Act2`

### Stage 0

Only `legacy URL` exist, served by `mod_perl` on top of `old_schema`.

### Stage 1

Entity objects are designed, work on `Act2` starts.
The entity objects are fleshened from the data in `old_schema`, and the
data is saved there too.

A "beta" site can be setup, running the `Act2` code. Since it's connected
to the same database schema (`old_schema`) as the `mod_perl` application,
old conferences can be seen in the new context, and managed with the parts of
`Act2` that are already available.

A subset of the `Act2` application can be made to support some of the
`legacy URL`, to be able to continue serving the old URL in the future.
This must be done using entity objects, because the `old_schema` will
go away in the future. However, there is only a need to support the
"read-only" URL, because the "write" operations will be performed by `Act2`.

Note: there can be several versions of `Act2`, living in separate branches
or repositories.

### Stage 2

`Act2` is ready to go out of beta, i.e. it is complete enough to support
a complete conference over its lifetime.
`Act2` also supports serving `legacy URL` using entities.
`Act2` is still using `old_schema` and `act.ini` files.

`mod_perl` becomes completely obsolete, and can be shut down.
The `legacy URL` supported by `Act2` are locked down to the list of all
conferences at this point in time (i.e. past conferences only).

At this stage, anyone running Act using the `mod_perl` application on
top of the `old_schema` should be able to upgrade to `Act2`, while
keeping all the `legacy URL` working.

`Act2` MUST be a drop-in replacement for Act at this point. When upgrading
from legacy Act, one should start from a specific commit that will have
a well-known (to be defined) tag from which a smooth migration path to
the latest development version will be provided.

### Stage 3

`Act2` is the only application managing conferences. The lockdown of
`old_schema` is dropped, and work can start on `new_schema` to better
support the entities and the new features.


## Beta versions

To encourage contributions, working *beta* versions will be installed
on top of the production schema. This implies such a *beta* version
MUST be using the `Act::Schema` in the `spec` branch.
