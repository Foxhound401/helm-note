### basic command

Five `repo` subcommands:

- `add`: To add a chart repository
- `list`: To list chart repositories
- `remove`: To remove a chart repo
- `update`: To update information on available charts locally from chart repo
- `index`: To generate an index file given a directory containing package charts

### list of plugins

https://helm.sh/docs/community/related/

Helm provides a `plugin` subcommands for managing plugins, which contain further subcommands, described in the following table:

| PLugin      | Description                         | Usage                           |
| ----------- | ----------------------------------- | ------------------------------- |
| `install`   | Installs one or more Helm plugins   | `helm plugin install $URL`      |
| `list`      | List installed Helm plugins         | `helm plugin list`              |
| `uninstall` | Uninstalls one or more Helm plugins | `helm plugin uninstall $PLUGIN` |
| `update`    | Updates one or more Helm plugins    | `helm plugin update $PLUGIN`    |

examples of the upstream plugins:

- `helm diff`: Performs a diff between a deployed release and a proposed Helm upgrade
- `helm secrets`: Used to help conceal secrets from Helm charts
- `helm monitor`: Used to monitor a release and perform a rollback if certain events occur
- `helm unittest`: Used to perform unit testing on a Helm chart

### Environement Variables

Helm relies on the existence of externalized variables to configure low-level options. The Helm documentation lists six primary environement variables used to configure Helm:

- `XDG_CACHE_HOME`: Sets an alternative location for storing cached files
- `XDG_CONFIG_HOME`: Sets an alternative location for storing Helm configuration
- `XDG_DATA_HOLME`: Sets an alternative location for storing Helm data
- `HELM_DRIVER`: Sets the backend storage driver
- `HELM_NO_PLUGINS`: Disables plugins
- `KUBECONFIG`: Sets an alternative Kubernetes configuration file
