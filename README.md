# Python libraries for smarter Vim coding

## Installation

Just add plugin using [Plug](https://github.com/junegunn/vim-plug):

```viml
Plug 'seletskiy/vim-pythonx'
```

Import required py-modules in snippets files (e.g. in `go.snippets`):

```
global !p
import go
endglobal
```

## PHP

Thx [@kovetskiy](https://github.com/kovetskiy).

### `cycle_by_var_name(identifiers=[])`
Will get identifier under cursor and either complete it to the full variable
name from previous code in file, or substitute identifier with previous
variable declaration.

![cycle by var name php](https://cloud.githubusercontent.com/assets/8445924/6058137/65e9eb92-ad1c-11e4-999b-ed4cf59fba98.gif)

### `get_phpdoc_variables_before_cursor()`
Returns key-value data from phpdoc before current cursor position. Sometimes value may be array.

Getter/setter snippets, based on this and following functions:

![getter/setter php](https://cloud.githubusercontent.com/assets/8445924/6056755/7fc473a2-ad0d-11e4-9d79-3ac5e8988f21.gif)

Snippets from the above gif: [php_vim_pythonx.snippets](https://github.com/kovetskiy/dotfiles/blob/master/.vim/UltiSnips/php_vim_pythonx.snippets)

### `get_class_name_with_underscored_namespaces(path="")`
Returns a class name with expanded namespaces from path variable.

### `get_camelcase_identifier_from_string(string, with_dollar=True)`
Returns camelCase identifier from string, for example:  
`protected $_storage;`  
returns just `Storage`.

## Go

### `cycle_by_var_name(identifiers=[])`

Will get identifier under cursor and either complete it to the full variable
name from previous code in file, or substitute identifier with previous
variable declaration

![cycle by var name](https://cloud.githubusercontent.com/assets/674812/5943979/32561378-a745-11e4-92a9-e28618dc4c09.gif)

#### Usage with UltiSnips

In `go.snippets`:

```
snippet i "if "
if ${1:`!p snip.rv=go.get_last_var_for_snippet()`} {
        ${2:${VISUAL}}
}
endsnippet
```

In `.vimrc`:

```viml
au FileType go inoremap <C-L> <C-\><C-O>:py go.cycle_by_var_name()<CR>
au FileType go smap <C-L> <BS><C-L>
```

`i` will expand into if with last used variable as condition. Use `<C-L>` to
select previous variables.

### `extract_prev_method_binding_for_cursor()`

Returns binded variable name and type name for inserting into method declarations:

```go
func (someVar TypeForSomeVar) MethodName() {
    // come code
}
```

Method will deduce tuple `(someVar TypeForSomeVar)` from either previous method
declaration or type declaration.

![extract_prev_method_binding_for_cursor](https://cloud.githubusercontent.com/assets/674812/5944082/0e46ff28-a746-11e4-8cf6-3e67e639e872.gif)

#### Usage with UltiSnips

In `go.snippets`:

```
snippet d "define method"
func (`!p snip.rv=' '.join(go.extract_prev_method_binding_for_cursor())`) $1($2) $3${3/.+/ /}{
        $4
}
endsnippet

snippet dp "define method on pointer"
func (`!p snip.rv=' *'.join(go.extract_prev_method_binding_for_cursor())`) $1($2) $3${3/.+/ /}{
        $4
}
endsnippet
```

### `guess_package_name_from_file_name(path)`

Return basename part of the path for using as new package name.

### `get_previous_slice_usage()`

Returns previous used slice name.

### `split_parenthesis()`

Split line in parenthesis into multiple lines.

_TBD: gif_

### `get_last_var_for_snippet()`

Same, asd `cycle_by_var_name`, but for using in snippet.

### `get_last_used_var(prev_var='', identifiers=[])`

Filter `identifiers` list and return first variable. If `prev_var` is
specified, then identifier list will be scanned either for exact match for
`prev_var`, and then return variable that came just before; if `prev_var` is
matching prefix for some var, then return value will be completed value.

### `get_all_last_used_identifiers()`

Return list of identifiers that are looks like variables.

### `get_last_defined_identifiers()`

Return list of identifiers that are looks like just declared using
`:=`, `=` or passed via arguments.

### `get_identifier_under_cursor()`

Return identifier under cursor.

### `get_imports()`

Return imports list with package names from current file.

### `path_to_import_name(path)`

Return package name from import name.

### `get_package_name_from_file(path)`

Return package name from specified file.
