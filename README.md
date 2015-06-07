# Mailchimp v3 PHP

PHP service class for managining Mailchimp v3 API subscriptions.

## Example usage

```php
$conf = [
          'apiurl' => 'https://us11.api.mailchimp.com/3.0/',
          'apikey' => 'someApiKeyb497cd07a4-us11',
          'list' => '87421ce950',  // list id from https://us11.api.mailchimp.com/playground/
          'logfile' => 'mailchimp.log' 
];
$user = (object)['id' => 53]; // for loging purposes only

$mailchimp = new Services\Mailchimp($conf, $user);

$mail = 'testmailforpatch@pppp.cz';
$mailchimp->getStatus($mail) // == FALSE;
$mailchimp->subscribe($mail, 'jm', 'př') // nothing returned
$mailchimp->getStatus($mail) // == 'subscribed';
$mailchimp->delete($mail) // nothing returned
$mailchimp->getStatus($mail) // == FALSE;
```

Request played for the example above:
```
2015-06-05 18:15:08 (uid=53) GET /lists/87421ce950/members/50ed17fa41ba3906dade48038810de77 >>> 404 {"type":"http://kb.mailchimp.com/api/error-docs/404-resource-not-found",...}

2015-06-05 18:15:08 (uid=53) GET /lists/87421ce950/members/50ed17fa41ba3906dade48038810de77 >>> 404 {"type":"http://kb.mailchimp.com/api/error-docs/404-resource-not-found",...}
2015-06-05 18:15:09 (uid=53) POST /lists/87421ce950/members{"email_address":"testmailforpatch@pppp.cz","merge_fields":{"FNAME":"jm","LNAME":"p\u0159"},"status":"subscribed"} >>> 200 {"id":"50ed17fa41ba3906dade48038810de77","email_address":"testmailforpatch@pppp.cz","status":"subscribed"...}

2015-06-05 18:15:09 (uid=53) GET /lists/87421ce950/members/50ed17fa41ba3906dade48038810de77 >>> 200 {"id":"50ed17fa41ba3906dade48038810de77","email_address":"testmailforpatch@pppp.cz","status":"subscribed"...}

2015-06-05 18:15:09 (uid=53) DELETE /lists/87421ce950/members/50ed17fa41ba3906dade48038810de77 >>> 204 

2015-06-05 18:15:09 (uid=53) GET /lists/87421ce950/members/50ed17fa41ba3906dade48038810de77 >>> 404 {"type":"http://kb.mailchimp.com/api/error-docs/404-resource-not-found","title":"Resource Not Found","status":404,"detail":"The requested resource could not be found.","instance":"bad7b267-b946-4024-a5bb-127b7ffe5178"}
```



## Nette framework integration
```yaml
common:
    parameters:
        mailchimp:
          apiurl: https://us11.api.mailchimp.com/3.0/
          apikey: someApiKeyb497cd07a4-us11
          list: 87421ce950
          logfile: %logDir%/mailchimp.log          
    services:
        mailchimp: Services\MailchimpService(%mailchimp%, @user)
```

Usage in presenter:
```php
$this->context->mailchimp->subscribe($mail, 'jm', 'pø');
```



## Author and licence

(c) 2015 [Pavel Zbytovský](http://zby.cz)

Licenced under MIT license.
