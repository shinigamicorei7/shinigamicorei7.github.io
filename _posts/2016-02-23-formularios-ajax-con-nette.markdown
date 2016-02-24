---
layout: post
comments: true
title: "Formularios Ajax con Nette-php"
date: "2016-02-23 19:20:50 -0500"
categories:
- php
- nette
---
En este ejercicio vamos a crear un password recovery utilizando el proyecto inicial que proporciona Nette.

Para esto creamos nuestro proyecto usando composer:

{% highlight bash %}
composer create-project nette/sandbox nette-project
{% endhighlight %}

En el archivo `app/config.local.neon` especificamos los parametros para la conexion a la base de datos y a nuestro smtp server:

{% highlight coffeescript %}
database:
    dsn: 'mysql:host=localhost;dbname=nette'
    user: root
    password: tupassword
    options:
        lazy: yes

mail:
    smtp: true
    host: smtp.tuhost.com
    port: 446
    username: 'tuemail@tudominio.com'
    password: tupassword
{% endhighlight %}

Como servidor de correo puedes usar tu cuenta de gmail o puedes crear una cuenta en [Mailgun][mailgun].

Ahora en la clase `App\Forms\SignFormFactory` vamos a inyectar nuestros servicios y vamos a agregar el formulario para nuestro password recovery:

{% highlight php %}
<?php

namespace App\Forms;

use \Nette\Application\UI\Form;
use \Nette\Security\User;
use \Nette\Database\Context;
use \Nette\Mail\IMailer;
use Nette\Bridges\ApplicationLatte\ILatteFactory;

class SignFormFactory extends \Nette\Object
{

    /** @var User */
    private $user;

    /** @var Context */
    private $database;

    /** @var IMailer */
    private $mailer;

    /** @var \Latte\Engine */
    private $latte;

    public function __construct(User $user, Context $database, IMailer $mailer, ILatteFactory $late_factory)
    {
        $this->user = $user;
        $this->database = $database;
        $this->mailer = $mailer;
        $this->latte = $late_factory->create();
    }

    //..lo que sigue
}
{% endhighlight %}

Creamos una función que nos proveera el formulario en cuestion

{% highlight php %}
<?php
//... el código anterior
public function createRecovery()
{
    $form = new Form;
    $form->addText('email', 'Email:')->addRule(Form::EMAIL)->isRequired();
    $form->addSubmit('send', 'Enviar');

    $form->onSuccess[] = array($this, 'recoverySucceeded');
    return $form;
}

public function recoverySucceeded(Form $form, $values)
{
    $row = $this->database->table('users')->where('email', $values->email)->fetch();
    if (!$row)
    {
        $form->addError('No existe una cuenta con este correo.');
        $form->getPresenter()->redrawControl('recoveryForm');
    }
    else
    {
        $mail = new \Nette\Mail\Message();
        $mail->setFrom('recovery@pasword.com')
             ->setSubject('Password Recovery')
             ->addTo($row->email, $row->username)
             ->setHtmlBody($this->getEmailBody($row));

        try
        {
            $this->mailer->send($mail);
        }
        catch (\Nette\Mail\SendException $e)
        {
            $form->addError($e->getMessage());
            $form->getPresenter()->redrawControl('recoveryForm');
        }

        $form->getPresenter()->flashMessage('<b>Listo!</b> Revisa tu bandeja de entrada.');
        $form->getPresenter()->redrawControl('recoveryForm');
    }
}

public function getEmailBody($row)
{
    $templates_dir = realpath(__DIR__ . '/../presenters/templates');
    $templates_dir = rtrim($templates_dir, '\/');
    $recovery_template = $templates_dir . '/emails/recovery.latte';
    return $this->latte->renderToString($recovery_template, $row);
}
//...fin
{% endhighlight %}

Para el email creamos nuestra template `app\presenters\templates\emails\recovery.latte` en este caso usamos las muestras gratis de mailgun :smile:

{% highlight html %}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" style="font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;">
<head>
<meta name="viewport" content="width=device-width" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>Actionable emails e.g. reset password</title>


<style type="text/css">
img {
max-width: 100%;
}
body {
-webkit-font-smoothing: antialiased; -webkit-text-size-adjust: none; width: 100% !important; height: 100%; line-height: 1.6em;
}
body {
background-color: #f6f6f6;
}
@media only screen and (max-width: 640px) {
  body {
    padding: 0 !important;
  }
  h1 {
    font-weight: 800 !important; margin: 20px 0 5px !important;
  }
  h2 {
    font-weight: 800 !important; margin: 20px 0 5px !important;
  }
  h3 {
    font-weight: 800 !important; margin: 20px 0 5px !important;
  }
  h4 {
    font-weight: 800 !important; margin: 20px 0 5px !important;
  }
  h1 {
    font-size: 22px !important;
  }
  h2 {
    font-size: 18px !important;
  }
  h3 {
    font-size: 16px !important;
  }
  .container {
    padding: 0 !important; width: 100% !important;
  }
  .content {
    padding: 0 !important;
  }
  .content-wrap {
    padding: 10px !important;
  }
  .invoice {
    width: 100% !important;
  }
}
</style>
</head>

