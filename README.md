# Google Maps MCP Agent

A Gemini-powered AI agent that connects to Google Maps in real time 
using Model Context Protocol (MCP) — built to explore how AI agents 
reason across live tools, not just retrieve from training data.

---

## What this project is

This project is part of my hands-on exploration of agentic AI and 
MCP tooling. I built a Gemini AI agent that connects to Google Maps 
as a live tool using Google's Agent Development Kit (ADK) and the 
MCP standard — then tested it with real queries to understand where 
agent-based reasoning outperforms a standard Maps search.

---

## What is MCP?

Model Context Protocol (MCP) is an open standard that lets AI models 
connect to external tools, APIs, and data sources in a standardised 
way. Think of it like USB-C for AI agents — instead of every tool 
needing its own custom integration with every model, MCP provides one 
standard that any compliant agent can use.

Without MCP: every tool needs its own custom integration.  
With MCP: build the connection once, any AI agent can use it.

---

## What I built

A Gemini-based AI agent (maps_assistant_agent) connected to the 
Google Maps MCP server, giving it access to seven live Maps tools:

- maps_geocode
- maps_reverse_geocode
- maps_search_places
- maps_place_details
- maps_distance_matrix
- maps_elevation
- maps_directions

The agent can call multiple tools within a single query, chain the 
results together, and reason across them to give a complete answer —
something a standard Maps search cannot do.

![Agent tool graph](screenshot_of_agent_graph.png) 

---

## Why this is different from a normal Google Maps search

A standard Maps search answers one question at a time. You do the 
sequencing, planning, and reasoning yourself.

An agent with Maps access via MCP takes a goal — not a query — and 
chains multiple tool calls to reason toward an answer. The difference 
shows up clearly in complex, multi-constraint situations.

| Normal Maps search | Agent with Maps via MCP |
|---|---|
| One query, one result | One goal, multiple tool calls |
| You sequence stops manually | Agent figures out optimal order |
| Separate searches for each need | Chains geocoding, search, routing together |
| No time-constraint reasoning | Factors in time budgets and finish times |

---

## Example queries that show multi-tool reasoning

These are the kinds of queries where the agent outperforms a direct 
Maps search:

**Multi-stop day planning**
> "I have three client meetings in Mumbai — BKC, Lower Parel, and 
> Nariman Point. Starting from Bandra at 9am, spending 90 minutes at 
> each. What order minimises travel and when do I finish?"

**Time-constrained itinerary**
> "I have 4 hours in a new city. Suggest two landmarks, a lunch spot, 
> and a coffee place — keep it in the same area and give me a timed 
> plan."

**Equidistant meeting point**
> "Four people are meeting — one from Gurgaon, one from Noida, one 
> from South Delhi, one from Dwarka. Find a well-rated restaurant 
> roughly equidistant for all four."

**Airport layover planning**
> "I have a 4-hour layover at Delhi airport. What can I realistically 
> go see outside, factoring in time to get back through security?"

---

## Tech stack

- Google Gemini (gemini-3.5-flash) via Vertex AI
- Google Agent Development Kit (ADK)
- Model Context Protocol (MCP)
- Google Maps MCP Server
- Python 3.12
- Google Cloud Shell

---

## Setup

**Prerequisites**
- Google Cloud account with Vertex AI enabled
- Google Maps API key (Directions API + Routes API enabled)
- Node.js (for running the Maps MCP server via npx)
- Python 3.12+

**Install dependencies**
```bash
pip install google-adk==1.22.1 -r requirements.txt
```

**Configure environment**
Copy `.env.example` to `.env` in the `google_maps_mcp_agent` folder 
and fill in your values:
```bash
cp .env.example google_maps_mcp_agent/.env
```

**Run the agent**
```bash
cd adk_mcp_tools
adk web
```

Open the ADK Dev UI at `http://localhost:8000` and select 
`google_maps_mcp_agent`.

---

## Key learnings

**Agents treat tools differently from search**
The shift from "give me directions" to "plan my day given these 
constraints" is only possible when the AI can reason across multiple 
tool calls. MCP is what makes that possible at scale.

**Prompt quality determines output quality**
Getting useful multi-step responses required goal-oriented prompts, 
not search-style queries. The more context and constraints given, the 
more useful the output.

**This changes the PM prototyping workflow**
Connecting an agent to live data sources opens up a new category of 
prototype — not a mockup, not a static demo, but something that 
reasons against real-world data. The gap between idea and testable 
concept gets significantly shorter.

---

## Related post

[LinkedIn post about this project](#) — what I built, what I learned, 
and what it means for how PMs think about AI tooling.
