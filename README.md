# Ruby Nagios plugin

A Useful library that makes easy the creation of nagios plugin using Ruby.


## Install

To install the library use the following command :

```
gem install ruby-nagios-plugin
```


## Example

An example wordth more than nothing. Suppose you want to monitor a remote service which is exposing an endpoint for monitoring purpose : `api.service.com/health`.
You're team are using Nagios as a monitoring tool and you are asked to write a check that tells the state of tis service regularly.

You are a great fan of Ruby and you don't want to use the built-in plugin check_http which doesn't meet your needs.
You decide to write a custom Nagios plugin in Ruby and  
 are using Nagion To monitor this se; 
and that this service is exposing an API endpoint to check whether or not   a plugin that execute a check on a remote API.
The API your want Using the library is as easy as installing it. Add the following import statement at the top of your ruby code:

```ruby
require 'nagios-plugin'

# ... your code

```

The library provides as a Ruby module `Nagios::Plugin`. The module provide some useful objects and some helpers (modules, functions, ...). As objects we have : 

## API

### Check

It's the first core object provided by the Plugin module. Writing a plugin for Nagios requires to define a `check` whose goals ate :
* To Verify whether or not a host or a service is well performing.
* To produce some performance data use as metric for the check.

<br>

#### .define(props:Block)
This class method creates an instance of a check configured with the given properties (`props`).

##### Parameters
The function requires only one paramter `props` which is a ruby block containing information needed to configure the check.

Below, the listing of available keys that could by use to configure a Check.

key  | Required | Type        | Default | Description
-----|----------|----------   |---------| ---------
name    | true     | `String`  | `nil`      | The name of your check. It is informational
uom     | true     | `Plugin::UOM`  | `nil`      | The unit of measurement used in this check. Check the list of available UOM provided by the library
flags   | true     | `Hash`  | {}      | Used to set documentation string for flags supported by your check. Available flags are describe in the [Nagion plugin guidelines](http://nagios-plugins.org/doc/guidelines.html#PLUGOPTIONS). If no flags are provided, the Usage text will use the default ones.
logic   | true     | `(c:Check, opts:Hash) -> result:Hash`   | `nil`      | The code logic of your plugin. It should return a hash containing information useful to state that a check succeeded or failed.

##### Returns

It returns a `Check` object.

##### Example

Suppose we want to create a simple chech that verify if we have anougth disk space on our host. The declaration of this plugin will look like : 

```ruby
check = Nagios::Plugin::Check.define do | c |
  c.name = "check-disk-space"
  
  c.uom = Plugin::UOM.PERCENT
  
  c.flags = {
    w: "Warning threshold for used space (%)",
    c: "Critical threshold for used space (%)"
  }
  
  c.logic = lambda do |check, options| 
    # Put your code logic here
  end

end
```


#### Threshold / Range 

A **Threshold** is compose of 3 collections of ranges; one for each level of alerts : Warning and critical.

A **Range** has : 
* A numerical start point 
* A numerical end point

#### Performance Data

**Check** produce performance data that are represented in its ouput as a collection of key/value items. A Check performance data is value if it matches the following pattern : 
```
<label>=<value[<UOM>]>;[<warn>];[<crit>];[<min>];[<max>]
```
Then, make sure to match this output format for your performance data.


**UOM** means Unit Of Measurement. It specify the unit of measurement of the performance data. Is is written as a ruby module and provides some common units to use.

* Time (s/ms/us)
* Percentage (%)
* Storage Size (B,KB, MB, TB)
* Continous counter (c). ***Ex**:  Bytes transmitted on an interface*



To see the full API and core object provided by the library, go the next section.

<br>

## API

### Check

#### Properties

#### Methods

##### Create : (props) -> Check

###### Params
Name  | Default | Desc 
------- | ------ | ----- 
`props` | `{}` | Check properties
###### Return value



<br>

## References

* [Nagios Core Doc | Nagios Plugin API](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/pluginapi.html)
* [Nagios Core Doc | Nagios Plugin Developer Guide](https://nagios-plugins.org/doc/guidelines.html#_ga=2.10148662.243997971.1545923831-197797091.1545923831)
* [Doc.monitoring-fr.org | API pour les plugins Nagios](https://doc.monitoring-fr.org/3_0/html/development-pluginapi.html)
