[node-expose-sspi](../README.md) › [Globals](../globals.md) › ["src/sso/interfaces"](_src_sso_interfaces_.md)

# Module: "src/sso/interfaces"

## Index

### Interfaces

* [ADUser](../interfaces/_src_sso_interfaces_.aduser.md)
* [AuthOptions](../interfaces/_src_sso_interfaces_.authoptions.md)
* [CookieList](../interfaces/_src_sso_interfaces_.cookielist.md)
* [Database](../interfaces/_src_sso_interfaces_.database.md)
* [User](../interfaces/_src_sso_interfaces_.user.md)

### Type aliases

* [ADUsers](_src_sso_interfaces_.md#adusers)
* [Middleware](_src_sso_interfaces_.md#middleware)
* [NextFunction](_src_sso_interfaces_.md#nextfunction)

## Type aliases

###  ADUsers

Ƭ **ADUsers**: *[ADUser](../interfaces/_src_sso_interfaces_.aduser.md)[]*

*Defined in [src/sso/interfaces.ts:98](https://github.com/jlguenego/node-expose-sspi/blob/f44ba74/src/sso/interfaces.ts#L98)*

___

###  Middleware

Ƭ **Middleware**: *function*

*Defined in [src/sso/interfaces.ts:16](https://github.com/jlguenego/node-expose-sspi/blob/f44ba74/src/sso/interfaces.ts#L16)*

#### Type declaration:

▸ (`req`: IncomingMessage, `res`: ServerResponse, `next`: [NextFunction](_src_sso_interfaces_.md#nextfunction)): *void*

**Parameters:**

Name | Type |
------ | ------ |
`req` | IncomingMessage |
`res` | ServerResponse |
`next` | [NextFunction](_src_sso_interfaces_.md#nextfunction) |

___

###  NextFunction

Ƭ **NextFunction**: *function*

*Defined in [src/sso/interfaces.ts:22](https://github.com/jlguenego/node-expose-sspi/blob/f44ba74/src/sso/interfaces.ts#L22)*

#### Type declaration:

▸ (`error?`: Error): *void | Promise‹void›*

**Parameters:**

Name | Type |
------ | ------ |
`error?` | Error |