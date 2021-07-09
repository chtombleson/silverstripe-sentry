# Sentry.io integration for SilverStripe

[![Build Status](https://api.travis-ci.org/phptek/silverstripe-sentry.svg?branch=master)](https://travis-ci.org/phptek/silverstripe-sentry)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/phptek/silverstripe-sentry/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/phptek/silverstripe-sentry/?branch=master)
[![License](https://poser.pugx.org/phptek/sentry/license.svg)](https://github.com/phptek/silverstripe-sentry/blob/master/LICENSE.md)

[Sentry](https://sentry.io) is an error and exception aggregation service. It takes your application's errors, aggregates them alongside configurable context and stores them for later analysis and debugging.

Imagine this: You see errors and exceptions before your clients do. The error > report > debug > patch > deploy cycle is therefore the most efficient it can possibly be.

This module binds Sentry.io and hosted Sentry installations, to the Monlog error logger in SilverStripe. If you've used systems like
[RayGun](https://raygun.com), [Rollbar](https://rollbar.com), [AirBrake](https://airbrake.io/) and [BugSnag](https://www.bugsnag.com/) before, you'll know roughly what to expect.

## Requirements
### SilverStripe Framework v4

 * PHP >=7.0
 * SilverStripe ^4.0
 * `phptek/sentry` version 3.x

#### Setup:

    composer require phptek/sentry:^3

### SilverStripe Framework v3

 * PHP 5.4+, <=7.4
 * SilverStripe > v3.1, < 4
 * `phptek/sentry` version 1.x

#### Setup:

    composer require phptek/sentry:^1

Notes:

* Versions 2.x and 3.x should work with the same Silverstripe v4 setups. v3 simply uses a newer version of the Sentry PHP SDK, and has a leaner codebase.
* Version 3.x `SentryClientAdaptor` has been renamed to `SentryAdaptor` and `SentryLogWriter` was renamed to `SentryLogger`, so your existing configuration(s) may need to be updated accordingly.

Configure your application or site with the Sentry DSN:

### SilverStripe Framework v4

#### General Config ####

You can set your DSN as a first-class environment variable or via your project's `.env` file:

    SENTRY_DSN="http://deacdf9dfedb24ccdce1b90017b39dca:deacdf9dfedb24ccdce1b90017b39dca@sentry.mydomain.nz/44"

Or you can set it in YML config, where you gain a little more flexibility and control:

The following will get you errors reported in all environment modes: `dev`, `test` and `live`:

    ---
    Name: my-project-config-sentry
    After:
      - 'sentry-config'
    ---

    PhpTek\Sentry\Adaptor\SentryAdaptor:
      opts:
        # Example DSN only. Obviously you'll need to setup your own Sentry "Project"
        dsn: http://deacdf9dfedb24ccdce1b90017b39dca:deacdf9dfedb24ccdce1b90017b39dca@sentry.mydomain.nz/44

#### Conditional Config ####

The following will get you errors reported just in `test` and `live` but not `dev`:

    ---
    Name: my-project-config-sentry
    After:
      - 'sentry-config'
    ---

    ---
    Only:
      environment: test
    ---
    PhpTek\Sentry\Adaptor\SentryAdaptor:
      opts:
        # Example DSN only. Obviously you'll need to setup your own Sentry "Project"
        dsn: http://deacdf9dfedb24ccdce1b90017b39dca:deacdf9dfedb24ccdce1b90017b39dca@sentry.mydomain.nz/44
    ---
    Except:
      environment: test
    ---
    PhpTek\Sentry\Adaptor\SentryAdaptor:
      opts:
        # Example DSN only. Obviously you'll need to setup your own Sentry "Project"
        dsn: http://deacdf9dfedb24ccdce1b90017b39dca:deacdf9dfedb24ccdce1b90017b39dca@sentry.mydomain.nz/44
    ---
    Only:
      environment: dev
    ---
    PhpTek\Sentry\Adaptor\SentryAdaptor:
      opts:
        dsn: null
    ---

#### Proxies ####

Should your app require outgoing traffic to be passed through a proxy, the following config will work:

    # Proxy constants
      http_proxy:
        host: '`MY_OUTBOUND_PROXY`'
        port: '`MY_OUTBOUND_PROXY_PORT`'

Notes:

* In module version 3.x you can silence errors from `Injector` where "test" and "live" envs have `http_proxy` set, but "dev" environments don't. Just set `null` as the value. This applies to all YML config where some envs have a setting and others don't. For example:

```
...
    ---
    Only:
      environment: dev
    ---
    PhpTek\Sentry\Adaptor\SentryAdaptor:
      opts:
        dsn: null
        http_proxy: null
    ---
...
```

* As per the examples above, ensure your project's Sentry config is set to come *after* the module's own config, thus:

    After:
      - 'sentry-config'

### SilverStripe Framework v3

YML Config:

    phptek\Sentry\Adaptor\SentryClientAdaptor:
      opts:
        # Example DSN only. Obviously you'll need to setup your own Sentry "Project"
        dsn: http://deacdf9dfedb24ccdce1b90017b39dca:deacdf9dfedb24ccdce1b90017b39dca@sentry.mydomain.nz/44

mysite/_config.php:

    SS_Log::add_writer(\phptek\Sentry\SentryLogWriter::factory(), SS_Log::ERR, '<=');

## Usage

Sentry is normally setup once in your project's YML config or `_config.php` file. See the above examples and the [usage docs](docs/usage.md) for details and options.

## Support Me

If you like what you see, support me! I accept Bitcoin:

<table border="0">
	<tr>
		<td rowspan="2">
			<img src="https://bitcoin.org/img/icons/logo_ios.png" alt="Bitcoin" width="64" height="64" />
		</td>
	</tr>
	<tr>
		<td>
			<b>3KxmqFeVWoigjvXZoLGnoNzvEwnDq3dZ8Q</b>
		</td>
	</tr>
</table>

<p>&nbsp;</p>
<p>&nbsp;</p>
