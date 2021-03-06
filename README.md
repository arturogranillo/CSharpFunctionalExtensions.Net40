Functional Extensions for C# NET 4.0

[Original version](https://github.com/vkhorikov/CSharpFunctionalExtensions).

======================================================
[![Nuget downloads](https://img.shields.io/nuget/v/csharpfunctionalextensions.net40.svg)](https://www.nuget.org/packages/CSharpFunctionalExtensions.Net40/)
[![GitHub license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/vkhorikov/CSharpFunctionalExtensions/blob/master/LICENSE)

This library helps write code in more functional way. To get to know more about the principles behind it, check out the [Applying Functional Principles in C# Pluralsight course](http://pluralsight.com/courses/csharp-applying-functional-principles).

## Installation

Available on [nuget](https://www.nuget.org/packages/CSharpFunctionalExtensions.Net40/)

	PM> Install-Package CSharpFunctionalExtensions.net40

## Get rid of primitive obsession

```csharp
Result<CustomerName> name = CustomerName.Create(model.Name);
Result<Email> email = Email.Create(model.PrimaryEmail);

Result result = Result.Combine(name, email);
if (result.IsFailure)
    return Error(result.Error);

var customer = new Customer(name.Value, email.Value);
```

## Make nulls explicit with the Maybe type

```csharp
Maybe<Customer> customerOrNothing = _customerRepository.GetById(id);
if (customerOrNothing.HasNoValue)
    return Error("Customer with such Id is not found: " + id);
```

## Compose multiple operations in a single chain

```csharp
return _customerRepository.GetById(id)
    .ToResult("Customer with such Id is not found: " + id)
    .Ensure(customer => customer.CanBePromoted(), "The customer has the highest status possible")
    .OnSuccess(customer => customer.Promote())
    .OnSuccess(customer => _emailGateway.SendPromotionNotification(customer.PrimaryEmail, customer.Status))
    .OnBoth(result => result.IsSuccess ? Ok() : Error(result.Error));
```

## Readings and watchings

 * [Functional C#: Primitive obsession](http://enterprisecraftsmanship.com/2015/03/07/functional-c-primitive-obsession/)
 * [Functional C#: Non-nullable reference types](http://enterprisecraftsmanship.com/2015/03/13/functional-c-non-nullable-reference-types/)
 * [Functional C#: Handling failures, input errors](http://enterprisecraftsmanship.com/2015/03/20/functional-c-handling-failures-input-errors/)
 * [Applying Functional Principles in C# Pluralsight course](http://pluralsight.com/courses/csharp-applying-functional-principles)
 
## Contributors
A big thanks to [yakimovim](https://github.com/yakimovim) and [alex-erygin](https://github.com/alex-erygin) for contributions!
