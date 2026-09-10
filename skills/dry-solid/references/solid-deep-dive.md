# SOLID Deep Dive for PHP

## Single Responsibility (SRP)

**Smell**: A class mixes business rules, persistence, formatting, and HTTP concerns.

**Fix examples**:
- Move business logic out of Controllers into Actions or Domain Services.
- Extract Query Objects or Repositories for data access.
- Use dedicated Value Objects or DTOs instead of arrays + transformation logic scattered everywhere.

## Open/Closed (OCP)

**Smell**: Long `if/else` or `match` that grows every time a new payment method, notification channel, or export format is added.

**Fix**:
```php
interface ExporterInterface
{
    public function export(array $data): string;
}

class PdfExporter implements ExporterInterface { ... }
class CsvExporter implements ExporterInterface { ... }

// New format = new class. No modification of existing code.
```

## Liskov Substitution (LSP)

**Smell**: Subclass throws or silently does nothing for methods defined in the parent.

**Fix**: Prefer interfaces + composition. If a class cannot honor the full contract, it should not extend that base.

## Interface Segregation (ISP)

**Smell**: Fat interfaces that force empty implementations.

**Fix**: Split into smaller interfaces (`CanExport`, `CanImport`, `Loggable`, etc.). A class implements only what it needs.

## Dependency Inversion (DIP)

**Smell**:
```php
// Bad
class InvoiceService
{
    public function generate()
    {
        $pdf = new Dompdf(); // concrete + hard to test
    }
}
```

**Good**:
```php
class InvoiceService
{
    public function __construct(
        private readonly PdfGeneratorInterface $pdf
    ) {}
}
```

Bind the concrete implementation in the service container.

## Relationship to DRY in PHP

- Extracting duplicated logic often reveals the missing abstraction (OCP + DIP).
- Traits are useful for DRY but can become a dumping ground — keep them focused.
- Constructor injection + interfaces makes the extracted reusable unit easy to share and test.
- Prefer small Action/Service classes over “God” helpers.
