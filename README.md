# AgentGeist

## Description:
Hackathon AWS

### Track 1 — The Exorcism Protocol
The brief: 
A paranormal outbreak is spreading through a live facility simulation. Build a supervised swarm — agents with distinct roles, distinct tools, and only partial individual knowledge — that senses, shares intel, and coordinates containment in real time.

Victory belongs to the swarm, not to a single model. Containment speed, false-alarm rate, and civilians saved feed the venue scoreboard.

### Pull a tick
Ticks come from the scenario feed on your card, not from a shared proxy. Connect it as an MCP server and the swarm gets the incident as a tool call:

```
from crewai.mcp import MCPServerHTTP

scenario = MCPServerHTTP(
    url=os.environ["SCENARIO_MCP"],
    streamable=True,
    headers={},
)
# tools: hack26-get-tick, hack26-submit-containment, hack26-get-scoreboard
```

Use MCPServerHTTP, not the "https://..." string shorthand: inside a running event loop the shorthand resolves to zero tools with no error, which is an hour you will not get back.

```
Input contract, per tick
{
  "tick_id": 142,
  "facility_id": "aws-builder-loft-sf",
  "sector_reports": [
    {
      "sector": "catering_coffee_bar",
      "sensors": { "emf_milligauss": 98, "temp_c": -4, "motion_events": 17 },
      "sighting": { "entity_class": "level-5-phantom", "confidence": 0.82, "witness_count": 4 },
      "civilian_status": "4 pinned",
      "containment_state": "breached"
    }
  ],
  "swarm_budget": { "max_active_agents": 8, "api_credits_remaining": 45000 },
  "escalation_channel": { "app": "discord", "channel": "#loft-incident", "via": "one-mcp" }
}
```

Fields are fixed: do not rename sector_reports, civilian_status, or containment_state, and do not assume a tick has only one sector. swarm_budget is a budget, not decoration — exceeding max_active_agents is a scored failure.

escalation_channel.app is discord for this venue, and the channel is #loft-incident = 1564842098740695100. Posting there is how the room sees your swarm work; it is also the evidence judges look at when your demo is over.

### Report the decision
```
# call the scenario feed's tool with your team handle
decision = await scenario_tools.submit_containment(
    team="team-04", tick_id=tick.tick_id, action="evacuate"   # contain | evacuate | observe
)
```

You get a judgment back immediately: the action the tick called for, a 0–1 score, and whether it counted as a false alarm. The scoreboard updates on every submission, so you can see where you stand without asking a judge.



## Resources
- [AWS Hackathon Link](https://youdotcom-oss.github.io/halloween-hack-2026/#8-how-you-are-scored)

##### Partner onboarding links (start here):
- You.com: sign up at [link](https://you.com/home) — you get a starter credit when you sign up. If you burn through it, ask the You.com folks at the organizers' table and they'll top you up.
- CrewAI: official quickstart — [link](https://docs.crewai.com/v1.13.0/en/quickstart)
- One: event setup page — [link](https://hackathon.withone.ai/sf-halloween-2026)
- Parasail [link](https://platform.parasail.io/home)

## Requirements
- **Required:** API used
