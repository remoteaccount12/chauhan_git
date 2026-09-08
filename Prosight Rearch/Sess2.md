### Ports and protocol
Ports - That place — where two genuinely different implementations can be swapped without the caller noticing — is a **port**, or a **seam**. 
"the five walls where external stuff plugs in"
A port is one of these five seams where the world touches your code. (define better)

Protocol = "a wall socket", not "a contract you sign"
A Protocol is Python's name for a type that says: "if you have this method (with this shape), you count as me." No inheritance needed.

Why "Protocol" instead of inheritance?
With inheritance (the old way):
class DatabricksReader(DailyReader):  # ← must say this
    def read(self, on):
        ...
DatabricksReader now imports DailyReader from application/ports.py, so adapters/ depends on application/. Swap Databricks for Snowflake and you might have to touch the application layer. Not great.
With Protocol (Prosight4's way):
class DatabricksReader:  # ← no mention of DailyReader
    def read(self, on):
        ...

TLDR
- Port = a seam where adapters plug in (the five places external stuff touches the pipeline)
- Protocol = how you declare the shape of that seam, without forcing adapters to know about the application layer
- Wall socket = the metaphor: the pipeline plugs into it without knowing what's on the other side
****



### The dependency DAG
                            cli.py
                    the composition root — the ONLY
                   module that imports both sides, and
                    the only one that knows fakes exist
                         ╱                    ╲
                        ╱                      ╲
                       ▼                        ▼
              adapters/                      application/
          fakes/reader.py                  ports.py     (the Protocols)
          fakes/forecaster.py              pipeline.py  (the order of steps)
          fakes/sender.py                  render.py    (report -> text)
                       │                        │
                       │                        ▼
                       │                     domain/
                       │                   types.py     (the nouns)
                       └──────────────────► anomaly.py  (the rules)
                            imports domain TYPES only,
                            never the application layer

              ┌──────────────────────────────────────────────┐
              │  An arrow may point INWARD, toward domain.   │
              │  No arrow ever points outward.               │
              └──────────────────────────────────────────────┘

