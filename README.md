Bash Container
==============

A wicked service container for Bash
-----------------------------------

Yes, you read right: Bash Container gives you - near as dammnit - the power of dependency injection in Bash!

No more hard paths in scripts forever linking one another in a rigid marriage destined for despare. With Bash Container you can define a script's dependencies in once place - achieving inversion of control in one fell swoop.

Under the hood, Bash Container extends the bash _builtin_ `source` command. `source` still works in the usual way with a plain old path to an undefined script. However, pass in a registered service name, or its path, and Bash Container's powerful dependency-loader functionality springs in to action. All of the service's dependencies will be injected as arguments to the underlying `source` call, including any additional run-time arguments that you care to throw at it.

A service's dependencies can be scalar values, arrays and even other services, which can then themselves be `source`d within the defining service, beginning the merry dance all over again!

## Installation

TODO: installation details, how to source bash-container itself

## Usage

### Defining container items

Bash Container can manage three types of definition.

#### Scalar

Add a scalar value definition as follows:

```
container::add_value "value-name" "scalar value"
```

Internally Bash Container stores a scalar definition with the following meta data:

```
--type=scalar --value="scalar value"
```

#### Array

Add an array value definition as follows, separating each array element with the positional separator _--_:

```
container::add_array "array-name" -- "element 1" -- "element 2"
```

Internally Bash Container stores an array definition with the following meta data:

```
--type=array -- "element 1" -- "element 2"
```

#### Service

Add a service definition as follows:

```
container::add_service "service-name" "path/2/file" \
  --named-arg1="@definition-alias" \
  --named-arg2="arg2 value" \
  -- "positional arg 1" \
  -- "positional arg 2"
```

Internally Bash Container stores a service definition with the following meta data:

```
--type=service --source="path/2/file name"      
  --named-arg1="arg value"
  --named-arg2="arg2 value"
  -- "positional arg 1"
  -- "positional arg 2"
```

Using a dependency alias to resolve the path to a service is a bit buggy, so
best to avoid this, at time of writing. However, a value can be concatenated
to the end of a resolved scalar parameter using '::' as the glue - e.g.:
"@parameter-value::concatenated-value"
