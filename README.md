Expose: an IDS for PHP
======================

Expose is an Intrusion Detection System for PHP loosely based on the PHPIDS project (and using its ruleset for detecting potential threats).

**NOTE:** This is a (ideally, temporary) fork of the original repository at [enygma/expose](https://github.com/enygma/expose) in order to allow for Expose to be able to use the latest versions of [TWIG](https://github.com/twigphp/Twig) (3.x at the time of this writing). No other changes will be made.


### Quick Install

1. Install Composer:

    ```
    curl -s https://getcomposer.org/installer | php
    ```

1. Require Expose as a dependency using Composer:

    ```
    php composer.phar require jmecahansen/expose
    ```

1. Install Expose:

    ```
    php composer.phar install
    ```

### Example Usage

```php
<?php
    // initialize the Composer autoloader
    require "vendor/autoload.php";

    // define some test data
    $data = [
        "POST" => [
            "test" => "foo",
            "bar" => [
                "baz" => "quux",
                "testing" => "<script>test</script>"
            ]
        ]
    ];

    // load the appropriate filters
    $filters = new \Expose\FilterCollection();
    $filters->load();

    // instantiate a PSR-3 compatible logger
    $logger = new \Expose\Log\Mongo();

    // instantiate the Manager class
    $manager = new \Expose\Manager($filters, $logger);

    // run the Manager class object on the test data
    $manager->run($data);

    // get the impact estimation for the provided data (should return 8 for this example)
    $impact = $manager->getImpact();

    // get all matching filter reports
    $reports = $manager->getReports();

    // export out the report in the given format ("text" is default)
    echo $manager->export();

```

### Documentation

Documentation for Expose can be found at [ReadTheDocs](https://expose.readthedocs.org/en/latest/) or through its GitHub [repository](https://github.com/enygma/expose) page.
