cfengine-promises
=================

CFEngine 3 Promises we have written at Morningside College.

These presume that you are using the COPBL which is available
from http://github.com/cfengine/copbl as cfengine_stdlib.cf.

Additionally, we dole out bundle sequences in the following 
fashion (Important: most general to most specific ordering
under the methods clause) in ``promises.cf``:

```cfengine3
body common control 
{

  ...

  bundlesequence => {
                     ...
                     "mside",
                     ...
                    };

  inputs => {
              ...
              "system_vmware_tools.cf",
              ...
            };

  ...

}

bundle agent mside 
{
  vars:
      classes:
      "servers" or => {   "server_A"
                        , "server_B"
                        , "server_C" 
                      };

      "vms" or => {  "server_A"
                   , "server_C"
                  };

  methods:
      any::
      "any" usebundle => additional_package_repos;

      servers::
      "servers" usebundle => edit_sshd;
      "servers" usebundle => ensure_nagios_nrpe;
      "servers" usebundle => install_ssh_keys;

      vms::
      "vms" usebundle => install_vmware_tools;

      server_A::
      "server_A" usebundle => app_webs;
 
  reports:
      cfengine_3::
      "Running under bundle agent mside.";
}
```
