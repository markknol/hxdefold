[![Build Status](https://travis-ci.org/hxdefold/hxdefold.svg?branch=master)](https://travis-ci.org/hxdefold/hxdefold)

# Haxe/Lua externs for the Defold game engine

**Status: PRE-ALPHA, very much work in progress, API may change. Currently requires latest Haxe [development builds](http://builds.haxe.org/).**

## Example

```haxe
// define script component data
// fields having @property metadata are visible in component editor
typedef GreeterData = {
    @property(true) var greet:Bool;
}

// define script by inheriting `Script` class with specified data type
class Greeter extends defold.support.Script<GreeterData> {
    // override callback function
    override function init(data:GreeterData) {
        if (data.greet)
            trace('Hello, World!');
    }
}
```

## Documentation

Required haxe compilation flags:
 * `-lib hxdefold` - use this library, obviously
 * `-lua something.lua` - the output lua module
 * `-D luajit` - compile for LuaJIT, because that's what Defold uses
 * `--macro defold.support.ScriptMacro.use(".")` - generate glue `.script` files for script classes, first argument is the path to Defold project root folder

More docs to be written...

Here's some [generated Haxe API documentation](http://hxdefold.github.io/hxdefold/).

And example Defold projects, ported from Lua:
 * https://github.com/hxdefold/hxdefold-example-sidescroller
 * https://github.com/hxdefold/hxdefold-example-frogrunner

## How does it work?

Since version 3.3, Haxe supports compiling to Lua, making it possible to use Haxe with Lua-based engines, such as Defold.

However, this requires a bit of autogenerated glue code, because Defold expects scripts to be in separate files, while
Haxe compiles everything in a single lua module. So what we do, is generate a simple glue `.script` file for each class
extending `defold.support.Script` base class.

For example, for the `Greeter` script from this README, this glue code is generated in the `Greeter.script` file:

```lua
-- Generated by Haxe, DO NOT EDIT (original source: src/Greeter.hx:8: lines 8-14)

require "main"

go.property("greet", true)

local script = Greeter.new()

function init(data)
    script:init(data)
end
```

You can then use this script in Defold game objects.
