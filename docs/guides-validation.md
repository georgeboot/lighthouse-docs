---
id: validation
title: Validation
---

## Validating Arguments

Lighthouse allows you to use [Laravel's validation](https://laravel.com/docs/validation) for your
queries and mutations. The simplest way to leverage the built-in validation rules is to use the
[@rules](directives#rules) directive.

```graphql
type Mutation {
  createUser(
    name: String @rules(apply: ["required", "min:4"])
    email: String @rules(apply: ["email"])
  ): User
}
```

In the case of a validation error, Lighthouse will abort execution and return the validation messages
as part of the response.

```graphql
mutation {
  createUser(email: "hans@peter.xyz"){
    id
  }
}
```

```json
{  
  "data":{  
    "foo":null
  },
  "errors":[  
    {  
      "message":"validation",
      "locations":[  
        {  
          "line":2,
          "column":13
        }
      ],
      "validation":[  
        "The name field is required."
      ]
    }
  ]
}
```

You can customize the error message for a particular argument.

```graphql
@create(apply: ["max:140"], message: "Tweets have a limit of 140 characters")
```

## Custom Validator Classes

Use the [@validate](directives#validate) directive to validate entire fields
with a custom validator class.
