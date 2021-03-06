.. _faq:

Frequently Asked Questions
==========================

.. index:: FAQ

How can I create users in the privacyIDEA Web UI?
-------------------------------------------------

So you installed privacyIDEA and want to enroll tokens to the users and are
wondering how to create users.

privacyIDEA can read users from different existing sources like LDAP, SQL,
flat files and SCIM.

You very much likely already have an application (like your VPN or a Web
Application...) for which you want to increase the logon security. Then this
application already knows users. Either in an LDAP or in an SQL database.
Most web applications keep their users in a (My)SQL database.
And you also need to create users in this very user database for the user to
be able to use this application.

Please read the sections :ref:`useridresolvers` and :ref:`userview` for more
details.

But you also can define and editable SQL resolver. I.e. you can edit and
create new users in an SQL user store.

If you do not have an existing SQL database with users, you can simple create
a new database with one table for the users and according rows.

So what's the thing with all the admins?
----------------------------------------

.. index:: admin accounts, pi-manage

privacyIDEA comes with its own admins, who are stored in a database table
``Admin`` in its own database (:ref:`code_db`). You can use the tool
``pi-manage`` to
manage those admins from the command line as the system's root user. (see
:ref:`installation`)

These admin users can logon to the WebUI using the admin's user name and the
specified password.
These admins are used to get a simple quick start.

Then you can define realms (see :ref:`realms`), that should be administrative
realms. I.e. each user in this realm will have administrative rights in the
WebUI.

.. note:: Use this carefully. Imagine you defined a resolver to a specific
   group in your Active Directory to be the pricacyIDEA admins. Then the Active
   Directory domain admins can
   simply add users to be administrator in privacyIDEA.

You define the administrative realms in the config file ``pi.cfg``, which is
usually located at ``/etc/privacyidea/pi.cfg``::

   SUPERUSER_REALM = ["adminrealm1", "super", "boss"]

In this case all the users in the realms "adminrealm1", "super" and "boss"
will have administrative rights in the WebUI, when they login with this realm.

As for all other users, you can use the :ref:`policy_login_mode` to define,
if these administrators should login to the WebUI with their userstore password
or with an OTP token.

What are possible rollout strategies?
-------------------------------------

.. index:: rollout strategy

There are different ways to enroll tokens to a big number of users.
Here are some selected high level ideas, you can do with privacyIDEA.

Autoenrollment
~~~~~~~~~~~~~~

Using the :ref:`autoassignment` policy you can distribute physical tokens to
the users. The users just start using the tokens.

.. _faq_registration_code:

Registration Code
~~~~~~~~~~~~~~~~~

If your users are physically not available and spread around the world, you can
send a registration code to the users by postal mail. The registration code
is a special token type which can be used by the user to authenticate with 2FA.
If used once, the registration token get deleted and can not be used anymore.
While logged in, the user can enroll a token on his own.

How can I translate to my language?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The web UI can be translated into different languages. The system determines
the preferred language of you browser and displays the web UI accordingly.

At the moment "en" and "de" are available.

Setup translation
.................
The translation is performed using grunt. To setup the translation
environment do::

   npm update -g npm
   # install grunt cli in system
   sudo npm install -g grunt-cli

   # install grunt in project directory
   npm install grunt --save-dev
   # Install grunt gettext plugin
   npm install grunt-angular-gettext --save-dev

This will create a subdirectory *node_modules*.

To simply run the German translation do::

   make translate

If you want to add a new language like Spanish do::

   cd po
   msginit -l es
   cd ..
   grunt nggettext_extract
   msgmerge po/es.po po/template.pot > po/tmp.po; mv po/tmp.po po/es.po

Now you can start translating with your preferred tool::

   poedit po/es.po

Finally you can add the translation to the javascript translation file
``privacyidea/static/components/translation/translations.js``::

   grunt nggettext_compile

.. note:: Please ask to add this translation to the Make directive
   *translation* or issue a pull request.


How can I setup HA (High Availability) with privacyIDEA?
--------------------------------------------------------

.. index:: HA

privacyIDEA does not track any state internally. All information is kept in
the database. Thus you can configure several privacyIDEA instances against one
DBMS [#dbms]_ and have the DBMS do the high availability.


.. note:: The passwords and OTP key material in the database is encrypted
   using the *encKey*. Thus it is possible to put the database onto a DBMS
   that is controlled by another database administrator in another department.

Read more about :ref:`ha_setups`.

.. rubric:: Footnotes

.. [#dbms] Database management system

Are there shortcuts to use the Web UI?
--------------------------------------

.. indes:: shortcuts, hotkeys

I do not like using the mouse. Are there hotkeys or shortcuts to use the Web UI?

With version 2.6 we started to add hotkeys to certain functions. You can use
'?' to get a list of the available hotkeys in the current window.

E.g. you can use ``alt-e`` to go to the *Enroll Token* Dialog and ``alt-r`` to
actually enroll the token.

For any further ideas about shortcuts/hotkeys please drop us a note at github
or the google group.

How to copy a resolver definition?
----------------------------------

Creating a user resolver can be a time consuming task. Especially an LDAP
resolver needs many parameters to be entered. Sometimes you need to create a
second resolver, that looks rather the same like the first resolver. So
copying or duplicating this resolver would be great.

You can create a similar second resolver by editing the exiting resolver and
entering a new resolver name. This will save this modified resolver
definition under this new name. Thus you have a resolver with the old name
and another one with the new name.


What drivers are available for MySQL or Maria DB?
-------------------------------------------------

Please read :ref:`mysqldb`.
