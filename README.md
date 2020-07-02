A PHP wrapper to be used with [Gitlab's API](https://github.com/gitlabhq/gitlabhq/tree/master/doc/api).
==============

[![Build Status](https://travis-ci.org/m4tthumphrey/php-gitlab-api.svg?branch=master)](https://travis-ci.org/m4tthumphrey/php-gitlab-api)
[![StyleCI](https://styleci.io/repos/6816335/shield?branch=master)](https://styleci.io/repos/6816335)
[![Total Downloads](https://poser.pugx.org/m4tthumphrey/php-gitlab-api/downloads?format=flat-square)](https://packagist.org/packages/m4tthumphrey/php-gitlab-api)
[![Latest Stable Version](https://poser.pugx.org/m4tthumphrey/php-gitlab-api/version?format=flat-square)](https://packagist.org/packages/m4tthumphrey/php-gitlab-api)
[![Latest Unstable Version](https://poser.pugx.org/m4tthumphrey/php-gitlab-api/v/unstable?format=flat-square)](//packagist.org/packages/m4tthumphrey/php-gitlab-api)

Based on [php-github-api](https://github.com/m4tthumphrey/php-github-api) and code from [KnpLabs](https://github.com/KnpLabs/php-github-api).

Installation
------------

Via [Composer](https://getcomposer.org). You will also need to install packages that "provide" [`psr/http-client-implementation`](https://packagist.org/providers/psr/http-client-implementation) and [`psr/http-factory-implementation`](https://packagist.org/providers/psr/http-factory-implementation).

### PHP 7.1+:

```bash
composer require m4tthumphrey/php-gitlab-api:^10.0 php-http/guzzle6-adapter:^2.0.1 http-interop/http-factory-guzzle:^1.0
```

### PHP 7.2+:

```bash
composer require m4tthumphrey/php-gitlab-api:^10.0 guzzlehttp/guzzle:^7.0.1 http-interop/http-factory-guzzle:^1.0
```

### Laravel 6+:

```bash
composer require graham-campbell/gitlab:^4.0 guzzlehttp/guzzle:^7.0.1 http-interop/http-factory-guzzle:^1.0
```

### Symfony 4.4:

```bash
composer require zeichen32/gitlabapibundle:^5.0 symfony/http-client:^4.4 nyholm/psr7:^1.3
```

### Symfony 5:

```bash
composer require zeichen32/gitlabapibundle:^5.0 symfony/http-client:^5.0 nyholm/psr7:^1.3
```

We are decoupled from any HTTP messaging client with help by [HTTPlug](http://httplug.io). You can visit [HTTPlug for library users](https://docs.php-http.org/en/latest/httplug/users.html) to get more information about installing HTTPlug related packages. [graham-campbell/gitlab](https://github.com/GrahamCampbell/Laravel-GitLab) is by [Graham Campbell](https://github.com/GrahamCampbell) and [zeichen32/gitlabapibundle](https://github.com/Zeichen32/GitLabApiBundle) is by [Jens Averkamp](https://github.com/Zeichen32).

General API Usage
-----------------

```php
$client = \Gitlab\Client::create('http://git.yourdomain.com')
    ->authenticate('your_gitlab_token_here', \Gitlab\Client::AUTH_URL_TOKEN)
;

// or for OAuth2 (see https://github.com/m4tthumphrey/php-gitlab-api/blob/master/lib/Gitlab/HttpClient/Plugin/Authentication.php#L47)
$client = \Gitlab\Client::create('http://gitlab.yourdomain.com')
    ->authenticate('your_gitlab_token_here', \Gitlab\Client::AUTH_OAUTH_TOKEN)
;

$project = $client->api('projects')->create('My Project', array(
  'description' => 'This is a project',
  'issues_enabled' => false
));

```

Example with Pager
------------------

to fetch all your closed issue with pagination ( on the gitlab api )

```php
$client = \Gitlab\Client::create('http://git.yourdomain.com')
    ->authenticate('your_gitlab_token_here', \Gitlab\Client::AUTH_URL_TOKEN)
;
$pager = new \Gitlab\ResultPager($client);
$issues = $pager->fetchAll($client->api('issues'),'all',[null, ['state' => 'closed']]);

```

Model Usage
-----------

You can also use the library in an object oriented manner:

```php
$client = \Gitlab\Client::create('http://git.yourdomain.com')
    ->authenticate('your_gitlab_token_here', \Gitlab\Client::AUTH_URL_TOKEN)
;

# Creating a new project
$project = \Gitlab\Model\Project::create($client, 'My Project', array(
  'description' => 'This is my project',
  'issues_enabled' => false
));

$project->addHook('http://mydomain.com/hook/push/1');

# Creating a new issue
$project = new \Gitlab\Model\Project(1, $client);
$issue = $project->createIssue('This does not work.', array(
  'description' => 'This doesn\'t work properly. Please fix.',
  'assignee_id' => 2
));

# Closing that issue
$issue->close();
```

You get the idea! Take a look around ([API methods](https://github.com/m4tthumphrey/php-gitlab-api/tree/master/lib/Gitlab/Api), [models](https://github.com/m4tthumphrey/php-gitlab-api/tree/master/lib/Gitlab/Model)) and please feel free to report any bugs. If you have integrated GitLab into a popular PHP framework, let us know!

Contributing
------------

There are many parts of Gitlab that I have not added to this as it was originally created for personal use, hence the lack of tests. Feel free to fork and add new functionality and tests, I'll gladly accept decent pull requests.
