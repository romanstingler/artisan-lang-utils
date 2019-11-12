This package contains artisan commands to import/export and work with 
language resources.

# Install

In composer.json add the following to repositories:

```
    "repositories": [{ 
        "type": "vcs", 
        "url":  "git@git.symvaro.com:dev/artisan-lang-utils.git" 
    }]
```

and require:

```
    "symvaro/artisan-lang-utils": "dev-master"
```

Then add the following service provider to config/app.php

```
    'providers' => [
        ...
        Symvaro\ArtisanLangUtils\ArtisanLangUtilsServiceProvider::class,
    ]
```

# Use

__Warning:__ The artisan lang commands will alter the language files
when used for editing or import. That means every content except
keys and values will be removed (e.g. comments), variables will be
replaced with their values and nested array structures will be flattened.
Use these tools only in combination with a VCS.

## Export

To export the languages strings in the supported formats use the command like:

```sh
php artisan lang:export --language=en --format=po filename.po
```

If the filename is omitted, stdout will be used.

## Import

The import command line api is similarly structured like the export.

```sh
php artisan lang:import --language=en --format=po filename
```

It will read from stdin, if no filename is specified. There is also 
the `--replace-all` option, which will remove language strings,
if they are not present in the import file.

## Examples

The commands can be combined with common shell utils. For example to
__list all non unique messages__ you can use this:

```sh
./artisan lang:export \
    | awk -F"\t" '{ print $2 }' \
    | sort | uniq -c | sort -rn \
    | grep -vE "^[ ]*1"
```

