---
title: "Typescript Type Narrowing And Assertions"
datePublished: Tue May 13 2025 08:54:50 GMT+0000 (Coordinated Universal Time)
cuid: cmama03ty003909jug46l6wff
slug: typescript-type-narrowing-and-assertions
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/c8jYxRgu3UU/upload/31e7874f4ebed5e70bcd82c3311730f7.jpeg
tags: typescript, assertion, typesafety

---

This week, we're getting a bit more technical and a bit more into the weeds of [TypeScript](https://www.typescriptlang.org/). This covers how the typescript handles types and behaves when converting from one type to another. Along with the basics of how this works, it also covers what you can do to make this narrowing more robust.

Typescript is a compile-time tool that adds a type system to JavaScript. Typescript does not affect runtime; all type information is discarded during the build/compile process.

Built on JavaScript, TypeScript inherits all of JavaScript's types. As JavaScript is loosely typed, JavaScript will try its best to do type conversion for you, using a process called [coercion](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion); therefore, JavaScript does have types, and these types form the foundation types in TypeScript.

Typescript uses a hierarchical type system, meaning one type can extend from another.

# Types

The base types in the hierarchy are

## Extendable

All these can have child types that extend from them

* String - any value that represents a string of characters
    
* Number - any number, both whole and decimal, from 0 to `Number.MAX_VALUE`
    
* Boolean - the values true or false
    
* Object - An object literal or anything that extends object literals. *Note that Arrays in JavaScript are objects.*
    

## Not Extendable

These can not have any child types

* undefined - only contains the value undefined, this is a special type that means that a variable does not exist or has had its value
    
    explicitly set to undefined, or has not had a value set (implicitly set to undefined)
    
* null - only contains the value null, a special type that means a variable has been defined but has had its value explicitly set to null.
    

## Typescript Types

JavaScript does not use these base types, so they are unavailable at runtime, only at compile time when using TypeScript. None of these types can be extended from

### Never

This special type tells TypeScript that if code ever reaches this, something has gone wrong, i.e. you should never have a path to this type.

```typescript
const runtimeError = (): never => {
    throw new Error("Something went wrong");
}
```

This is never used because if this function is called, it doesn’t return; therefore, we indicate using the never type.

You can not convert to never at all.

### Unknown

This special type tells TypeScript that we don’t know what the type of this variable is, but not to worry about it and that we will clarify it later. To maintain type safety, you should deduce the correct type before using an unknown variable.

### Any

<div data-node-type="callout">
<div data-node-type="callout-emoji">⚠</div>
<div data-node-type="callout-text">Any <strong>SHOULD NOT</strong> be used in production code.</div>
</div>

This is the last of the special types in TypeScript; all types can be converted to any, and all types can be converted to a narrower type from any. This makes it very dangerous, as you discard type checking completely when using any. There is no type-safe way to use the `any` type, the closest is to treat it like `unknown` But if you forget that TypeScript, it will not detect it, unlike with `unknown` where it will.

# Conversion

Now that we have a basic understanding of the type system, we can discuss how they get converted, sometimes called casting. TypeScript uses two strategies when converting: Type Narrowing, which goes down the type hierarchy tree from a broader type to a narrower one, and Type Broadening, which goes in the opposite direction, from a narrow type to a broader one.

## Cross-Type Conversion

