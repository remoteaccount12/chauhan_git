
File Structure - Separate things that change for different reasons, at different rates. Make the stable things ignorant of the unstable ones.

**The three principles**

**1 · Group by rate of change, not by topic.**

The instinct is to group by subject — "everything about the card goes in the card file." That's why actions_card.py contains a font loader, a business-model builder, a generic text layout engine, and a 318-line paint function. All "about the card." All changing for different reasons.
Group instead by why it would change.

2 · Dependencies point toward stability.

Ask which of these is more likely to change in the next two years:

- what counts as a revenue anomaly
- the Databricks SQL connector

The business rule is far more stable. So the business rule must not depend on the connector. In Prosight2 it does — anomaly_detector.py transitively reaches Databricks, OpenAI, and torch. The most stable idea in the product is chained to the three most volatile things in it.
Inverting that is what the ports do. The rule depends on nothing; the connector conforms to what the rule needs.

3 · Testability is a symptom, not the goal.

I've said "testable" a lot and that undersells it. The real property is:
▎ Code you can run in isolation is code you can understand in isolation.
If you can't call a function without a GPU and a warehouse, you also can't reason about it without holding a GPU and a warehouse in your head. Prosight2 is 28.7% testable — which is another way of saying 71% of it can only be understood by running the whole system. Tests are just the measurable proxy for "can a human hold this in their head."



domain - 
adapters - 
application - 
config - 
cli - 

    Folder    │ The question it answers │              Real example               │
├──────────────┼─────────────────────────┼─────────────────────────────────────────┤
│ domain/      │ What's the rule?        │ "outside the range → anomaly"           │
├──────────────┼─────────────────────────┼─────────────────────────────────────────┤
│ adapters/    │ How do I talk to X?     │ connecting to Databricks                │
├──────────────┼─────────────────────────┼─────────────────────────────────────────┤
│ application/ │ What order?             │ read → forecast → flag → explain → send │
├──────────────┼─────────────────────────┼─────────────────────────────────────────┤
│ config/      │ What value?             │ 0.75                                    │
├──────────────┼─────────────────────────┼─────────────────────────────────────────┤
│ cli/         │ Which pieces today?     │ real reader, or fake one