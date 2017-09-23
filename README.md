# ZHONGKUI

## Introduction

ZHONGKUI is a json checker and converter.

There are some other great tool like json-schema or cerberus work well.
But I feel they are not intuitionistic at all.

ZHONGKUI is made to validate json like the way it should be.

eg:

json like
```
{
    "firstName": "abc",    (required)
    "lastName": "efg",     (required)
    "age": 12,
    "sex": "female"
}
```
in json-schema, it looks like
```
{
    "title": "Person",
    "type": "object",
    "properties": {
        "firstName": {
            "type": "string"
        },
        "lastName": {
            "type": "string"
        },
        "age": {
            "description": "Age in years",
            "type": "integer",
            "minimum": 0,
            "maximum": 150,
            "exclusiveMaximum": true
        },
        "role": {
            "type": "integer",
            "enum": [1, 2, 3],
            "default" : 1
        }
        "sex": {
            "type": "string",
            "enum": ["female", "male"]
        }
    },
    "required": ["firstName", "lastName", "sex"]
}
```
but in ZHONGKUI, it looks like
```
{
    "firstName": "xxx",
    "lastName": "xxx",
    "age[0:150]?": 12,
    "role?=1": {1, 2, 3},
    "sex": {"female", "male"}
}
```
It's easy to read, write and understand.

## Syntax

ZHONGKUI is designed to be simple and free, so the symbol below is allowed
anywhere of the key name.

That means `=1[1:2]?` is equal to `?=1[1:2]` and `?[1:2]=1`.

- Value type is decide by the type of sample value.
- Enum is represented by set.
- Date representation, as defined by RFC 3339, section 5.6.
- `?` means that key is optional.
- `=` means the key has a default value when it's not provided.
- `[:]` means the value is inside the range. it means min and max when
value type is numeric and min length and max length when value type is string
or array.

## Installation

    $ pip3 install zhongkui

## Python versions

ZHONGKUI supports Python 3.4.1+.

## Usage

    >>> import zhongkui
    >>> template = {
            "firstName": "xxx",
            "lastName": "xxx",
            "age[0:150]?": 12,
            "role?=1": {1, 2, 3},
            "sex": {"female", "male"}
        }
    >>> raw = '{"firstName": "abc", "lastName": "efg", "age": 12, "sex": "female"}'
    >>> print(zhongkui.json2object(raw))
    {
        "firstName": "abc",
        "lastName": "efg",
        "age": 12,
        "role": 1,
        "sex": "female"
    }

## CAUTION

ZHONGKUI IS WIP NOW AND NOT TESTED WELL.
