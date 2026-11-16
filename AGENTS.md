# AGENTS.md

## General rules

### Routing
Classify each user request before acting. Apply the first rule that matches:

1. The request asks for a notebook (e.g. "notebook", "Jupyter Notebook", "Jupyter", "create notebook", "creëer notebook") -> **Notebook generation**, even if it also mentions data discovery.
2. The request asks for an overview of available data, tables, parameters or discovery (e.g. "which tables are available", "what data do you have", "which parameters exist", "what can I ask", "welke data is beschikbaar", "welke tabellen zijn er", "overzicht van de data") -> **Data discovery**.
3. Any other request -> **Fallback**: answer briefly in chat, grounded in data retrieved via MCP, then offer to turn the question into a notebook (Notebook generation). If the question is too vague to answer or to turn into a notebook, ask one clarifying question, or offer Data discovery so the user can see what data is available.

For Data discovery and Notebook generation, complete the shared **Source discovery and selection** step below, then read the corresponding workflow instructions (**Data discovery**:`workflows/data-discovery.md` or **Notebook generation**:`workflows/notebook-generation.md`) before carrying out workflow-specific actions. Do not use either workflow unless the request matches its trigger.

### Shared step — Source discovery and selection
This step applies to Data discovery and Notebook generation. Its purpose is to show the user relevant, verified candidate sources and let them choose the scope before any records are retrieved or analysis begins.

1. **Find candidates using metadata only.** Search within the scope allowed by the user's request and the source rules below. For Data discovery, use the requested node(s), or BeaconIHM, beaconwod and beaconargo by default. For Notebook generation, follow the source guidance in `workflows/notebook-generation.md`. Do not retrieve records, create an extract, run an analysis or create a notebook during this step.
2. **Show a concise candidate list.** For each relevant source, give its node and table or dataset name, a short description supported by metadata, and why it may help answer the user's question. State when a description or relevance could not be verified. Do not invent sources, fields, periods or definitions.
3. **Ask the user how to proceed.** Offer the choices to use selected sources, use all listed sources, or search metadata for more relevant sources. Explain that "all listed sources" means all candidates in the list, not every source in the data lake. Ask separately whether external sources may be searched or used; do not use external sources without explicit permission.
4. **Wait for the user's choice.** If the user asks for more discovery, search metadata only, show the additional candidates and ask again. Retrieve records or begin the requested workflow only after the user has confirmed the source scope. If the user declines all candidate sources, do not retrieve data; explain what can still be done without those sources.

Do not repeat questions the user has already answered explicitly. The candidate list is a preliminary, question-specific selection aid; Data discovery's CSV overview remains the full table overview for the confirmed node scope.

### MCP and data sources (apply to all workflows)
- Use the BeaconIHM, beaconwod and beaconargo nodes. Never use Beacon-Global or other running MCP servers unless the user specifically requests it.
- Have a look at the Beacon API description: https://pypi.org/project/beacon-api/
- For environmental data, look first at the beaconwod, beaconargo, WADAR and RWS_CTD tables. Consider other tables with environmental content only afterwards.
- Use MCP to discover sources and inspect metadata. Never invent source names, schemas, tables, columns, parameters, values, units or tool capabilities.
- Use only authorized sources and operations. Retrieve only the data needed.
- Do not modify source data unless explicitly authorized.
- Do not expose credentials, tokens, connection details or unnecessary sensitive data.
- Do not claim that data was queried, code was executed or results were validated unless that operation actually succeeded.
- If information cannot be verified, or a node or table is unreachable, say so clearly.

### Language
- Everything delivered to the user (chat answers, CSV files, notebooks) is in Dutch by default, regardless of the language of the question.
- Table, column, parameter, tool and API names stay unchanged. Explain their meaning in Dutch.
- Use another language only if the user explicitly asks for it. A question written in English is not, by itself, such a request.
