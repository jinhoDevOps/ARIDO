https://developer.hashicorp.com/terraform/cli/commands
---
# Basic CLI Features

> **Hands-on:** Try the [Terraform: Get Started](https://developer.hashicorp.com/terraform/tutorials/aws-get-started?utm_source=WEBSITE&utm_medium=WEB_IO&utm_offer=ARTICLE_PAGE&utm_content=DOCS) tutorials.

The command line interface to Terraform is the `terraform` command, which accepts a variety of subcommands such as `terraform init` or `terraform plan`.

We refer to the `terraform` command line tool as "Terraform CLI" elsewhere in the documentation. This terminology is often used to distinguish it from other components you might use in the Terraform product family, such as [Terraform Cloud](https://developer.hashicorp.com/terraform/cloud-docs) or the various [Terraform providers](https://developer.hashicorp.com/terraform/language/providers), which are developed and released separately from Terraform CLI.

To view a list of the commands available in your current Terraform version, run `terraform` with no additional arguments:

```text
Usage: terraform [global options] <subcommand> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.

Main commands:
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure

All other commands:
  console       Try Terraform expressions at an interactive command prompt
  fmt           Reformat your configuration in the standard style
  force-unlock  Release a stuck lock on the current workspace
  get           Install or upgrade remote Terraform modules
  graph         Generate a Graphviz graph of the steps in an operation
  import        Associate existing infrastructure with a Terraform resource
  login         Obtain and save credentials for a remote host
  logout        Remove locally-stored credentials for a remote host
  metadata      Metadata related commands
  output        Show output values from your root module
  providers     Show the providers required for this configuration
  refresh       Update the state to match remote systems
  show          Show the current state or a saved plan
  state         Advanced state management
  taint         Mark a resource instance as not fully functional
  untaint       Remove the 'tainted' state from a resource instance
  version       Show the current Terraform version
  workspace     Workspace management

Global options (use these before the subcommand, if any):
  -chdir=DIR    Switch to a different working directory before executing the
                given subcommand.
  -help         Show this help output, or the help for a specified subcommand.
  -version      An alias for the "version" subcommand.
```

Copy

(The output from your current Terraform version may be different than the above example.)

To get specific help for any specific command, use the `-help` option with the relevant subcommand. For example, to see help about the "validate" subcommand you can run `terraform validate -help`.

The inline help built in to Terraform CLI describes the most important characteristics of each command. For more detailed information, refer to each command's page for details.

## Shell Tab-completion

If you use either `bash` or `zsh` as your command shell, Terraform can provide tab-completion support for all command names and some command arguments.

To add the necessary commands to your shell profile, run the following command:

```bash
terraform -install-autocomplete
```

After installation, it is necessary to restart your shell or to re-read its profile script before completion will be activated.

To uninstall the completion hook, assuming that it has not been modified manually in the shell profile, run the following command:

```bash
terraform -uninstall-autocomplete
```
