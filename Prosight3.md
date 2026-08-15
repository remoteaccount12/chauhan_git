
Port - A port is an interface declared by the code that needs something, in the language of what it needs — not by the code that provides it.


That inversion is the whole idea, and it's easiest to see in your own repo. Today:
evaluate.py ──imports──► data_loader.py ──imports──► DBClient ──► databricks-sql-connector
