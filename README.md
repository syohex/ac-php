# ac-php   [![MELPA](http://melpa.org/packages/ac-php-badge.svg)](http://melpa.org/#/ac-php)     [![MELPA Stable](http://stable.melpa.org/packages/ac-php-badge.svg)](http://stable.melpa.org/#/ac-php)

emacs auto-complete for php

use [phpctags](https://github.com/xcwen/phpctags) gen tags 

and use `ac-php`  work with tags 


* support system function 

![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/7.png)
------
![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/8.png)

* support system class

core: SPL SplFileInfo DOMDocument  SimpleXMLElement   ... 

externed: PDO Http mysqli Imagick  SQLite3 Memcache  ...

externed:  Redis ,Swoole 

![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/6.png)

* support user self class system


![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/4.png)


* example:
![example.gif](https://raw.githubusercontent.com/xcwen/ac-php/master/images/ac-php.gif)


 
## Table of Contents

* [Install](#install)
* [Test](#test)
* [Usage](#usage)
* [php extern for complete](#php-doc-for-complete)
* [tags](#tags)
* [lager php project config](#lager-php-project-config)
* [FQA](#fqa)


##  Install 
### UBUNTU
* install `php5-cli` command  for phpctags
```bash 
localhost:~/$ sudo apt-get install php5-cli 
```

* install `cscope` command  for `ac-php-cscope-find-egrep-pattern`
```bash 
localhost:~/$ sudo apt-get install cscope
```

### MAC OSX 
```bash
 brew  install homebrew/php/php56
```
```bash
 brew  install cscope 
```
### check `php`,`cscope`  existed 
```bash
localhost:~$ php --version
PHP 5.5.20 (cli) (built: Feb 25 2015 23:30:53) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
localhost:~$ cscope --version
cscope: version 15.8a
```


##  Test


```bash
#backup old .emacs
cp ~/.emacs ~/.emacs.bak
```

save it as `~/.emacs`
```elisp
  (setq package-archives
        '(("melpa" . "http://melpa.milkbox.net/packages/")) )
  (package-initialize)
  (unless (package-installed-p 'ac-php )
    (package-refresh-contents)
    (package-install 'ac-php )
    )
  (require 'cl)
  (require 'php-mode)
  (add-hook 'php-mode-hook
            '(lambda ()
               (auto-complete-mode t)
               (require 'ac-php)
               ;;(setq ac-php-use-cscope-flag  t ) ;;enable cscope
               (setq ac-sources  '(ac-source-php ) )
               (yas-global-mode 1)
               (define-key php-mode-map  (kbd "C-]") 'ac-php-find-symbol-at-point)   ;goto define
               (define-key php-mode-map  (kbd "C-t") 'ac-php-location-stack-back   ) ;go back
               ))
```

```bash
cd ~/
git clone https://github.com/xcwen/ac-php/
#test php files in ~/ac-php/phptest
#open file for test
emacs ~/ac-php/phptest/testb.php
```

# Usage

* install `ac-php` from melpa
```elisp
  (setq package-archives
        '(("melpa" . "http://melpa.milkbox.net/packages/")) )
```

"M-x" :`package-list-packages`  find  `ac-php` install

* emacs php-mode function  define

```elisp
  (add-hook 'php-mode-hook
            '(lambda ()
               (auto-complete-mode t)
               (require 'ac-php)
               (setq ac-sources  '(ac-source-php ) )
               ;;(setq ac-php-use-cscope-flag  t )  ;;enable cscope
               (yas-global-mode 1)
               (define-key php-mode-map  (kbd "C-]") 'ac-php-find-symbol-at-point)   ;goto define
               (define-key php-mode-map  (kbd "C-t") 'ac-php-location-stack-back   ) ;go back
               ))
```


* mkdir ".tags"  in root of project

``` bash
cd /project/to/path #  root dir of project
mkdir .tags
```
* DONE 

*  command 

`ac-php-remake-tags` ;; **if source is changed ,re run this commond for update tags**

`ac-php-remake-tags-with-lib` ;; **if you find a error, run it an retest**

`ac-php-remake-tags-all` ;; **if you find a error, run it an retest**


`ac-php-find-symbol-at-point`   ;goto define

`ac-php-location-stack-back`    ;go back

`ac-php-show-tip` ;; show define at point

`ac-php-cscope-find-egrep-pattern` ;; find current-word in project  


need: (setq ac-php-use-cscope-flag  t ) 





## Php Doc for complete  
define class memeber type :

`public  $v1;`  =>
``` php
/**
 * @var classtype
 */
public $v1;
```

if you won't define `public $v1 ` you can define with comment ,like this =>
```
/** @var  $v1 \Test\Testa  */
```
![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/5.png)


define class function   return type:

`public  function get_v1()`  =>
```php
/**
 * @return classtype 
 */
public function get_v1()

```

or define use php7 :

```php
public function get_v1() :classtype  {

}
```

![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/2.png)

![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/3.png)


define variable: (**if function or member no define reutrn value  you need define it  **)

`$value=ff();` => 
```php
/** @var $value  Testa */
$value=ff();
```

like this
```php
class Testa {
    /** @var  $v8 \Test\Testa  */

    /**
     * @var int; 
     */
	const CON=1;

    
    /**
     * @var Testb; 
     */
	public  $v1;
    /**
     * @var \Test\TestC; 
     */
	public  $v2;
    public function set_v1($v){

        //for complete system function 
        $v=trim($v);

        $c=new Testb();
        //can complete
        $c->get_v2();

        //for complete memeber 
        $this->v1=$v;


        //for complete function  return type  
        $this-> get_v2()->testC();

        //for complete field from comment
        $this->v8->testA();

        //for complete system class 
        $q=new SplQueue ();
        $q->push(11);

       //jump for class function point
        $f=array($this->v8, "test_A" );
        $f();

    }

    /**
     * 
     * @return  \Test\TestC; 
     */
    public function get_v2(){
        $this->v2;
    }
}
```
![](https://raw.githubusercontent.com/xcwen/ac-php/master/images/1.png)


## Tags
tags file location dir is in  `.tags`   for example:  `/project/to/path/.tags`
```bash
localhost:~/ac-php/phptest/.tags$ tree .
.
├── tags-data.el
└── tags_dir_jim
    ├── a.el
    ├── testa.el
    └── testb.el
1 directory, 4 files
```

### Configue PHP file Search 

config file name  is `.ac-php-conf.json`  it's at  `.tags` location dir ,

when run `ac-php-remake-tags`  will gen `.ac-php-conf.json` if it's not find ;

like this

```json
{
  "tags-save-to-home-dir" :true,
  "filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
      "."
    ],
    "php-path-list-without-subdir": []
  },
  "lib-filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
    ],
    "php-path-list-without-subdir": []
  }

}
```

`tags-save-to-home-dir` : default tags data save `.tags` , if set true, will save  to `~/.ac-php/` 
it's usefull for  edit remote php file  use sshfs mount.


`php-file-ext-list` : file extern name list;

`php-path-list`:  find php files *recursion*  ;

`php-path-list-without-subdir`:  find php files  *no recursion* no find php from subdir  ;

for exmaple:

```
├── dir1
│   ├── 1.php
│   └── dir11
│       └── 11.php
├── dir2
│   └── 2.php
└── dir3
    ├── 3.php
    ├── dir31
    │   └── 31.php
    ├── dir32
    │   └── 32.php
    └── dir33
        └── 33.php
```

```json
{
  "filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
      "./dir1",
      "./dir2",
      "./dir3/dir32/"
    ],
    "php-path-list-without-subdir": [
      "./dir3"
     ]
  },
  "lib-filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
    ],
    "php-path-list-without-subdir": [
     ]
  }

}
```
 
filter result is:
 
```
├── dir1
│   ├── 1.php
│   └── dir11
│       └── 11.php
├── dir2
│   └── 2.php
└── dir3
    ├── 3.php
    ├── dir32
    │   └── 32.php
```

`31.php` `33.php` will not gen tags;



### Rebuild Tags 
**if source is changed ,re run this commond for update tags**: `ac-php-remake-tags` 

if php file cannot pass from `phpctags`.

you can find any  error from `Messages` buffer  fix it and next

like this 
```
phpctags[/home/jim/phptest/testa.php] ERROR:PHPParser: Unexpected token '}' on line 11 - 
```
you need fix testa.php  error and re run `ac-php-remake-tags`


if show:
```
no find .tags dir in path list :/home/jim/phptest/ 
```

you need *mkdir ".tags" in root of project*

like this:

`mkdir /home/jim/phptest/.tags`

or

`mkdir /home/jim/.tags `


###lager php project config
config php lib dir  into  `lib-filter` node 

```json
{
  "filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
      "./"
    ],
    "php-path-list-without-subdir": [
     ]
  },
  "lib-filter": {
    "php-file-ext-list": [
      "php",
      "inc"
    ],
    "php-path-list": [
       "./lib",
       "./vendor"
    ],
    "php-path-list-without-subdir": [
     ]
  }

}
```

if lib php file changed .

use   `ac-php-remake-tags-with-lib`   to remake tags 

## FQA 
#  for all any question　

you find a question 

exec : `M-x`: `ac-php-remake-tags-all`

and retest

# php files is in remote server. 

you can set  `tags-save-to-home-dir :true`  in  `.ac-php-conf.json ` 
