# MageHelper : How to use Slick Slider in Magento 2

We will learn here, how to use **Slick Slider** in Magento 2 step by step.

We can create new module in `app/code/` directory..

### Step - 1 - Create a directory for the module

- In Magento 2, module name divided into two parts i.e Vendor_Module (for e.g Magento_Theme, Magento_Catalog)
- We will create `MageHelper_SlickSlider` here, So `MageHelper` is vendor name and `SlickSlider` is name of this module.
- So first create your namespace directory (`MageHelper`) and move into that directory.
- Then create module name directory (`SlickSlider`)

Now Go to : `app/code/MageHelper/SlickSlider`

### Step - 2 - Create module.xml file to declare new module.

- Magento 2 looks for configuration information for each module in that module’s etc directory. so we need to add module.xml file here in our module `app/code/MageHelper/SlickSlider/etc/module.xml` and it's content for our module is :

~~~ xml
<?xml version="1.0"?>
<!--
/**
 * MageHelper Use Slick slider in Magento 2
 *
 * @package      MageHelper_SlickSlider
 * @author       Kishan Savaliya <kishansavaliyakb@gmail.com>
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
	<module name="MageHelper_SlickSlider" setup_version="1.0.0" />
</config>
~~~

In this file, we register a module with name `MageHelper_SlickSlider` and the version is `1.0.0`.

### Step - 3 - create registration.php

- All Magento 2 module must be registered in the Magento system through the magento `ComponentRegistrar` class. This file will be placed in module's root directory.

In this step, we need to create this file:

~~~
app/code/MageHelper/SlickSlider/registration.php
~~~

And it’s content for our module is:

~~~ php
<?php
/**
 * MageHelper Use Slick slider in Magento 2
 *
 * @package      MageHelper_SlickSlider
 * @author       Kishan Savaliya <kishansavaliyakb@gmail.com>
 */
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'MageHelper_SlickSlider',
    __DIR__
);
~~~

### Step - 4 - Enable `MageHelper_SlickSlider` module.

- By finish above step, you have created an empty module. Now we will enable it in Magento environment.
- Before enable the module, we must check to make sure Magento has recognize our module or not by enter the following at the command line:

~~~ 
php bin/magento module:status
~~~

If you follow above step, you will see this in the result:

~~~
List of disabled modules:
MageHelper_SlickSlider
~~~

This means the module has recognized by the system but it is still disabled. Run this command to enable it:

~~~
php bin/magento module:enable MageHelper_SlickSlider
~~~

The module has enabled successfully if you saw this result:

~~~
The following modules has been enabled:
- MageHelper_SlickSlider
~~~

This’s the first time you enable this module so Magento require to check and upgrade module database. We need to run this command:

~~~
php bin/magento setup:upgrade
~~~

### Step - 5 - Create `requirejs-config.js` file

Create `requirejs-config.js` this file here in module..

> app/code/MageHelper/SlickSlider/view/frontend/requirejs-config.js

Content for this file is ..

~~~
var config = {
    paths: {
        slick: 'MageHelper_SlickSlider/js/slick'
    },
    shim: {
        slick: {
            deps: ['jquery']
        }
    }
};
~~~

### Step - 6 - Add `slick.js` and `slick.min.js` files

You can download latest version of [slick here](http://kenwheeler.github.io/slick/)

Download that and extract .zip file and copy `slick.js` and `slick.min.js` files from there and paste that here..

> app/code/MageHelper/SlickSlider/view/frontend/web/js/slick.js

and

> app/code/MageHelper/SlickSlider/view/frontend/web/js/slick.min.js

### Step - 7 - Now add `slick.less` and `slick-theme.less` files

Copy `.less` files from downloaded directory to below mentioned path..

> app/code/MageHelper/SlickSlider/view/frontend/web/css/source/slick.less

and 

> app/code/MageHelper/SlickSlider/view/frontend/web/css/source/slick-theme.less

### Step - 8 - create `_module.less` file

Now create `_module.less` file here..

> `app/code/MageHelper/SlickSlider/view/frontend/web/css/source/_module.less`

This is important file to run slick slider properly in Magento 2 without any issues.

Content for this file is..

~~~
@import 'slick.less';
@import 'slick-theme.less';
~~~

- We will display all sliders in one custom action in this module.

### Step - 9 - Create frontend router

- Create `routes.xml` file here..

> app/code/MageHelper/SlickSlider/etc/frontend/routes.xml

Content for this file is ..

~~~
<?xml version="1.0"?>
<!--
/**
 * MageHelper Use Slick slider in Magento 2
 *
 * @package      MageHelper_SlickSlider
 * @author       Kishan Savaliya <kishansavaliyakb@gmail.com>
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="standard">
        <route id="magehelper_slickslider" frontName="magehelper_slickslider">
            <module name="MageHelper_SlickSlider" />
        </route>
    </router>
</config>
~~~

- So we can run this URL to check our output.. (http://www.example.com/magehelper_slickslider)

### Step - 10 - Create Controllers and Actions

- Now we will create our controller and action to show all slick slider demo. We will create controller and action here

~~~
app/code/MageHelper/SlickSlider/Controller/Index/Index.php 
~~~

And content for this file is :

~~~
<?php
/**
 * MageHelper Use Slick slider in Magento 2
 *
 * @package      MageHelper_SlickSlider
 * @author       Kishan Savaliya <kishansavaliyakb@gmail.com>
 */

namespace MageHelper\SlickSlider\Controller\Index;

class Index extends \Magento\Framework\App\Action\Action
{
    protected $resultPageFactory;

    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Framework\View\Result\PageFactory $resultPageFactory
    ){
        $this->resultPageFactory = $resultPageFactory;
        return parent::__construct($context);
    }
     
    public function execute()
    {
        $resultPage = $this->resultPageFactory->create();
        $resultPage->getConfig()->getTitle()->prepend(__('MageHelper Slick slider demo'));
 
        return $resultPage;
    }
}
~~~

### Step - 11 - Create layout and template files

- Create layout file here..

> app/code/MageHelper/SlickSlider/view/frontend/layout/magehelper_slickslider_index_index.xml

Content for this file is ...

~~~
<?xml version="1.0"?>
<!--
/**
 * MageHelper Use Slick slider in Magento 2
 *
 * @package      MageHelper_SlickSlider
 * @author       Kishan Savaliya <kishansavaliyakb@gmail.com>
 */
-->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" layout="1column" xsi:noNamespaceSchemaLocation="../../../../../../../lib/internal/Magento/Framework/View/Layout/etc/page_configuration.xsd">       
    <referenceContainer name="content">
        <block
            template="MageHelper_SlickSlider::slick-slider.phtml"
            class="Magento\Framework\View\Element\Template"
            name="magento-2-slick-slider-demo"/>
    </referenceContainer>
</page>
~~~

- Create template file here ...

> app/code/MageHelper/SlickSlider/view/frontend/templates/slick-slider.phtml

Add Content in phtml file..

### Step - 12 - Run following commands

- We need to run following commands to check output properly

~~~
php bin/magento setup:upgrade
php bin/magento cache:clean
php bin/magento cache:flush
php bin/magento setup:static-content:deploy -f
~~~

Hope this article will helpful to you!!