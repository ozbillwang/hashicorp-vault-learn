# Getting Started

## start vault as service on mac, but where is the rook token?

After you installed vault on mac with command `brew install vault`, you are prompt to start vault server with two ways.

```
==> Pouring vault-1.6.1.catalina.bottle.tar.gz
==> Caveats
To have launchd start vault now and restart at login:
  brew services start vault
Or, if you don't want/need a background service you can just run:
  vault server -dev
==> Summary
```

If you start it with first way, you will be confused on where I can get the unseal key and root token.

#### Here is the answer:

```
$ brew services list
Name       Status  User Plist
...
vault      started bill /Users/bill/Library/LaunchAgents/homebrew.mxcl.vault.plist

$ cat /Users/bill/Library/LaunchAgents/homebrew.mxcl.vault.plist

...
    <key>WorkingDirectory</key>
    <string>/usr/local/var</string>
    <key>StandardErrorPath</key>
    <string>/usr/local/var/log/vault.log</string>
    <key>StandardOutPath</key>
    <string>/usr/local/var/log/vault.log</string>
```

So the vault log is at `/usr/local/var/log/vault.log` with unseal key and root token.

```
$ cat /usr/local/var/log/vault.log

...
2020-12-25T22:07:01.747+1100 [INFO]  secrets.kv.kv_516b0983: upgrading keys finished
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: XhctKSIXlx2zjd1arUGBucprBXxXmzoygjpGR0S62T4=
Root Token: s.Gd5HSuBB87nSeYMOsdwaMOCk

Development mode should NOT be used in production installations!
```

## how to list roles generated via dynamic secrets

I have set dynamic secrets (type is aws), and I can generate roles with it. 

```
$ vault read aws/creds/my-role

Key                Value
---                -----
lease_id           aws/creds/my-role/1M0p1H0USTLPto0fWyKFi6Hc
lease_duration     768h
lease_renewable    true
access_key         AKIA5JKGG7DERBxxxx
secret_key         d1QX7oCt7j6YpXxxxx
security_token     <nil>

```

I can repeat this command to generate more roles.

But what's the vault command to list the roles it was created? Because I need to list these roles and revoke one of them

## The second part of "github authentication" is confusing.

https://learn.hashicorp.com/tutorials/vault/getting-started-authentication?in=vault/getting-started#github-authentication

After read and follow the steps, I still don't know what to do with it.
