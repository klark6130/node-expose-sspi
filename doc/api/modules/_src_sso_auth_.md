[node-expose-sspi](../README.md) › [Globals](../globals.md) › ["src/sso/auth"](_src_sso_auth_.md)

# Module: "src/sso/auth"

## Index

### Functions

* [auth](_src_sso_auth_.md#auth)

## Functions

###  auth

▸ **auth**(`options`: [AuthOptions](../interfaces/_src_sso_interfaces_.authoptions.md)): *[Middleware](_src_sso_interfaces_.md#middleware)*

*Defined in [src/sso/auth.ts:21](https://github.com/jlguenego/node-expose-sspi/blob/45f90aa/src/sso/auth.ts#L21)*

Tries to get SSO information from browser. If success, the SSO info
is stored under req.sso

**`export`** 

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`options` | [AuthOptions](../interfaces/_src_sso_interfaces_.authoptions.md) | {} |

**Returns:** *[Middleware](_src_sso_interfaces_.md#middleware)*
