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
DatabricksReader doesn't import anything from application/. The only place that knows about both is cli.py, the composition root. Swap for Snowflake and only cli.py changes.