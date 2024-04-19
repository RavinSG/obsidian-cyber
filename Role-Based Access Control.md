RBAC is used when authorisation is *determined by a user's role within an organization*. For example, a user in the marketing department may have access to user analytics but not network administration.

![[Role-Based Access Control.png]]

RBAC systems boil down to three primary rules:

- *Role assignment*, which states that subjects can use only permissions that match a role they have been assigned. 
  
- *Role authorisation*, which states that the subject's active role must be authorised for the subject. This prevents subjects from taking on roles they shouldn't be able to.
  
- *Permission authorisation*, which states that subjects can use only permissions that their active role is allowed to use.