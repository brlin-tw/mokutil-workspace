# mokutil-workspace

Workspace for assistance in manipulating machine owner keys of the shim bootloader.

<https://gitlab.com/brlin/mokutil-workspace>  
[![The GitLab CI pipeline status badge of the project's `main` branch](https://gitlab.com/brlin/mokutil-workspace/badges/main/pipeline.svg?ignore_skipped=true "Click here to check out the comprehensive status of the GitLab CI pipelines")](https://gitlab.com/brlin/mokutil-workspace/-/pipelines) [![GitHub Actions workflow status badge](https://github.com/brlin-tw/mokutil-workspace/actions/workflows/check-potential-problems.yml/badge.svg "GitHub Actions workflow status")](https://github.com/brlin-tw/mokutil-workspace/actions/workflows/check-potential-problems.yml) [![pre-commit enabled badge](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white "This project uses pre-commit to check potential problems")](https://pre-commit.com/) [![REUSE Specification compliance badge](https://api.reuse.software/badge/gitlab.com/brlin/mokutil-workspace "This project complies to the REUSE specification to decrease software licensing costs")](https://api.reuse.software/info/gitlab.com/brlin/mokutil-workspace)

\#shim \#secure-boot \#mokutil

## Common operations

This section documents common operations regarding the usage of the `mokutils` utility:

### List the currently-enrolled machine owner keys

Run the following command to list the currently-enrolled machine owner keys:

```bash
mokutil --list-enrolled
```

Some common machine owner keys and their source:

* Canonical Ltd. Master Certificate Authority  
  Subject: `C=GB, ST=Isle of Man, L=Douglas, O=Canonical Ltd., CN=Canonical Ltd. Master Certificate Authority`  
  SHA-1 fingerprint: `76:a0:92:06:58:00:bf:37:69:01:c3:72:cd:55:a9:0e:1f:de:d2:e0`  
  This is the key used to sign the GRUB bootloader, modules, and kernel modules of the Ubuntu distribution.
* Ventoy signing key  
  Subject: `CN=grub`  
  SHA-1 fingerprint: `54:f4:18:74:f4:d8:84:28:09:bc:be:88:10:65:92:0a:17:56:5d:25`  
  This suspicious key is used to sign the Ventoy application bootloader, I would recommend revoking the key whenever it is no longer used from the security perspective.

### Export all currently enrolled machine owner key certificates

Run the following command to export all currently enrolled machine owner key certificates:

```bash
mokutil --export
```

After running the command you should be able to locate the exported machine owner certificates at the working directory with the MOK-\*.der filenames.

### View exported certificate information

Run the following command to view the information of the specified exported machine owner key certificate file:

```bash
openssl x509 -in _certificate_file_ -noout -text
```

This allows you to verify the MOK certificate to operate on.

### Revoke user-specified machine owner keys

Run the following command to remoke a user-specified machine owner key:

```bash
sudo mokutil --delete _certificate_file_...
```

You need to first [export all the enrolled machine owner key certificates](#export-all-currently-enrolled-machine-owner-key-certificates) in order to delete them.

To complete this operation you need to specify and remember a password to complete the single-time machine owner presence validation during the system boot process, the steps are documented as the following:

1. Reboot the system.
1. When the blue background countdown prompt appears, press any key to enter the Perform MOK management menu.
1. Select the Delete MOK option in the Perform MOK management menu.
1. View details of all machine owner keys to be deleted to avoid misoperation, then select the Continue option to continue.
1. Select the Yes option in the Delete the key(s)? prompt.
1. Enter the password specified in the `mokutil --delete` command.
1. Select the Reboot option in the Perform MOK management menu.
1. After rebooting the system, [list the currently-enrolled machine owner keys](#list-the-currently-enrolled-machine-owner-keys) to verify whether the machine owner keys are really revoked.

## References

The following materials are referenced during the development of this project:

* [The output of the `mokutil --help` command](command-outputs/mokutil-help.out.txt).  
  Explains the supported options of the `mokutil` command.
* The openssl-x509 tldr page  
  Explains the command to view X.509 format exported machine owner key certificates.

## Licensing

Unless otherwise noted([comment headers](https://reuse.software/spec-3.3/#comment-headers)/[REUSE.toml](https://reuse.software/spec-3.3/#reusetoml)), this product is licensed under [version 3 of the GNU Affero General Public License](https://www.gnu.org/licenses/agpl-3.0.en.html), or any of its more recent versions of your preference.

This work complies to [the REUSE Specification](https://reuse.software/spec/), refer to the [REUSE - Make licensing easy for everyone](https://reuse.software/) website for info regarding the licensing of this product.
