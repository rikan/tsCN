#3	Types
TypeScript adds optional static types to JavaScript. Types are used to place static constraints on program entities such as functions, variables, and properties so that compilers and development tools can offer better verification and assistance during software development. TypeScript’s **static** compile-time type system closely models the **dynamic** run-time type system of JavaScript, allowing programmers to accurately express the type relationships that are expected to exist when their programs run and have those assumptions pre-validated by the TypeScript compiler. TypeScript’s type analysis occurs entirely at compile-time and adds no run-time overhead to program execution.

All types in TypeScript are subtypes of a single top type called the Any type. The any keyword references this type. The Any type is the one type that can represent any JavaScript value with no constraints. All other types are categorized as **primitive types** or **object types**. These types introduce various static constraints on their values.

The primitive types are the Number, Boolean, String, Null, and Undefined types. The number, bool, and string keywords reference the Number, Boolean, and String primitive types respectively. It is not possible to explicitly reference the Null and Undefined types—only values of those types can be referenced, using the null and undefined literals.

The object types are all class, module, interface, and literal types. Class, module, and interface types are introduced through class, module, and interface declarations and are referenced by the name given to them in their declarations. Literal types are written as object, array, function, or constructor type literals and are used to compose new types from other types.

A special Void type exists purely to indicate the absence of a value, such as in a function with no return value.

Declarations of modules, classes, properties, functions, variables and other language entities associate types with those entities. The mechanism by which a type is formed and associated with a language entity depends on the particular kind of entity. For example, a module declaration associates the module with an anonymous type containing a set of properties corresponding to the exported variables and functions in the module, and a function declaration associates the function with an anonymous type containing a call signature corresponding to the parameters and return type of the function. Types can be associated with variables through explicit **type annotations**, such as

	var x: number;
or through implicit **type inference**, as in

	var x = 1;
which infers the type of ‘x’ to be the Number primitive type because that is the type of the value used to initialize ‘x’.

*NOTE*: The TypeScript compiler currently implements an experimental form of enum types. We expect the final language to support enum types, but not in the form they are currently implemented.

*NOTE*: TypeScript currently doesn’t support Generics, but we expect to include them in the final language. Since TypeScript’s static type system has no run-time manifestation, Generics will be based on “type erasure” and intended purely as a conduit for expressing parametric type relationships in interfaces, classes, and function signatures.

##3.1	The Any Type
The Any type is used to represent any JavaScript value. A value of the Any type supports the same operations as a value in JavaScript and no static type checking is performed for operations on Any values. Specifically, properties of any name can be accessed through an Any value and Any values can be called as functions or constructors with any argument list.

The any keyword references the Any type. In general, in places where a type is not explicitly provided and TypeScript cannot infer one, the Any type is assumed.

The Any type is a supertype of all types.

Some examples:

	var x: any;             // Explicitly typed
	var y;                  // Same as y: any
	var z: { a; b; };       // Same as z: { a: any; b: any; }

	function f(x) {         // Same as f(x: any): void
    console.log(x);
	}

##3.2	Primitive Types
The primitive types are the Number, Boolean, String, Null, and Undefined types.
###3.2.1	The Number Type
The Number primitive type corresponds to the similarly named JavaScript primitive type and represents double-precision 64-bit format IEEE 754 floating point values.

The number keyword references the Number primitive type and numeric literals may be used to write values of the Number primitive type.

For purposes of determining type relationships (section 3.6) and accessing properties (section 4.10), the Number primitive type behaves as an object type with the same properties as the global interface type ‘Number’ plus its own unique brand.

Some examples:

	var x: number;          // Explicitly typed
	var y = 0;              // Same as y: number = 0
	var z = 123.456;        // Same as z: number = 123.456
	var s = z.toFixed(2);   // Property of Number interface
###3.2.2	The Boolean Type
The Boolean primitive type corresponds to the similarly named JavaScript primitive type and represents logical values that are either true or false.

The bool keyword references the Boolean primitive type and the true and false literals reference the two Boolean truth values.

For purposes of determining type relationships (section 3.6) and accessing properties (section 4.10), the Boolean primitive type behaves as an object type with the same properties as the global interface type ‘Boolean’ plus its own unique brand.

Some examples:

	var b: bool;            // Explicitly typed
	var yes = true;         // Same as yes: bool = true
	var no = false;         // Same as no: bool = false
###3.2.3	The String Type
The String primitive type corresponds to the similarly named JavaScript primitive type and represents sequences of characters stored as Unicode UTF-16 code units.

The string keyword references the String primitive type and string literals may be used to write values of the String primitive type.

For purposes of determining type relationships (section 3.6) and accessing properties (section 4.10), the String primitive type behaves as an object type with the same properties as the global interface type ‘String’ plus its own unique brand.

Some examples:

	var s: string;          // Explicitly typed
	var empty = "";         // Same as empty: string = ""
	var abc = 'abc';        // Same as abc: string = "abc"
	var c = abc.charAt(2);  // Property of String interface
###3.2.4	The Null Type
The Null type corresponds to the similarly named JavaScript primitive type and is the type of the null literal.

The null literal references the one and only value of the Null type. It is not possible to directly reference the Null type itself.

The Null type is a subtype of all types, except the Void and Undefined types. This means that null is considered a valid value for all primitive and object types, including even the Number and Boolean primitive types.

Some examples:

	var n: number = null;   // Primitives can be null
	var x = null;           // Same as x: any = null
	var e: Null;            // Error, can't reference Null type
