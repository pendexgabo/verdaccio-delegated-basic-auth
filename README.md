# verdaccio-delegated-auth

verdaccio authentication plugin that delegates authentication to another HTTP URL

## Rationale

We use Kerberos for authentication. Developers are accustomed to be able to use
the same password everywhere in the organisation to gain access to resources
that they need for their work. This should also apply to our private `npm`
repositories.

## How does it work?

Basically, authentication is delegated to *another* website where Kerberos
authentication is already set up. The verdaccio authentication plugin is given
the URL of this website. Whenever the user tries to authenticate with verdaccio
using a username-password combination, verdaccio makes an HTTP request in the
background to this URL with the credentials that the user supplied. When the
website returns an HTTP 200 response, the password is considered correct and
the user is authenticated in verdaccio. In all other cases, authentication fails.

## Installation

```sh
$ npm install verdaccio-delegated-auth
```

## Config

Add to your `config.yaml`:

```yaml
auth:
  delegated-auth:
    url: "https://some.other.site.com/auth"
```

## Security considerations

verdaccio sends an HTTP Basic authentication request to the delegate website. The
usual security considerations for HTTP Basic authentication apply here: don't
do it over an unencrypted channel unless you are *absolutely* sure that
unauthorised parties cannot intercept the communication and inspect the
password in the Authorization header. Use `https` if you can. 


## Acknolege

This is just a simple fork of https://github.com/lab360-ch/verdaccio-gitlab-ci