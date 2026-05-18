# MyApp - WinForms MVP Clean Architecture Template (.NET Framework 4.8)

## Overview

This solution is a production-ready WinForms architecture template inspired by:

- MVP (Model View Presenter)
- Clean Architecture principles
- SOLID principles
- Feature-based organization
- Separation of Concerns

The purpose of this template is to provide a scalable and maintainable structure for desktop applications built with:

- WinForms
- .NET Framework 4.8
- ADO.NET / EF Core / Dapper
- Dependency Injection (optional)
- Feature modularization

This architecture intentionally avoids forcing ASP.NET patterns directly into WinForms while still borrowing the best architectural concepts from modern backend systems.

---

# Architecture Philosophy

WinForms is fundamentally:

- Event-driven
- View-first
- Stateful

Traditional MVC does not fit naturally in WinForms because the user interacts directly with the View (`Form`).

For this reason, this template uses:

# MVP (Model View Presenter)

Where:

- View = UI only
- Presenter = UI behavior orchestration
- UseCases = business/application logic
- Core = business entities/contracts
- Data = persistence implementation

---

# Solution Structure

```plaintext
/MyApp.sln
 ??? MyApp.Core
 ??? MyApp.UseCases
 ??? MyApp.Data
 ??? MyApp.Presentation

```
---

# Project Responsibilities

---

# 1. MyApp.Core

## Purpose

Contains pure business definitions.

This project must NOT depend on any other project.

## Responsibilities

- Entities
- Enums
- Repository interfaces
- Shared contracts
- Value Objects

## Example Structure

```plaintext
MyApp.Core
 ??? Entities
 ??? Enums
 ??? Interfaces
 ?    ??? Repositories
 ??? ValueObjects

```
## Example Classes

- SampleItem
- BaseEntity
- IRepository<T>
- ISampleItemRepository

---

# 2. MyApp.UseCases

## Purpose

Contains application logic and business workflows.

This project orchestrates operations using the contracts defined in `Core`.

## Responsibilities

- DTOs
- Services
- Validation
- Mappers
- Business rules
- Strategies
- Use case orchestration

## Important Rule

This layer communicates with the UI using DTOs.

Never expose Core entities directly to the Presentation layer.

---

## Feature-Based Organization

Everything related to a feature lives together.

Example:

```plaintext
/SampleItems
    /DTOs
    /Interfaces
    /Services
    /Validation
    /Mappers

```
This avoids large global folders and improves scalability.

---

# 3. MyApp.Data

## Purpose

Contains persistence implementations.

## Responsibilities

- Repository implementations
- Database contexts
- SQL access
- External persistence systems

## Example Structure

```plaintext
MyApp.Data
 ??? Context
 ??? Repositories
 ??? Configurations

```
This project implements interfaces defined in `MyApp.Core`.

---

# 4. MyApp.Presentation

## Purpose

Contains the WinForms user interface.

Uses the MVP pattern.

## Responsibilities

- Forms
- View interfaces
- Presenters
- Application startup
- Dependency composition

## Example Structure

```plaintext
MyApp.Presentation
 ??? Views
 ?    ??? Interfaces
 ?    ??? Forms
 ?
 ??? Presenters
 ?
 ??? Configuration
 ?
 ??? Program.cs

```
---

# MVP Responsibilities

---

# View

The View should:

- Display UI
- Raise UI events
- Expose user input through properties

The View should NOT:

- Access database
- Contain business logic
- Perform persistence
- Contain heavy validation

---

# Presenter

The Presenter is responsible for:

- Handling UI behavior
- Listening to View events
- Calling UseCase services
- Updating the View

The Presenter should NOT:

- Access database directly
- Contain persistence logic

---

# UseCase Services

Services contain business/application logic.

Examples:

- Sample item retrieval workflow
- Validation
- Strategies

---

# DTO Philosophy

DTOs are used to isolate the UI from business entities.

## Why?

The UI often does not need:

- Internal IDs
- Audit fields
- Sensitive data
- Domain-only properties

Example:

```plaintext
View
 ?
SampleItemDto
 ?
UseCase
 ?
Entity
 ?
Repository

```
---

# Dependency Rules

Dependencies always point inward.

```plaintext
Presentation ? UseCases ? Core
Data ? Core

```
Forbidden dependencies:

- Core ? anything
- UseCases ? Data
- Presentation ? Data directly

---

# Dependency Injection

This template supports:

- Manual composition
- Microsoft.Extensions.DependencyInjection

Because WinForms is View-first, Presenters are usually created inside Forms.

Example:

```csharp
public partial class SampleItemForm : Form
{
    private readonly SampleItemPresenter _presenter;

    public SampleItemForm(IGetSampleItemsUseCase useCase)
    {
        InitializeComponent();

        _presenter = new SampleItemPresenter(this, useCase);
    }
}

```
---

# SOLID Principles

This template follows:

## Single Responsibility Principle

Each class has one responsibility.

## Open/Closed Principle

Business rules can be extended via strategies and abstractions.

## Liskov Substitution Principle

Interfaces are properly substitutable.

## Interface Segregation Principle

Small focused interfaces are preferred.

## Dependency Inversion Principle

High-level modules depend on abstractions.

---

# Validation Strategy

Validation exists in two levels:

## UI Validation

Simple checks:

- Empty fields
- Formatting
- Required controls

## UseCase Validation

Real business validation:

- Rules
- Constraints
- Workflows

---

# Scaling Recommendations

For large applications:

- Organize by feature
- Keep Presenters small
- Split commands and queries if needed
- Use strategies for replaceable business rules
- Avoid massive service classes

---

# Recommended Future Enhancements

Possible improvements:

- FluentValidation
- CQRS
- MediatR
- Event Bus
- Modular architecture
- Plugin systems
- Unit Testing
- Integration Testing

---

# Goal of This Template

This template aims to provide:

- A clean WinForms architecture
- Production-level organization
- Educational value
- Scalability
- Maintainability
- Simplicity without sacrificing structure

---

# License

MIT License

Free to use, modify, and distribute.
