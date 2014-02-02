# NAME

tnl

# SYNOPSIS

    $ tnl help                # Show basic help
    $ tnl tnlrc               # Show setup of $HOME/.tnlrc file
    $ tnl man                 # Complete tnl manpage
    $ tnl start gatwy1        # Starts tunnel
    $ tnl ssh tomcat@syssrv3  # Logs in as user "tomcat" on system you called "syssrv3"
    $ tnl ssh jenkins         # Logs into on system you call Jenkins as current user
    $ tnl lsgwy               # Shows gateways and status
    $ tnl ls           	      # Shows configured systems as for that gateway
    $ tnl stop gatwy3         # Stops the tunnel gatwy3

# DESCRIPTION

SSH Tunnel Manager

This program helps you manage your ssh tunnels. Sometimes, in order
to log into one system, you must first ssh into another system, and 
then ssh into the system you want. You can simpify this by using ssh
tunnel manager.

This program allows you to setup a SSH tunnel and systems you want to
tunnel to using a file called '.tnlrc' in your $HOME directory.

# SUB-COMMANDS

The following commands can be used by `tnl`:

- `tnl ls`

    Lists all defined systems. This will list the _Name_ you call the
    system, the actual system name, the user who should log into that system
    if it is not the default user. The tunnel port it is on, the debug
    setting, and the Cypher (Crytography) setting.

    An astrisk on the begging of the list means that system's tunnel is
    active.

- `tnl lsgwy`

    Lists all defined gateways. This will list the PID of the gateway
    process if it is active, the name you gave the gateway, and the actual
    system name.

- `tnl start <gateway>`

    This will start the gateway <gateway>, and the tunnels for all systems
    that can use that gateway.

- `tnl stop <gateway>`

    This will stop the gateway <gateway> and all tunnels running on that
    gateway.

- `tnl ssh <name>`

    This will start the system <name> where <name> is the name you gave the
    system. If this name has a defined user, this user will be logged in
    rather than the default user. Note that you can have the same system
    defined with multiple _names_. This allows you to have multiple user
    names for the same system.

- `tnl ssh <user>@<name>`

    Same as the above, but forces the system to log you in as user <user>
    instead of the user defined for <name>.

- `tnl help`

    Shows the synopsis on using the `tnl` command.

- `tnl tnlrc`

    Shows you information on setting up the `tnlrc` file.

- `tnl man`

    Gives you the complete manpage for the `tnl` command.

# TUNNEL RESOURCE FILE

The Tunnel Reource File is located at "$HOME/.tnlrc" and consists of
multiple lines. Each line can be either a __Gateway Description__ or a
__System Description__.

A __Gateway Description__ line must begin with `gateway:`) _Note the
colon on the end of the string!_ A __System Description__ line must
begin with `system:` _Note the colon on the end of the string!_.

After the `gateway:` or `system:` string is a set of parameters that
configure either the system or the gateway. You need at least one
__Gateway__ line. (Otherwise, why do you need to manage SSH Tunnels?) and
at least one __System__ line. (Otherwise, what systems are you tunneling
to?)

## Gateway Configuration

    gateway: -name gateway -system corgwy1.prod.local -user bsmith -port 2000

A __Gateway__ system has the following parameters:

- `-name`

    (_Required_) The name you are giving the gateway system.

- `-system`

    (_Required_) The actual name of the system or its IP address.

- `-start`

    (_Optional_) The starting port number to use for tunnelling. If this is 2000,
    then the first system will tunnel through port 2000, the next defined one will
    tunnel through port 2001, etc. Systems can override these settings by using
    their own port number.

- `-crypt`

    (_Optional_) The cryptography type to use over the tunnel. The default
    is `3des`\>.  This setting depends upon your system `ssh` command and
    whether you are using Protocol 1 or Protocol 2. In Protocol 1, you can
    only use a single cryptography method. In Protocol 2, you can use
    multiple ones, each separated by a common.

- `-port`

    (_Optional_) The port to use for the connection. Default is the default
    SSH port 22.

- `-debug`

    (_Optional_) The debugging level for this connection. Valid values are
    from 0 (for no debugging) to 3 (for maximum information). Default is 0.

## System Configuration

    system: -name build -system jenkins2.prod.local
    system: -name depoly -system maven.prod.local -gateway gateway3

- `-name`

    (_Required_) The name you are giving the system. This is the system
    alias you need to use. You cannot use the system name.

- `-system`

    The actual system name or its IP address.

- `-user`

    The user you want to log in as. You may use the syntax C(<user@name>) to
    override this.

- `-gateway`

    This system must use this particular gateway. This allows you to have
    multiple gateways installed and specify that a particular client can
    only be on that gateway.

- `-x11`

    Whether or not you want to tunnel X11 through this connection. This
    parameter has no value after it.

- `-port`

    The port you want to use for tunneling. This overrides the setting used
    in the Gateway's `-start` parameter.

- `-crypt`

    The crytography type you want to use for this connection. See your
    system's _ssh manpage_ for the proper values to use.

- `-debug`

    (_Optional_) The debugging level for this connection. Valid values are
    from 0 (for no debugging) to 3 (for maximum information). Default is 0.

# BUGS

- This command is heavily dependent upon the system's `ssh`
command. A better version may use the Net::SSH::Perl.
- This command does not verify the commands before sending them off
to the system `ssh` command.

# AUTHOR

David Weintraub <david@weintraub.name>

# COPYRIGHT

Copyright © 2014 by the author, David Weintraub. All rights reserved. This
program is covered by the open source BMAB license.

The BMAB (Buy me a beer) license allows you to use all code for whatever reason
you want with these three caveats:

1. If you make any modifications in the code, please consider sending them to me,
so I can put them into my code.  
2. Give me attribution and credit on this
program. 
3. If you're in town, buy me a beer. Or, a cup of coffee which is what
I'd prefer. Or, if you're feeling really spendthrify, you can buy me lunch. I
promise to eat with my mouth closed and to use a napkin instead of my sleeves.
