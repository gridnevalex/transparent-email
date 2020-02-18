[![Build Status](https://travis-ci.org/bkrukowski/transparent-email.svg?branch=master)](https://travis-ci.org/bkrukowski/transparent-email)
[![Coverage Status](https://coveralls.io/repos/github/bkrukowski/transparent-email/badge.svg?branch=master)](https://coveralls.io/github/bkrukowski/transparent-email?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/7f5196c71a7349a4b04228bbafb72c13)](https://www.codacy.com/app/bartlomiej-krukowski/transparent-email?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=bkrukowski/transparent-email&amp;utm_campaign=Badge_Grade)

# Transparent Email

Transparent Email clears aliases from email address. Email `John.Doe+alias@gmail.com` will be transformed to `johndoe@gmail.com`.

## Why?

To detect multi-accounts on your website.

## Supported mailboxes

* [gmail.com](https://gmail.com) and [Google Apps](https://apps.google.com) with custom domain
* [tlen.pl](http://tlen.pl)
* [33mail.com](https://www.33mail.com)
* [outlook.com](http://outlook.com)
* [yahoo.com](http://mail.yahoo.com)
* [icloud.com](https://www.icloud.com/)
* [yandex.ru](https://yandex.ru/)
* [mail.ru](https://mail.ru/)

## Usage

```php
use bkrukowski\TransparentEmail\TransparentEmailFactory;
use bkrukowski\TransparentEmail\Emails\Email;
use bkrukowski\TransparentEmail\Emails\EmailInterface;
use bkrukowski\TransparentEmail\Emails\InvalidEmailException;

try {
    $factory = new TransparentEmailFactory();
    $cleaner = $factory->createDefault();
    $inputEmail = new Email('John.Doe+alias@gmail.com');
    /** @var EmailInterface $transformedEmail */
    $transformedEmail = $cleaner->getPrimaryEmail($inputEmail);
    echo $transformedEmail;
} catch (InvalidEmailException $exception) {
    echo 'Invalid email!';
}
```

## Versioning

The version numbers follow the [Semantic Versioning 2.0.0](http://semver.org/) scheme.

## Yahoo.com

Aliases work different on Yahoo than on Gmail. On Gmail part after plus is skipped.
For example message sent to `janedoe+alias@gmail.com` will be redirected to `janedoe@gmail.com`.

Yahoo uses the following pattern[*](https://help.yahoo.com/kb/SLN16026.html):

*baseName*-*keyword*@yahoo.com

* *baseName* - value defined by the user, different than email login;
* *keyword* - one from a list of keywords defined by the user.

Therefore we do not know what is the real email, so in this case result will be `baseName@yahoo.com`,
which actually does not exist.

## Yandex.ru
In official [documentation](https://yandex.com/support/mail/web/preferences/about-sender/additional-addresses.html) you can find some use-cases about aliases usage.

## Mail.ru
Mail.ru service uses following aliases:
* `janedoe+alias@mail.ru` will be redirected to `janedoe@mail.ru`
* `janeDoe@MAIL.RU` will be redirected to `janedoe@mail.ru`

## Icloud.com
Icloud.com service uses following aliases:
* `janedoe+alias@icloud.com` will be redirected to `janedoe@icloud.com`
* `janeDoe@ICLOUD.COM` will be redirected to `janedoe@icloud.com`