For completeness, cross-type conversion, going from one base type to another (i.e., from a string to a number), can not happen implicitly in TypeScript. You must always handle this explicitly using runtime functions such as [`parseInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt).

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">This can happen explicitly in TypeScript (i.e., without using runtime conversion), and you may find information on how to do it. However, this breaks type safety and should not get past a code review. I have intentionally chosen not to cover how to do it here.</div>
</div>

## Broadening

This is the easiest to understand, and the easiest for TypeScript to do. This happens implicitly because it doesn’t require type checking.

When you broaden a type, you discard the extra type information and cast the type to the new broader type. Because it relies on discarding Information, you can not implicitly return to the narrower type once broadening occurs.

## Narrowing

Because narrowing requires more information, TypeScript can rarely do this implicitly; the only time it can is when the value of a variable is guaranteed at compile time, which means if a variable is of one of these types. `string` , `number` , `boolean` **AND** declared with `const`. Objects are not included because of how they work under the hood. An object is considered `const` long as its underlying reference ID never changes, regardless of whether its shape changes. Due to this, even freezing using `Object.freeze` an object won’t guarantee that it won’t change at runtime. The exception to this is a const array, but you have to follow specific rules to declare them.

Because of this, you will most often need to help TypeScript understand how to narrow a type.

## As

When looking this up, you may come across the `as` keyword. The `as` A keyword allows you to instruct TypeScript that you know what is happening and want it to treat your broader type as the narrower type. This is not considered type safe because, as mentioned above, TypeScript only operates at compile time; therefore, it can only guarantee type safety at compile time. Using `as` it will result in possible runtime type mismatches being missed. The `as` keyword will also never allow you to change the type when no overlap exists. This is when the type you are converting to does not exist in the same inheritance chain; in that case, you are trying to cross-convert types.

## Type Assertion Functions

This relatively new feature in TypeScript allows you to narrow the type and maintain type safety (when used correctly). Type Assertions bridge compile time and runtime, meaning they must be guaranteed at compile time. This allows TypeScript to use them when doing type narrowing. For them to be guaranteed at compile time, they must be declared with the `function name() {}` syntax not `const a = () => {}` or `const a = function() {}`. They must take at least one argument of the value you are checking, and should use the type `unknown`. They can take other arguments to help with asserting the type. They can not return a value and should only return if the variable matches the declared type. The type assertion function should throw if the variable does not meet the required type. This mimics the behaviour of assert in the Node standard library `node:assert` and other assertion libraries/languages. Due to the way they are defined, type assertion functions must be declared before use.

An example type assertion would look like

```typescript
function assertAType (value: unknown): asserts value is <TYPE> {
}
```

The `asserts value is <TYPE>`is what makes this a type assertion, where `<TYPE>` is replaced with the type to which you want to assert the value conforms.

An example of this could be

```typescript
import assert from "node:assert";
type Px = `${number}px`

function assertIsPx (value: unknown): asserts value is Px {
  assert.equal(typeof assert, "string", "Px should be a string");
  assert.ok(value.endsWith('px'), 'should end in pixal');
  assert.ok(parseInt(value, 10) !== Number.NaN, "should contain a number");
}

let width: string = "10px"; 
assertIsPx(width);
// width type has now been narrowed from "string" to "Px"
let height: string = "200";
assertIsPx(height);
// this would throw an error because height doesn't confirm to the type rules 
let depth: Px = "200px";
assertIsPx(depth);
// This would pass and no type narrowing is needed so it will remain "Px"
```

For type assertions to be valid, you must define the type rules in the assertion and ensure they cover everything the new type must have. This can be complex; the more complicated the type is, the simpler the assertion is. The above example is simple, and so is its assertion. This is because the following is valid, but pointless as it doesn’t do any type checking; however, if used like the assertion function above, it will mean any variable passed to value will become of type. `Px` As far as TypeScript is concerned, it doesn’t match the type rules.

```typescript
function assertIs (value: unknown): asserts value is Px {}
```

# Fun Fact

If you are wondering why you have to use `function name() {}` over `const name = () => {}` or `const name = function () {}`

This is because of the need to be guaranteed at compile time. `function name() {}` Declares a function; this function exists as soon as the runtime engine starts, and remains immutable until the runtime engine completes and exits.

On the other hand `const name = () => {}` and `const name = function () {}` Assign a reference to a function. This means that `const name` becomes type `Function` which in turn is a child type of `object` , as long as the object reference ID stored in `const name` doesn’t change it, doesn’t violate the immutability of `const` The result is that the name value can’t be guaranteed at compile time, so TypeScript can’t use it.

%%[cc-license]