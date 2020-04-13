Bash Container
==============

A rudimentary services container for Bash
-----------------------------------------

## Usage

### Defining container items

The syntax for adding items to the container is as follows:
  - a scalar parameter:
    ```
    container::add "parameter-name" "parameter-value"
    ```
  - an array-like parameter:
    ```
    container::add "parameter-name" "--" "parameter-value-1" "parameter-value-2"
    ```
  - a service:
    ```
    container::add "service-name" "path/2/service" "@dependency-alias" [,...]
    ```

Using a dependency alias to resolve the path to a service is a bit buggy, so
best to avoid this, at time of writing. However, a value can be concatenated
to the end of a resolved scalar parameter using '::' as the glue - e.g.:
"@parameter-value::concatenated-value"
