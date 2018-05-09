---
layout: post
title: man pam
categories: [linux]
---

PAM (Pluggable Authentication Modules) handle authentication tasks of applications on a system. In the past, any program that needed to authenticate users would directly read the /etc/passwd file; as various novelties in password storage (e.g. hashing) were developed, any application which directly read the file would have to be updated to handle new formats of password data. This lead to the development of PAM as a standard library which applications could call to authenticate users. The actual authentication process is abstracted for the calling application into a config file which lists methods through which authentication can be obtained. An example of a PAM config file for `passwd` is shown below:

```
#%PAM-1.0
auth     requisite      pam_nologin.so
auth     include        common-auth
account  include        common-account
password include        common-password
session  required       pam_loginuid.so
session  include        common-session
session  optional       pam_mail.so standard
```

Although these fields will be explained more thoroughly later, essentially what a PAM config file will do is perform some action for each line, with the main action being the `pam_xxxxx.so` file for the desired method of authentication. There might be other setup or teardown actions before and after, and some actions might be mandatory for success while others might be optional.

####pam.d config

######__service__
The service is the name of the corresponding application, for example `login` or `su`. This is the main difference between the `/etc/pam.conf` and `/etc/pam.d/*` implementations - lines in the `/etc/pam.conf` file have the service specified as the first field on each line, while files within the `/etc/pam.d/*` directory have the `service` as the name of the file (in lowercase) and omit the `service` field from all lines within the file.

There is one reserved service name, `other`, which is reserved for supplying default rules in the event that no config is found for the `service` which calls PAM.

__type__ `(account, auth, password, session)`

**authentication vs account management**  
authenication is confirming that the user/password is valid - account management is checking that on top of the user/password being valid, they are allowed to do that thing at that time. (E.g. account isn't locked, other rules on the account etc.)

* __control__ `(required, requisite, sufficient, optional, include, substack)`

* __module-path__
* __module-arguments__

```
...
pam_start(...);                Initializes the PAM library
...
if ( ! pam_authenticate(...) ) Authenticates using "auth" modules
   error_exit();
...
if ( ! pam-acct_mgmt(...) )    Checks for a valid, unexpired account and
                               verifies access restrictions with "account" modules
   error_exit();
...
pam_setcred(...)               Sets extra credentials, e.g. a Kerberos ticket
...
pam_open_session(...);         Sets up the session with "session" modules

do_stuff();

pam_close_session(...);        Tear-down session using the "session" modules
pam_end(...);
```
