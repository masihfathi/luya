<p align="center">
  <img src="https://raw.githubusercontent.com/luyadev/luya/master/docs/internals/images/luya_logo_rc4.png" alt="LUYA Logo"/>
</p>

# Frontend User Group Module

[![LUYA](https://img.shields.io/badge/Powered%20by-LUYA-brightgreen.svg)](https://luya.io)
[![Latest Stable Version](https://poser.pugx.org/luyadev/luya-module-frontendgroup/v/stable)](https://packagist.org/packages/luyadev/luya-module-frontendgroup)
[![Total Downloads](https://poser.pugx.org/luyadev/luya-module-frontendgroup/downloads)](https://packagist.org/packages/luyadev/luya-module-frontendgroup)
[![Slack Support](https://img.shields.io/badge/Slack-luyadev-yellowgreen.svg)](https://slack.luya.io/)

The main purpose of this module to provide the ability to allow cms pages for specific user groups. This can also be only one group with one users or different users in the same group or different groups with different users.

## Installation

For the installation of modules Composer is required

```sh
composer require luyadev/luya-module-frontendgroup:1.0.0-RC4
```

### Configuration
After adding to your composer json you have to include the frontendgroup module into your Yii/LUYA config of your project and bootstrap the Module (otherwhise it can not catch the menu before item event).

```php
'modules' => [
    // ...
    'frontendgroup' => [
        'class' => 'luya\frontendgroup\Module',
        'frontendUsers' => [
            'user1', 'user2', 'user3',
        ],
        'frontendGroups' => [
            'groupA', 'groupB',
        ],
    ],
],

'bootstrap' => [
    // ...
    'frontendgroup',
],

'components' => [

    // ...

    'user1' => [
        'class' => 'luya\web\GroupUser',
        'identityClass' => 'app\models\User1Class',
    ],
    'user2' => [
        'class' => 'luya\web\GroupUser',
        'identityClass' => 'app\models\User2Class',
    ],
    'user3' => [
        'class' => 'luya\web\GroupUser',
        'identityClass' => 'app\models\User3Class',
    ],
]
```

The config above shows defines your configuration:

+ In the module `frontendgroup` you have to define the different users which are allowed in your setup by `frontendUsers`. And you have to defined the available groups by using `frontendGroups`.
+ The mentioned `frontendUsers` in the module must exists als component with the base class `luya\web\GroupUser` (this is a wrapper of yii\web\User).

The frontend users must follow the `GroupUserIdentityInterface`:

```php
// GroupUserIdentityInterface implentation

class User1 extends \yii\db\ActiveRecord implements GroupUserIdentityInterface
{
    // ...
    
    public function authGroups()
    {
        return ['groupA', 'groupB'];
    }
    
    // ...
}
```

The the above user class of `User1` is now allowed to access all pages which are defined for `groupA` and `groupB`.

### Initialization 

After successfully installation and configuration run the migrate, import and setup command to initialize the module in your project.

1.) Migrate your database.

```sh
./vendor/bin/luya migrate
```

2.) Import the module and migrations into your LUYA project.

```sh
./vendor/bin/luya import
```
