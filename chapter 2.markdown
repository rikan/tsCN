2 Basic Concepts
===
The remainder of this document is the formal specification of the TypeScript programming language and is intended to be read as an adjunct to the ECMAScript Language Specification (specifically, the ECMA-262 Standard, 5th Edition). This document describes the syntactic grammar added by TypeScript along with the compile-time processing and type checking performed by the TypeScript compiler, but it only minimally discusses the run-time behavior of programs since that is covered by the ECMAScript specification.
2.1	Grammar Conventions
---
The syntactic grammar added by TypeScript language is specified throughout this document using the existing conventions and production names of the ECMAScript grammar. In places where TypeScript augments an existing grammar production it is so noted. For example:

	CallExpression:  ( Modified )
	…
	super   Arguments
	super   .   Identifier
The ‘( Modified )’ annotation indicates that an existing grammar production is being replaced, and the ‘…’ references the contents of the original grammar production.
2.2	Declarations
---
Declarations introduce names in the **declaration spaces** to which they belong. It is an error to have two names with same spelling in the same declaration space. Declaration spaces exist as follows:

+ The global module has a declaration space for global members (variables, functions, modules, and constructor functions), and a declaration space for global types (modules, classes, and interfaces).
+ Each module body has a declaration space for local members (variables, functions, modules, and constructor functions), and a declaration space for local types (modules, classes, and interfaces). Every declaration (whether local or exported) in a module contributes to one or both of these declaration spaces.
+ Each internal or external module has a declaration space for exported members and a declaration space for exported types. All export declarations in the module contribute to these declaration spaces. Each internal module’s export declaration spaces are shared with other internal modules that have the same root module and the same qualified name starting from that root module.
+ Each interface type has a declaration space for its members. An interface’s declaration space is shared with other interfaces that have the same root module and the same qualified name starting from that root module.
+ Each object type literal has a declaration space for its members.
+ Each class declaration has a declaration space for instance members and a declaration space for static members.
+ Each function declaration (including constructor, member function, and member accessor declarations) and each function expression has a declaration space for locals (parameters, variables, and functions).
+ Each object literal has a declaration space for its properties.

Top-level declarations in a non-module source file belong to the **global module**. Top-level declarations in a module source file belong to the external module represented by that source file.

An internal module declaration contributes both a member name (representing the module instance) and a type name (representing the module instance type) to the containing module. Likewise, a class declaration contributes both a member name (representing the constructor function) and a type name (representing the class instance type) to the containing module. An interface declaration contributes a type name (but no member name) to the containing module. Any other declaration contributes a member name (but no type name) to the declaration space to which it belongs.

The **root module** of a member or type is the outermost module within which the member or type is reachable. The root module of a member or type M in a parent module P is determined as follows:

+ If P is the global module or an external module, M’s root module is P.
+ If M is not exported, M’s root module is P.
+ If M is exported, M’s root module is the root module of P.

Interfaces and internal modules are “open ended,” and interfaces and internal module declarations with the same qualified name relative to a common root are automatically merged. For further details, see section 9.3.

Type names are in separate declaration spaces from other names. This means a module can declare both a type and a member with the same name, for example:

	module M
	{
    	interface X { }     // Type
    	var X;              // Member
	}
Note that an error would have occurred if the interface ‘X’ above was instead a class ‘X’ because a class declaration contributes both a member name and a type name to the containing module.

Instance and static members in a class are likewise in separate declaration spaces. Thus the following is permitted:

	class C
	{
    	x: number;          // Instance member
    	static x: string;   // Static member
	}

2.3	Scopes
---
The **scope** of a name is the region of program text within which it is possible to refer to the entity declared by that name without qualification of the name.

+ The scope of a global variable, function, class, module, or interface is the entire program text.
+ The scope of a non-exported variable, function, class, module, or interface declared within a module declaration is the body of that module declaration.
+ The scope of an exported variable, function, class, module, or interface declared in an internal module is the body of that module and every internal module with the same root and the same qualified name relative to that root.
+ The scope of an exported variable, function, class, module, or interface declared in an external module is the body of that module.
+ The scope of a parameter, local variable, or local function declared within a function declaration (including a constructor, member function, or member accessor declaration) or function expression is the body of that function declaration or function expression.

Scopes may overlap, for example through nesting of modules and functions. When the scopes of two entities with the same name overlap, one name takes precedence over the other and access to the hidden name is either not possible or only possible by qualifying the name.

When an identifier is used as a *ModuleOrTypeName* (section 3.5), all identifiers not representing classes, modules, or interfaces are hidden, and identifiers declared in nested modules take precedence over identifiers declared in enclosing modules.

When an identifier is used as a *PrimaryExpression* (section 4.3), identifiers declared in nested modules or functions take precedence over identifiers declared in enclosing modules or functions.

Note that class members are never directly in scope—they can only be accessed by applying the dot (‘.’) operator to a class instance. This even includes members of the current instance in a constructor or member function, which are accessed by applying the dot operator to this.


