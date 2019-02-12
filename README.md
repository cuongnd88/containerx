# ContainerX

ContainerX is a little PHP dependency injection container.

# Installation
```bash
$ composer require kanian/containerx
```
# Usage
Let:

The **Car** class be:
```php
class Car {
	protected $driver;
    public function __construct(Driver $driver)
    {
    	$this -> driver = $driver;
    }
    \\\ ... more car code
}
```
And the **HumanDriver** class be:
```php
class HumanDriver implements Driver {
  public function drive()
  {
  	\\\ ... some driving code
  }
}
```

We can use:
## Container functionalities as Object Methods
In order to access the functionalities of the container as object methods:
```php
use Kanian\ContainerX\Container;

$container = new Container();
$container->set('chauffeur', function($c){ return new HumanDriver;});
$container->set('limo', function($c){ return new Car($c->get('chauffeur'));};);

$limo = $container->get('limo');
```
We have used anonymous functions has factories.
Moreover, We could simply register the dependencies we need and let the container instantiate them:
```php
use Kanian\ContainerX\Container;
$container = new Container();
$container->set('chauffeur',HumanDriver::class);
$container->set('limo',Car::class);
$limo = $container->get('limo');
```
The container will know how to construct a Car instance for us.

Alternatively, we can use:
## Container Functionalities Through The Array Access Interface
For example, we can achieve factory based registration by using the **Kanian\Container\ContainerX** class, which implements the ArrayAccess interface.

```php
use Kanian\ContainerX\ContainerX;

$container = new ContainerX();
$container['chauffeur'] =  HumanDriver::class;
$container['limo'] = Car::class;

$limo = $container['limo'];
```