<body itemscope itemtype="http://schema.org/EmailMessage" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; -webkit-font-smoothing: antialiased; -webkit-text-size-adjust: none; width: 100% !important; height: 100%; line-height: 1.6em; background-color: #f6f6f6; margin: 0;" bgcolor="#f6f6f6">

<table class="body-wrap" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; width: 100%; background-color: #f6f6f6; margin: 0;" bgcolor="#f6f6f6"><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0;" valign="top"></td>
		<td class="container" width="600" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; display: block !important; max-width: 600px !important; clear: both !important; margin: 0 auto;" valign="top">
			<div class="content" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; max-width: 600px; display: block; margin: 0 auto; padding: 20px;">
				<table class="main" width="100%" cellpadding="0" cellspacing="0" itemprop="action" itemscope itemtype="http://schema.org/ConfirmAction" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; border-radius: 3px; background-color: #fff; margin: 0; border: 1px solid #e9e9e9;" bgcolor="#fff"><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="content-wrap" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0; padding: 20px;" valign="top">
							<meta itemprop="name" content="Confirm Email" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;" /><table width="100%" cellpadding="0" cellspacing="0" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="content-block" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0; padding: 0 0 20px;" valign="top">
										Please confirm your email address by clicking the link below.
									</td>
								</tr><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="content-block" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0; padding: 0 0 20px;" valign="top">
										We may need to send you critical information about our service and it is important that we have an accurate email address.
									</td>
								</tr><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="content-block" itemprop="handler" itemscope itemtype="http://schema.org/HttpActionHandler" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0; padding: 0 0 20px;" valign="top">
										<a href="http://www.mailgun.com" class="btn-primary" itemprop="url" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; color: #FFF; text-decoration: none; line-height: 2em; font-weight: bold; text-align: center; cursor: pointer; display: inline-block; border-radius: 5px; text-transform: capitalize; background-color: #348eda; margin: 0; border-color: #348eda; border-style: solid; border-width: 10px 20px;">Confirm email address</a>
									</td>
								</tr><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="content-block" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0; padding: 0 0 20px;" valign="top">
										&mdash; The Mailgunners
									</td>
								</tr></table></td>
					</tr></table><div class="footer" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; width: 100%; clear: both; color: #999; margin: 0; padding: 20px;">
					<table width="100%" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><tr style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; margin: 0;"><td class="aligncenter content-block" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 12px; vertical-align: top; color: #999; text-align: center; margin: 0; padding: 0 0 20px;" align="center" valign="top">Follow <a href="http://twitter.com/mail_gun" style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 12px; color: #999; text-decoration: underline; margin: 0;">@Mail_Gun</a> on Twitter.</td>
						</tr></table></div></div>
		</td>
		<td style="font-family: 'Helvetica Neue',Helvetica,Arial,sans-serif; box-sizing: border-box; font-size: 14px; vertical-align: top; margin: 0;" valign="top"></td>
	</tr></table></body>
</html>
{% endhighlight %}

Ahora en nuestro presenter `App\Presenters\SignPresenter` vamos a crear nuestro componente:

{% highlight php %}
<?php
//...resto de código
protected function createComponentRecoveryForm()
{
    return $this->factory->createRecovery();
}
//...resto de código
{% endhighlight %}

Vamos a crear nuestra template `app\presenters\templates\Sign\recovery.latte`:

{% highlight html %}
{block content}
<h1 n:block=title>Recuperar contraseña</h1>
<div n:snippet=recoveryForm>
    <div n:foreach="$flashes as $flash" n:class="flash, $flash->type">{$flash->message|noescape}</div>
    {if count($flashes) == 0}
        {control recoveryForm}
    {else}
        <a n:href="Sign:in">Iniciar Sesión</a>
    {/if}
</div>
{/block}

{block scripts}
{include parent}
<script src="{$basePath}/js/nette.ajax.js"></script>
<script>
    $(function () {
        $.nette.init();
    });
</script>
{/block}
{% endhighlight %}

Para lo cual necesitamos descargar el addon [nette.ajax][nette.ajax] y lo guardamos en nuestro folder de javascripts `www\js`

Con esto ya tenemos listo nuestro password recovery. Si alguna parte del tema no te queda claro puedes revisar el repositorio de github [nette-project][project].

Para mayor información puedes visitar la web oficial: [Nette][nette]

[nette]: https://nette.org
[mailgun]: https://www.mailgun.com/
[nette.ajax]: https://addons.nette.org/vojtech-dobes/nette-ajax-js
[project]: https://github.com/shinigamicorei7/nette-project
