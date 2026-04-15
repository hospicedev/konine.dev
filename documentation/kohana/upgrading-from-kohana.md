---
layout: documentation
title: Kohana
---
# Upgrading from Kohana / Koseven

## Migrating to Konine

Konine is the successor to Koseven, which was archived on 14 April 2026. If you are running Koseven or Kohana and need to continue receiving PHP compatibility and security fixes, migrating to Konine is straightforward — the codebase is a direct continuation of Koseven 3.3.x.

### Adding Konine as a remote

Rather than cloning a fresh copy, you can pull Konine's changes directly into your existing project:

```bash
# Add the Konine repository as a new remote
git remote add konine https://github.com/hospicedev/konine.git

# Fetch all Konine branches and tags
git fetch konine

# Merge the Konine master branch into your current branch
git merge konine/master --allow-unrelated-histories
```

If you have local customisations to framework files (anything in `system/` or the bundled `modules/`), resolve any merge conflicts in favour of your own code unless the conflict is in a PHP compatibility fix.

### Keeping up to date

Once the remote is added, pulling future patches is a single command:

```bash
git fetch konine && git merge konine/master
```

---

## Breaking Changes in Konine 3.4.x (PHP 8.4 compatibility)

The following changes were made as part of the `feature/php84-compat` work. Review each one before completing your migration.

### PHP version requirement raised to 8.4

**Minimum PHP version is now 8.4.** The `composer.json` constraint has been updated to `>=8.4`. Verify your hosting environment before upgrading.

### Mcrypt engine removed from the Encrypt module

The `Encrypt_Engine_Mcrypt` driver and its associated `Kohana_Encrypt_Engine_Mcrypt` class have been **deleted**. If you are using `new Encrypt` with a `mcrypt` type, your application will throw a fatal error.

**Action required:** Switch to the `openssl` engine. Update your `application/config/encrypt.php`:

```php
return [
    'default' => [
        'type'   => 'openssl',
        'key'    => 'your-key-here',
        'cipher' => 'AES-256-CBC',
    ],
];
```

Any data previously encrypted with Mcrypt will need to be re-encrypted with OpenSSL.

### ext-mbstring is now a required dependency

`ext-mbstring` has been added as a hard requirement in `composer.json`. The `UTF8` class now delegates `strtolower` and `str_pad` to native `mb_*` functions instead of maintaining its own codepoint lookup tables. If `mbstring` is not enabled in your PHP installation, Composer will refuse to install.

### Image::rotate() — fourth parameter removed

The deprecated fourth argument to PHP's `imagerotate()` has been removed from `Image_GD::_do_rotate()`. If you have subclassed `Image_GD` and overridden `_do_rotate()` with the four-argument form, remove the final argument.

### `++` on non-integer values now throws TypeError (PHP 8)

PHP 8 raises a `TypeError` when you increment a non-numeric value with `++`. The `HTTP_Cache` hit-counter code in `modules/cache/classes/Kohana/HTTP/Cache.php` has been updated to cast the cached value to `(int)` before incrementing. If your application does something similar with a cache driver that can return `null` on a miss, apply the same cast.

### ORM `behaviors()` fix — no longer returns empty array

`Kohana_ORM::behaviors()` previously hard-coded `return []`, silently discarding any `$_behaviors` you defined. It now returns `$this->_behaviors`. If you were working around this by defining behaviors elsewhere, review your ORM models to avoid double-registration.

### Request\Client\Stream — failed `fopen()` now throws

`Request_Client_Stream` previously silenced a failed `fopen()` and passed a non-resource to further processing. It now throws a `Request_Exception` immediately. If your code caught a downstream error from a bad stream URL, it will now receive the exception earlier.

### Request\Client\Curl — non-integer option keys now throw

`Request_Client_Curl` now validates that all entries in the CURL options array have integer keys before calling `curl_setopt_array()`. Passing a string key (e.g. `'CURLOPT_TIMEOUT'` instead of `CURLOPT_TIMEOUT`) will throw a `Request_Exception` rather than silently failing.

### `Kohana::version()` now returns "Konine x.y.z"

`Kohana::version()` returns `'Konine 3.3.9 (karlsruhe)'` instead of `'Koseven ...'`. If you display or check the version string anywhere, update those checks accordingly.

---

## Breaking Changes from Kohana 3.3.x (Koseven era)

These changes were already present in Koseven and carry forward into Konine.

- **`Kohana_Kohana_Exception`** — all functions that received `Exception $e` have been replaced with just `$e`. If you are extending this class, update the signature.
- **`Kohana_URL::site()`** — has a new parameter `$subdomain = NULL`. If you extend this method, add it.
- **Encrypt module** — encryption is now a separate module. Enable it in your bootstrap: `'encrypt' => MODPATH.'encrypt'`
- **MySQL driver removed** — use MySQLi. Update `config/database.php` to `'type' => 'MySQLi'`

## New modules included

- Encrypt (separated from system)
- Pagination
