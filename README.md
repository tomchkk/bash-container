Bash Container
==============

A wicked service container for Bash
-----------------------------------

Yes, you read right: Bash Container gives you - near as dammnit - the power of dependency injection in Bash!

No more hard paths in scripts forever linking one another in a rigid marriage destined for despare. With Bash Container you can define a script's dependencies in one place - achieving inversion of control in one fell swoop.

Under the hood, Bash Container extends the bash _builtin_ `source` command. `source` still works in the usual way with a plain old path to an undefined script. However, pass in a registered service name, or its path, and Bash Container's powerful dependency-loader functionality springs in to action. All of the service's dependencies will be injected as arguments to the underlying `source` call, including any additional run-time arguments that you care to throw at it.

A service's dependencies can be scalar values, array-like values and even other services, which can then themselves be `source`d within the defining service, beginning the merry dance all over again!

## Installation

TODO: installation details, how to wire-up bash-container itself

## Usage

### Defining container items

Bash Container can manage three types of item definition: scalar, array and service.

#### Scalar

Add a scalar value definition using the `container::add_scalar` function and the following signature:

```
container::add_scalar "value-name" "scalar value"
```

#### Array

Add an array value definition with the `container::add_array` function, separating each array element with the positional separator _--_ (also used optionally before the first element):

```
container::add_array "array-name" "element 1" -- "element 2"
container::add_array "array-name" -- "element 1" -- "element 2"
```

#### Service

Add a service definition with the `container::add_service` function, followed by the path to the source file and then any remaining arguments - as follows:

```
container::add_service "service-name" "path/2/file" \
  --named-arg1="@another-service" \
  --named-arg2="arg2 value" \
  -- "positional arg 1" \
  -- "positional arg 2"
```

### Dereferencing Container Items

Items in the container can be referred to using an alias, which consists of the `@` character followed by the service name. For example:

```
container::add_scalar "value-1" "Value 1"
container::add_scalar "value-2" "@value-1"
```

The resolved value of the `value-2` item would be `Value 1`.

One, or more aliases can be embedded between a prefix and/or a suffix to achieve an accumulation of resolved values, using curly brackets `{}` to encapsulate the referenced service name where there is no clear word boundary. For example:

```
container::add_scalar "value-1" "Value 1"
container::add_scalar "value-2" "Value 2"
container::add_scalar "value-3" "prefix@{value-1}suffix @value-2"
```

The resolved value of the `value-3` item above would be `prefixValue 1suffix Value 2`.

Escaping the alias indicator can be achieved with a leading `\`, as in:

```
container::add_scalar "email-address" "name\@email.com"
```

### Loading Container Services

Once defined, container resources services can be loaded by calling the `loader::source` function, followed by the service alias, as well as any trailing run-time arguments - e.g.:

```
loader::source @service-1 "argument value" --named-arg="named argument"
```

All of `service-1`'s dependencies, as defined in the container will be injected into the script including with trailing arguments. Positional parameters will be asssigned to `$@` and `$1`, `$2`, etc as usual. Any named parameters will be normalized and declared as a parameter within the script. For example, `--named-arg="named argument"` would be declared as `named_arg` with a value of _named argument_.
