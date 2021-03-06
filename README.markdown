# .env

**Version: 0.2.3**

Is a comprehensive, cascading bash environment system under heavy development.  It is not recommended for those just beginning to use the Bash shell.  However, veteran Bash users may find it useful.

## Install ##

To use it:

I recommend you fork the project in GitHub, so that your changes can be easily incorporated back into the tree.

Clone the project into your home directory,

    cd $HOME
    git clone git://github.com/<your github username>/.env.git

...then add the following to your `$HOME/.bashrc` file:

    [[ -r $HOME/.env/source.sh ]] && . $HOME/.env/source.sh

### Load on Login ###

Or you can source it manually once and then run the `.env load_on_login` command like this:

    . $HOME/.env/source.sh
    load_on_login

... which will add the above source command to your `.bashrc` file.

### Load on Alias ###

Or you can source it manually and then run the `.env load_on_alias` command like this:

    . $HOME/.env/source.sh
    load_on_alias

...which will setup the `ees` alias which will source .env whenever you enter `ees` after logging in.  I use this method on all of my remote hosts that are used by other people.  That way they don't notice any difference when they login unless they run `ees`.

## Customize your local environment ##

The convention is, `$HOME/.env/host/[your-hostname]` directory contains these files:

* alias.sh
* env.sh
* functions.sh
* prompt.sh

... and anything else you wish to customize your own local environment.

You can put any `*.sh` files in your host directory that you want, and they will only get sourced when you start a Bash shell on the machine with the specified hostname.

The easiest way to customize is to run `configthis.env`, which will create a folder in the `host` directory specifically for your machine and stub out some empty files that you can, and should, customize.

You are not required to follow this file-naming convention.  But the files must have a `.sh` extension.

### Example: ###

Let's say your hostname is `Harry-Potters-Macbook.local`, and you want to add `/usr/local/bogus` to your path.  Here's how you do it:

    configthis.env

This command will stub out a few files as examples and `cd` to the directory where they are `$HOME/.env/host/Harry-Potters-Macbook.local`.  Now you edit each of those files, and delete the ones you don't need.  So, since we are only going to add to the path let's remove the other files, then edit the path.sh file.

    rm alias.sh env.sh functions.sh prompt.sh

Inside `path.sh` add this line:

    pathmunge "/usr/local/bogus"

Done!

Now you can source those changes `. $HOME/.env/source.sh` and your path will include the new entry.  Next time you start a shell it will maintain your path and you'll never have duplicates.

## Themes ##

.env themes are located in `$DOT_ENV_PATH/global/theme/`.  To use a theme set `bash_prompt='THEME_NAME'`. e.g.,

    bash_prompt='spark'

A variant of the `spark` theme is default and will be loaded if no theme is specified.  Please send me a pull request if you create your own themes.

## Other Functions ##

### Add your SSH key to a remote host ###

    add_ssh_key_to_host [user@]HOSTNAME

... which will append your public `~/.ssh/id_dsa.pub` key to the host's authorized_keys file and allow you to login without a password.

### Propagate your copy of .env to other hosts ###

If you want to copy your .env to another host, simply type:

    propagate_env_to_host [user@]HOSTNAME

... which will compress your local .env, copy it to the specified host and decompress it.

If the remote machine is used by other people beside yourself, you can add this to the .bashrc file:

    alias .env='. $HOME/.env/source.sh'

Then whenever you login, to get your .env sourced, just type `.env`

If the remote machine is only used by you, you can add this to the .bashrc file:

    [[ -r $HOME/.env/source.sh ]] && . $HOME/.env/source.sh

Then your own OS specific `.env` will automatically be sourced when you login.

If you want to customize the remote host environment even further:

* Run `confighost.env HOSTNAME`
* Edit the newly created environment files
* Then `propagate_env_to_host HOSTNAME`

To determine the correct `HOSTNAME` to use for your remote environment make sure you first login to that environment and type `echo $HOSTNAME`.

## Make it better

Please feel free to fork it, branch it, change it and send me a pull request.

Cheers,

-- Midwire
