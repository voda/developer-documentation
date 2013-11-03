<small>Piwik</small>

Mail
====

Class for sending mails, for more information see: [http://framework.zend.com/manual/en/zend.mail.html](#http://framework.zend.com/manual/en/zend.mail.html)


Methods
-------

The class defines the following methods:

- [`__construct()`](#__construct) &mdash; Constructor.
- [`setFrom()`](#setFrom) &mdash; Sets the sender.

### `__construct()` <a name="__construct"></a>

Constructor.

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$charset`
- It does not return anything.

### `setFrom()` <a name="setFrom"></a>

Sets the sender.

#### Signature

- It is a **public** method.
- It accepts the following parameter(s):
    - `$email`
    - `$name`
- It returns a(n) `Zend_Mail` value.
