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

Based on the XDG specification, Helm automatically creates three different default directories on each operating system as required:

| Operating system | Cache path                 | Configuration path              | Data path                |
| ---------------- | -------------------------- | ------------------------------- | ------------------------ |
| Window           | %TEMP\helm                 | %APPDATA%\helm                  | %APPDATA%\helm           |
| macOS            | \$HOME/Library/Caches/helm | \$HOME/Library/Preferences/helm | \$HOME/Library/helm      |
| Linux            | \$HOME/.cache/helm         | \$HOME/.config/helm             | \$HOME/.local/share/helm |

`**cache path**` for charts that are downloaded from upstream chart repositories, Installed charts are cached to the local machine enable faster installation next time it is referenced. To upcate the cache, run `helm repo update`.

`**configuration path**` save repo information that was added by running `helm repo add`

`**data path**` is used to store plugins. The plugin data is stored in this location after using `helm plugin install`

### Authorization/RBAC

Kubernetes uses **role-based access control (RBAC)**.

Kubernetes provides many different roles on the platform. Three common roles are listed here:

- `cluster-admin`: Allows a user to perform any action against any resource throughout the cluster
- `edit`: Allows a user to read and write to most resources within a namespace or a logical grouping of Kubernetes resources
- `view`: Prevents a user from modifying existing resources, and only allows users to read resources within a namespace

=> Helm authenticates to Kubernetes using the credentials defined in the `kubeconfig` file, Helm is given the same level of access as the users defined in the file.
If `edit` access is enabled, Helm can be assumed to have sufficient permission to install applications.

### Values Types

Values in a YAML file can be of different types. Them most common type is a string, which is a text value.
String can be declared by wrapping a value in quotations, but this is not always required. If a value contains at least one alphabetical letter or special character, the value is considered a string, with or without quotation marks. Multi-line strings can be set by using the pipe (|) symbol, as shown:

```yaml
configuration: |
  server.port=8443
  logging.file.path=/var/log
```

values can be integers. A value is an integer when it is a numeric character that is not wrapped in quotations.

```yaml
replicas: 1
```

Compare to the following YAML, which assigns replicas to a string value:

```yaml
replicas: "1"
```

Boolean values can be declared with either true or false:

```yaml
ingress:
  enable: true
```

Other acceptable Boolean values are `yes, no, on off, y, n, Y and N`.

Values can be set to more complex types, such as lists. Items in a list in YAML are identified by the dash ( - ) symbol

```yaml
servicePorts:
  - 8080
  - 8443
```

This YAML sets servicePorts to the list of integers (such as 8080 and 8443). This syntax can also be used to describe a list of objects:

```yaml
deployment:
  env:
    - name: MY_VAR
      value: MY_VALUE
    - name: SERVICE_NAME
      value: MY_SERVICE
```

### The Helm chart structure

In order to be considered a Helm chart, a certain file structure must be followed:

```
my-chart/
  # chart files and directories
```

| **File/directory**  | **Definition**                                                                                                                 | **Required?**                                                                                                            |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Chart.yaml          | A file that contains metadata about the Helm chart.                                                                            | Yes.                                                                                                                     |
| templates/          | A directory that contains Kubernetes resources in YAML format.                                                                 | Yes, unless dependencies are declared in Chart.yaml                                                                      |
| templates/NOTES.txt | A file that can be generated to provide usage instructions during chart installation                                           | No.                                                                                                                      |
| values.yaml         | A file that contains the chart's default values.                                                                               | No, but every chart should contain this file as a best practice.                                                         |
| .helmignore         | A file that contains a list of files and directories that should be omitted from the Helm chart's packaging                    | No.                                                                                                                      |
| charts/             | A directory that contains charts that the Helm chart depends on.                                                               | Does not need to be explicitly provided as Helm's dependency management system will automatically create this directory. |
| Chart.lock          | A file used to save the previously applied dependency versions.                                                                | Does not need explicitly provided as Helm's dependency management system will automatically create this file.            |
| crds/               | A directory that contains Custom Resource Definition (CRD) YAML resources to be installed before resources under `templates/`. | No.                                                                                                                      |
| README.md           | A file that contains installation and usage information about the Helm chart.                                                  | No, but every Helm chart should contain this file.                                                                       |
| LICENSE             | A file that contains the chart's license.                                                                                      | No.                                                                                                                      |
| values.schema.json  | A file that contains the chart's values schema in JSON format.                                                                 | No.                                                                                                                      |
