# Cuicui chirp tooling

This repository contains tooling for editing `chirp` files used in [`cuicui_chirp`].

## Helix syntax highlight and semantic navigation

We will use [the `chirp` tree-sitter implementation][tree-sitter].

Supposing that `helix` is the root config path for your helix install.
On my system, it is `~/.config/helix`.

1. Add the following to `helix/languages.toml`:

```toml
# Consider adding the following line to speed up the next step
# use-grammars = { only = [ "chirp" ] }
[[grammar]]
name = "chirp"
source = { git = "https://github.com/nicopap/tree-sitter-chirp", rev = "55397d954fead82364d4fa10e0b6b36d9b3d8a3c"}

[[language]]
name = "chirp"
scope = "source.chirp"
file-types = ["chirp"]
comment-token = "//"
indent = { tab-width = 4, unit = " " }
roots = ["Cargo.lock"]
```

2. Run the following commands to generate the grammar:

```sh
helix --grammar fetch
helix --grammar build
```

3. Add the query files to the runtime dir:

```sh
mkdir -p helix/runtime/queries/chirp

COMMIT="55397d954fead82364d4fa10e0b6b36d9b3d8a3c"

for FILE in highlights.scm indents.scm ; do
  URL="https://github.com/nicopap/tree-sitter-chirp/raw/${COMMIT}/queries/${FILE}"
  curl -SL $URL > helix/runtime/queries/chirp/$FILE
done
```

You should be good to go now!

## Planned

- LSP server using `rustdoc` output and the chirp parser to provide completion and
  goto definition for chirp files.
- Formatting tool
- kate syntax highlight file (used by kate and Sublime text)
- mate syntax highlight file (used by VScode)
- Vim syntax highlight file

[`cuicui_chirp`]: https://cuicui.nicopap.ch/chirp/index.html
[tree-sitter]: https://github.com/nicopap/tree-sitter-chirp