###3.2.5	The Undefined Type
The Undefined type corresponds to the similarly named JavaScript primitive type and is the type of the undefined literal.

The undefined literal denotes the value given to all uninitialized variables and is the one and only value of the Undefined type. It is not possible to directly reference the Undefined type itself.

The undefined type is a subtype of all types. This means that undefined is considered a valid value for all primitive and object types. Furthermore, undefined is the only possible value for the Void type.

Some examples:

	var n: number;          // Same as n: number = undefined
	var x = undefined;      // Same as x: any = undefined
	var e: Undefined;       // Error, can't reference Undefined type
##3.3	Object Types
The object types are all class, module, interface, and literal types. Every object type is composed from zero or more of the following kinds of members:

+ **Properties**, which define the names and types of the properties of an object of the given type. Property names are unique within their type.
+ **Call signatures**, which define the possible parameter lists and the return type associated with applying a call operation to an object of the given type.
+ **Construct signatures**, which define the possible parameter lists and the return type associated with applying the new operator to an object of the given type.
+ **Index signatures**, which define the index expression types and the return type associated with applying an index operation to an object of the given type.
+ **Brands**, which denote categories the given object type belongs to. Unique brands are automatically added to all class types and are not present in other object types.

Properties are either **public** or **private** and are either **required** or **optional**:

+ Properties in a class declaration may be designated public or private, while properties of all other types are always considered public. Private members are only accessible within the class body containing their declaration, as described in section 8.2.1.
+ Properties in an object type literal or interface declaration may be designated required or optional, while properties of all other types are always considered required. Properties that are optional in the target type of an assignment may be omitted from source objects, as described in section 3.6.3.

Brands are purely an artifact of TypeScript’s type system. They have no run-time representation or cost. Brands serve mainly as a mechanism for restricting compatibility between types that otherwise have the same structure. For example, two classes with exactly the same member declarations are still not compatible because each class has a distinct brand.

For purposes of determining type relationships (section 3.6) and accessing properties (section 4.10), object types appear to have certain additional members:

+ Every object type appears to have the members of the global interface type ‘Object’ unless those members are hidden by members in the object type.
+ An object type with one or more call or construct signatures appears to have the members of the global interface type ‘Function’ unless those members are hidden by members in the object type.

Object type members hide ‘Object’ or ‘Function’ interface members in the following manner:

+ A property hides an ‘Object’ or ‘Function’ property with the same name.
+ A call signature hides an ‘Object’ or ‘Function’ call signature with the same number of parameters and identical parameter types in the respective positions.
+ A construct signature hides an ‘Object’ or ‘Function’ construct signature with the same number of parameters and identical parameter types in the respective positions.
+ An index signature hides an ‘Object’ or ‘Function’ index signature with the same parameter type.

In effect, object types are subtypes of the ‘Object’ or ‘Function’ interface unless the object types define members that are incompatible with those of the ‘Object’ or ‘Function’ interface—which, for example, occurs if an object type defines a property with the same name as a property in the ‘Object’ or ‘Function’ interface but with a type that isn’t a subtype of that in the ‘Object’ or ‘Function’ interface.

Some examples:

	Object o = { x: 10, y: 20 };         // Ok
	Function f = (x: number) => x * x;   // Ok
	Object err = { toString: 0 };        // Error, incompatible toString
##3.4	The Void Type
The Void type, referenced by the void keyword, is used as the return type for functions that return no value. The only place the Void type may appear is in a return type annotation for a function.

A call expression that calls a void function is of type Void. Such an expression can appear only in an expression statement, in a return statement, or as the body of an arrow function expression. In all other contexts, an expression of type Void is an error.

The only possible value for the Void type is undefined.

The Void type is a subtype of the Any type and a supertype of the Undefined type, but otherwise Void is unrelated to all other types.

##3.5	Specifying Types
Types are specified either by referencing their keyword or name or by writing type literals which compose other types into new types.

+ *Type*:  
*PredefinedType*  
*TypeName*  
*TypeLiteral*

+ *PredefinedType*:  
any  
number  
bool  
string

+ *TypeName*:  
*ModuleOrTypeName*

+ *ModuleOrTypeName*:  
*Identifier*  
*ModuleName   .   Identifier*

+ *ModuleName*:  
*ModuleOrTypeName*

+ *TypeLiteral*:  
*ObjectType*  
*ArrayType*  
*FunctionType*  
*ConstructorType*
###3.5.1	Predefined Types
The any, number, bool, and string keywords reference the Any type and the Number, Boolean, and String primitive types respectively. These keywords are reserved and cannot be used as type names.
###3.5.2	Type Names
Class, module, and interface declarations introduce **named types** which may be referenced by their name wherever a type is expected.

Resolution of a *ModuleOrTypeName* consisting of a single identifier is described in section 2.3.

Resolution of a *ModuleOrTypeName* of the form *M.N*, where *M* is a *ModuleName* and *N* is an *Identifier*, proceeds by first resolving the module name *M*. If the resolution of *M* is successful and the resulting module contains an exported class, module, or interface member *N*, then *M.N* refers to that member. Otherwise, *M.N* is undefined.

A *TypeName* is a *ModuleOrTypeName* that can refer to a class, module, or interface. When a *TypeName* refers to a module it denotes the object type associated with the module. When resolution of a *TypeName* fails, an error is reported and the Any type is substituted.

A *ModuleName* is a *ModuleOrTypeName* that must refer to a module.














