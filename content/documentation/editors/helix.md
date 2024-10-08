---
.title = "Helix",
.date =  "2004-01-01T00:00:00",
.author = "Loris Cro",
.draft = false,
.layout = "documentation.shtml",
.tags = [],
---
# Helix
 
## 1. Copy Tree Sitter queries

In your Helix **runtime directory** (https://docs.helix-editor.com/install.html#configuring-helixs-runtime-files), copy the Tree Sitter queries from our parsers.  
  From the root of the [Ziggy repository](https://github.com/kristoff-it/ziggy), run the following two commands after replacing `HELIX_RUNTIME_PATH`:
  - `cp -rT tree-sitter-ziggy/queries HELIX_RUNTIME_PATH/queries/ziggy`
  - `cp -rT tree-sitter-ziggy-schema/queries HELIX_RUNTIME_PATH/queries/ziggy_schema`  

NOTE: '-T' makes it so you can run the command multiple times without nesting new copies of `queries` more deeply than intended. Also macOS doesn't support it, sorry.
    
## 2. Add Ziggy (and Ziggy Schema) to your language config 
In your Helix **config directory** (usually `~/.config/helix/`)create `languages.toml` and copy the following lines:
```toml

[language-server.ziggy-lsp]
command = "ziggy"
args = ["lsp"]

[[language]]
name = "ziggy"
scope = "text.ziggy"
roots = []
injection-regex = "ziggy|zgy"
file-types = ["ziggy", "zgy"]
comment-token = "//"
auto-format = true
formatter = { command = "ziggy" , args = ["fmt", "--stdin"] }
language-servers = [ "ziggy-lsp" ]

[[grammar]]
name = "ziggy"
source = { git = "https://github.com/kristoff-it/ziggy", rev = "0e46579ed878bb28a78cf624c2e593eb39301648", subpath = "tree-sitter-ziggy" }

[[language]]
name = "ziggy_schema"
scope = "text.ziggy_schema"
roots = []
injection-regex = "ziggy-schema|zgy-schema"
file-types = ["ziggy-schema", "zgy-schema"]
comment-token = "///"
indent = { tab-width = 4, unit = "    " }
formatter = { command = "ziggy" , args = ["fmt", "-", "--type", "schema"] }
auto-format = true
language-servers = [ "ziggy-schema-lsp" ]

[[grammar]]
name = "ziggy_schema"
source = { git = "https://github.com/kristoff-it/ziggy", rev = "0e46579ed878bb28a78cf624c2e593eb39301648", subpath = "tree-sitter-ziggy-schema" }

[language-server.ziggy-schema-lsp]
command = "ziggy"
args = ["lsp", "--schema"]
```

 
## 3. Build Tree Sitter grammars

Run `hx --grammar build`, it should report that `ziggy` and `ziggy_schema` were built successfully.



