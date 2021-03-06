---
title: "modUser.hasSessionContext"
_old_id: "1345"
_old_uri: "2.x/developing-in-modx/other-development-resources/class-reference/moduser/moduser.hassessioncontext"
---

## modUser::hasSessionContext

Checks if the user has a specific session context, or in other words, is "logged into" a certain context.

## Syntax

API Doc: [http://api.modx.com/revolution/2.2/db\_core\_model\_modx\_moduser.class.html#%5CmodUser::hasSessionContext()](http://api.modx.com/revolution/2.2/db_core_model_modx_moduser.class.html#%5CmodUser::hasSessionContext())

``` php 
boolean hasSessionContext (mixed $context)
```

## Example

See if the User has a Session for the 'sports' Context:

``` php 
if ($user->hasSessionContext('sports')) {
    // do code here
}
```

## See Also

- [modUser](developing-in-modx/other-development-resources/class-reference/moduser "modUser")
- [Contexts](building-sites/contexts "Contexts")