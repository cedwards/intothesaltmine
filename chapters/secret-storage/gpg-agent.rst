The GPG Agent
=============

In order to publish GPG encrypted secrets using a passphrase-enabled key you'll
need to run a GPG agent. This agent will allow you to authenticate once to the
encryption key and not require a passphrase be entered anytime someone requests
a key. This provides the added security of a passphrase on the encryption key,
but the usability of not requesting a passphrase on every request.

This step of the process requires an update to a configuration file as well as
manually unlocking the GPG key. I will again mention that this process is
currently manual and will need to be repeated anytime the system is restarted
and the GPG agent restarted.

gpg-agent.conf
--------------

In order for Salt to prompt you for the passphrase it needs to know how to do
so. This can be defined within the ``gpg-agent.conf`` file, which you'll likely
need to create inside the ``/etc/salt/gpgkeys`` directory. This file simply
holds configuration on how the agent should run. In our basic setup you'll only
need to add a single line to this new file. The example below shows a unified
diff of the file. Add the line(s) as defined by the + character, but do not add
the + character itself.

.. code-block:: diff

    + pinentry-program /usr/bin/pinentry-curses

gpg-agent
---------

In order for this component to work you'll need to manually launch a GPG agent
and tell Salt where it can be found. (Note: I'm still working on automating
this piece, but until then it'll require just a touch of copy-paste).

A ``gpg-agent`` is a tool that creates a socket that the ``gpg`` tool can
connect to for authentication. This socket is usually created within ``/tmp/``
under the name ``/tmp/gpg-xxxxxx``, where x is a random string of alpha-numeric
characters. It isn't important to know a lot about how a ``gpg-agent`` works,
but it will be important to know *where* your agent socket is created. This
information can be gathered in two ways:

 # start the gpg-agent daemon and query for the environment values
 # start the gpg-agent daemon and manually enter the resulting values

I'll outline both methods here for clarity. Only one of these processes needs
to be followed.

eval
----

The ``eval`` command is a shell built-in that will evaluate environment
variables as they are passed from other programs. In this case it will evaluate
the output of the embedded command (surrounded by parenthesis). The
``gpg-agent`` command outputs an environment variable to be exported, and the
``eval`` shell built-in evaluates it. If you start your ``gpg-agent`` this way
you'll need to manually query for the environment values in order to provide
them to Salt. The environment variable is ``GPG_AGENT_INFO``.

.. code-block:: bash

    eval $(gpg-agent --homedir /etc/salt/gpgkeys --daemon)
    echo $GPG_AGENT_INFO
    export GPG_TTY=$(tty)

You'll need to then copy/paste the output and provide it to SaltStack. This
value, and the path it defines, is the location of the gpg-agent and is
critical towards allowing Salt to unlock the private key.

manual
------

If you don't use the ``eval`` method you can manually run the ``gpg-agent``
command and copy/paste the values where needed. Either method will work just as
well, it's simply a matter of preference. I consider this method slightly more
manual as it requires an extra step.

.. code-block:: bash

    gpg-agent --homedir /etc/salt/gpgkeys --daemon
    GPG_AGENT_INFO=/tmp/gpg-o6aR6w/S.gpg-agent:72087:1; export GPG_AGENT_INFO;
    export GPG_TTY=$(tty)

When you run the above command you'll get output similar to what you see above.
The path to the created socket, the process ID and the agent version. It is
this same information that needs to be provided to SaltStack in order to enable
access to the unlocked key.

/etc/default/salt
-----------------

In order for SaltStack to be aware of the environment variables defined by the
gpg-agent is to have them included when the Salt service is started. This can
be done by adding these variables to a file that is automatically included each
time the service is started using the standard init script. Simply copy/paste
the ``GPG_AGENT_INFO`` line into this file and restart the Salt master. This
will make the master aware of these environment variables and allow it to
access the agent and reuse the agent once it has been unlocked.

unlock the key
--------------

Once you've started the gpg-agent and provided SaltStack with the information
required to access this agent you're ready to unlock the keyring. All you
should need to do for this to happen is request a key from the Salt master.
Make sure that your pillar top file has been configured to allow the Salt
master access to a key (any key will do). Request this key using the command
below:

.. code-block:: salt

    salt-call pillar.item secret

When you run this command Salt will try to decipher the encrypted value stored
within your pillar data. Now that it knows about your gpg agent information
it'll request access through that socket. The first time it runs it'll
determine that no access has been granted and prompt you for a passphrase. You
should see a curses-based prompt appear in your terminal asking you for the
encryption password. Enter this password once and your key will be unlocked.
This should last for the duration of your session or your GPG max-cache-ttl.
