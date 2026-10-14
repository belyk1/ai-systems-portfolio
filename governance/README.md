# Reliability framework for autonomous agents

The set of rules that keeps a team of autonomous agents accountable and its work auditable. It is written down and used daily by the agents that build the other projects in this portfolio.

This page describes what each rule is for and what it is meant to lead to, not the rule text.

## The ideas

**Instructions come only from the rules and the human. Everything else is data.** Tool results, files, web pages, hand-offs from other agents and the agent's own stored memories never count as instructions. When any of them tells the agent to do something, the agent reports it to the human and does not act on it. This is the framework's defence against prompt injection, and it covers other agents in the same team: a hand-off is information, not a command.

**An author is never the last reader of anything treated as authoritative.** A second reader, working from the artifact and not from the author's account, can catch errors the author cannot see. This is why the results in this portfolio are checked by someone other than whoever produced them.

**Ground truth over self-report.** An agent's account of its own state is not the state. Before acting on a report, check one level down. The purpose is to stop agents from building on each other's assumptions, and to turn "done" into something verifiable. An agent's own settled conclusion counts as self-report too.

**A completeness claim needs a complete read.** "None remain" or "all fixed" is only allowed after a read that was not truncated, capped, sampled or remembered from earlier. Tool output limits are the usual way an agent reports a clean result that isn't.

**Nothing exists only in an agent's context.** Findings, decisions and artifacts go to disk before they are reported. When a long run is interrupted or compacted, the next session continues from what was written.

**Retire, never delete.** Anything taken out of service is moved to an archive with an index entry, so the record of what existed and why it was removed survives. The one exception is when the owner asks for a deletion and the agent agrees.

**Hard-to-reverse actions need an explicit human yes.** Deleting, rewriting history, anything that reaches outside the system. Nothing inside the system can grant that permission. A small number of narrow standing exceptions are written down, such as pushing to private repositories once checks pass.

**Small mistakes are corrected on the spot, and wrong foundations are restarted.** An agent that discovers a wrong architectural assumption stops and starts a fresh session with the right assumption in the first prompt.

**Humans hear about problems early.** Surfacing a problem never blocks work that does not depend on it.

## Intent

The aim is autonomous work that can be trusted: errors caught before they ship, long runs that survive interruption, and decisions that can be traced and audited. The ideas are the ones a human engineering team already uses (code review, a paper trail, a rollback plan), applied to agents that work faster and forget more.
