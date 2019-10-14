# Catalog

**State**: *Proposal*

### Goals:
* Provide a default repository of webhooks for users to easily use.
* Allow users to provide their own curated sources of webhooks.

## Configuring the catalog

To configure the catalog, we will introduce a mechanism to allow configuration
of `hook`.

We will look for a configuration file in `$HOOK_CONFIG_PATH/config.yaml`. If
not set, `$HOOK_CONFIG_PATH` will default to `$HOME/.config/hook`.

### Configuration format

Configuration will be specified in yaml, and will map local aliases to remote
git repositories.

```yaml
catalog:
  remote:
    - name: eddiezane
      url: https://github.com/eddiezane/catalog1
    - name: wlynch
      url: https://github.com/wlynch/catalog2
      revision: my-branch
```

#### Reference

Field    | Description
---------|------------
name     | Catalog name. Used to reference the catalog in commands. Must match `^[a-zA-Z0-9]+$|^@$`
url      | [Git URL](https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a) to clone.
revision | Git revision to checkout.

Remote URL configurations will be stored under `remote` in order to leave room
for future catalog settings.

Remote catalogs are processed in order, with the last one winning. (e.g. if you
specify the same name twice, the last config will be the one that applies).

## CLI usage

Catalog resources can be referenced from the CLI, but using specify a hook as
`catalog@path`, where `catalog` corresponds to the remote catalog name.

## Config caching

To enable ease of offline development, `hook` will cache remote catalogs into
`$HOOK_CACHE_PATH/<catalog name>`. If not set, `$HOOK_CACHE_PATH` defaults to
`$HOME/.cache/hook`.

## Default catalog

By default, hook will come installed with a default remote catalog (URL TBD)
with commonly used webhooks. This will be referenced in the CLI by `@path`
(e.g. empty catalog name). To override this, you can specify a remote catalog
name of `@` in your `config.yaml`.

```yaml
catalog:
  remote:
    # Override the default remote.
    - name: @
      url: https://github.com/wlynch/catalog
```
