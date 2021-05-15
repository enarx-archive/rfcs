# workload-package-format
- Authors: Daiki Ueno
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-05-19
- Status Note: Under discussion
- Supersedes: None
- Start Date: 2020-05-19
- Tags: feature, protocol

## Summary

Typical applications are accompanied with read-only resources used at
run time, such as configuration files or precomputed binary data. This
RFC proposes a way to provision those resources along with the
application executable into a Keep.

## Motivation

Unlike normal applications running on the host, an application
deployed in a Keep must be careful about accessing external resources,
so that it doesn't expose any confidential information to the host. On
the other hand, applications practically require external resources to
achieve their tasks. There needs to be a way to provision such
resources into a Keep so that the applications can access those files
in a secure manner.

## Tutorial

This RFC introduces a new [custom section] in the WebAssembly binary
format, identified by the name `.enarx.resources`. The payload of the
custom section is in the `tar` file format that may contain any number
of resource files.

The name of each file entry MUST be a canonical path name, meaning
that it is a relative path name from the current directory, and
symbolic links or any special path components that affect directory
traversal (`.` and `..`) are resolved in advance.

When running the WebAssembly binary executable, each file entry is
mapped on an in-memory virtual filesystem exposed to the WASI
instance. The executable is allowed to access those files through the
standard WASI system calls such as [`path_open`].

## Reference

[wasi-common] provides a way to expose in-memory data as a file on
virtual filesystem with [VirtualDirEntry]. Once the bundled resources
are mapped to a VirtualDirEntry, it can be associated with the WASI
instance through `WasiCtxBuilder::preopened_virt()` call.

To create a WebAssembly executable with resources, we will provide a
command-line tool that bundles files with the executable, after proper
checks and conversions of the path names.

## Drawbacks

No drawbacks currently known.

## Rationale and alternatives

For simplicity, this RFC is extending the WebAssembly binary format to
be able to embed resources in it. However, there is a possibility to
use a container format directly for packaging, such as bundling the
executable along with other data files in the same archive, or using
the [OCI image format]. The benefits of this approach are:
- The application bundle can be built using a standard tools
- May bring additional flexiblity in composition, such as versioning

## Prior art

[GResource] in glib provides a similar mechanism using a custom
section in the ELF format (`.gresources.*`).

## Unresolved questions

This proposal assumes that bundled resources fit in the Keep's memory.
It is still under discussion what approach should be taken to
provision non-confidential data as well, in particular the sizes of such
resources are large and cause unreasonable memory consumption.

## Implementations

No implementation yet available (2020-05-19).

## Future Possibilities

It might be useful to convey other information than the resources,
such as environment variables or the default command-line arguments,
as a new custom section holding metadata.

[`path_open`]: https://github.com/WebAssembly/WASI/blob/master/phases/snapshot/docs.md#path_open
[custom section]: https://webassembly.github.io/spec/core/binary/modules.html#custom-section
[wasi-common]: https://crates.io/crates/wasi-common
[VirtualDirEntry]: https://docs.rs/wasi-common/0.16.0/wasi_common/enum.VirtualDirEntry.html
[GResource]: https://developer.gnome.org/gio/stable/glib-compile-resources.html
[OCI image format]: https://github.com/opencontainers/image-spec
