---
title: Install the Azure CLI on Linux with yum
description: How to install the Azure CLI with yum
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/09/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azurecli
---

# Install Azure CLI with yum

For Linux distributions with  `yum` such as RHEL, Fedora, or CentOS, there's a package
for the Azure CLI. This package has been tested with RHEL 7, Fedora 19 and higher, and CentOS 7.

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## Install

1. Import the Microsoft repository key.

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. Create local `azure-cli` repository information.

   ```bash
   sudo sh -c 'echo -e "[azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/azure-cli
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
   ```

3. Install with the `yum install` command.

   ```bash
   sudo yum install azure-cli
   ```

Run the Azure CLI with the `az` command. To sign in, use [az login](/cli/azure/reference-index#az-login) command.

[!INCLUDE [interactive-login](includes/interactive-login.md)]

To learn more about different authentication methods, see [Sign in with Azure CLI](authenticate-azure-cli.md).

## Troubleshooting

Here are some common problems seen when installing with `yum`. If you experience a problem not covered here, [file an issue on github](https://github.com/Azure/azure-cli/issues).

### Proxy blocks connection

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

You may also want to explicitly configure `yum` to use this proxy at all times. Make sure that the following
lines appear under the `[main]` section of `/etc/yum.conf`:

```yum.conf
[main]
# ...
proxy=http://[proxy]:[port] # If your proxy requires https, change http->https
proxy_username=[username] # Only required for basic auth
proxy_password=[password] # Only required for basic auth
```

In order to get the Microsoft signing key and get the package from our repository, your proxy needs to
allow HTTPS connections to the following address:

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## Update

Update the Azure CLI with the `yum update` command.

```bash
sudo yum update azure-cli
```

## Uninstall

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. Remove the package from your system.

   ```bash
   sudo yum remove azure-cli
   ```

2. If you don't plan to reinstall the CLI, remove the repository information.

   ```bash
   sudo rm /etc/yum.repos.d/azure-cli.repo
   ```

3. If you don't use any other Microsoft packages, remove the signing key.

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## Next Steps

Now that you've installed the Azure CLI, take a short tour of its features and common commands.

> [!div class="nextstepaction"]
> [Get started with the Azure CLI](get-started-with-azure-cli.md)
