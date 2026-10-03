# Abstract Factory

> **Wall Note / A4**
>
> **Intent:** create a **compatible family** of related products. **Strength:** adding a new family is easy. **Weakness:** adding a new product role changes every factory.

## Detailed Notes

Abstract Factory matters when several created objects must vary **together** and remain compatible.

### Example — payment provider family

```java
interface PaymentFamily {
    PaymentClient payments();
    RefundClient refunds();
    WebhookVerifier webhooks();
}

final class AcmePaymentFamily implements PaymentFamily {
    public PaymentClient payments() { return new AcmePaymentClient(/*...*/); }
    public RefundClient refunds() { return new AcmeRefundClient(/*...*/); }
    public WebhookVerifier webhooks() { return new AcmeWebhookVerifier(/*...*/); }
}
```

A Stripe-like family and an Acme-like family can be swapped without mixing a payment client from one provider with a webhook verifier from another.

### What it protects
The pattern preserves a **family invariant**. That is its distinguishing value.

### Spring assembly
A Spring application may instead bind one coherent provider configuration using profiles, conditional beans, or configuration classes. Prefer that when the container can express the family clearly.

### When to use
- platform-specific UI/runtime families;
- payment/provider SDK families;
- production vs simulation/test families where compatibility matters;
- protocol-version families.

### When not to use
- only one product varies;
- products are unrelated;
- the factory becomes a giant container/service locator;
- adding product roles is frequent.

### Trade-offs
Easy family replacement; difficult product-axis extension. More interfaces can improve boundary clarity but add ceremony.

### Failure modes
- `getService(Class<T>)` generic locator disguised as factory.
- Downcasting to concrete family types.
- One “factory” with 30 unrelated products.
- Shared credentials/config duplicated inconsistently across products.

## Senior Questions / Exercises
1. Why is Abstract Factory good for new families but bad for new product roles?
2. Compare provider `@Configuration` classes with a hand-written Abstract Factory.
3. Model payment, refund, and webhook clients so provider families cannot be accidentally mixed.
4. What test proves family compatibility?

## Related Topics
- [Factory Method](./factory-method.md)
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [Adapter](../02-structural/adapter.md)
