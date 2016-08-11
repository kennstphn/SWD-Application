# SWD-Application
Design Spec for building independent and recursively integrated applications. 

##Scope

This project is to define a standard for the way SWD\Applications will be built. There should be an abstract class called Application at namespace SWD when this is finished

##Specifications

To build applications that contain applications recursively without sacrificing power or stability, All applications should be configured at the top level application configuration settings, and should be completely stable apart from whatever else happens in the other applications. When a Parent-Application calls multiple Applications, each sibling should be entirely executed AND FALL OUT OF SCOPE before other sibling applications begin.

##The Configuration Array
The configuration array should contain all the settings for all the applications and referenced class/objects within a classname array.
If SWD\Applications\MyApp\Application incorporated SWD\Applications\MyOtherApp\Application_v3 and SWD\MyThirdApp\Application_v2, then the $configuration array would look like the following 

`$configurationArray( `

     'SWD\Applications\MyApp\Application'=>array(), 
     
     'SWD\Applications\MyOtherApp\Application_v3'=>array(), 
     
     'SWD\Applications\MyThirdApp\Application_v2'=>array()
     
`);`

And if the applications use objects that require configuration within this array, those values should be encapsulated within their own namespace string keys as such,

`$configurationArray(`

     'SWD\Applications\MyApp\Application'=>array(), 
     
     'SWD\Applications\MyOtherApp\Application_v3'=>array(), 
     
     'SWD\Applications\MyThirdApp\Application_v2'=>array()
     
     'SWD\MySubNameSpace\MyClass'=>array()
     
     'SWD\OtherClass'=>array()
     
`);`

Settings should never be passed in via a single dimensional array, as this breaks the pattern for the constructor and predictability / integratability.

##Application class __constructors

Each Application constructor should include the following code to handle it's configuration options

`public function __construct($configOptionList){`

    $thisNameSpace = get_class($this);
    
    foreach ($configOptionList[$thisNameSpace] as $parameter => $value){
    
      $this->'set_opt_' . $parameter($value);
        
    }
    
`}`

##Application output

Each Application should be executed by the method `execute_runtime();` 

1. This method should never accept any passed variables. 
2. This method should always return a string of output



##USE OF GLOBALS

Usage of globals is out of spec, unless they are set and unset within the same application scope. This practice is discouraged, although not strictly speaking out of spec. [You get to be technically right, which IS the best kind of right...]

##USE OF SUPERGLOBALS

This is necessarily within scope for numerous use-cases. We handle this by stipulating that all superglobals MUST be namespaced  (i.e. set_cookie('SWD\MyApplication\Application'.$cookiename, $value);

