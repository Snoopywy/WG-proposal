# FG-TIDA Working Group Proposal: Authorization and Privilege Management for AI Agents (APMA)

## Charter Information

|||
|-|-|
|Charter version|0.2|
|Date|2026-11-19|
|Status|Draft, initial proposal for discussion|

## 1\. Group Information

|Field|Content|
|-|-|
|Working group name and acronym|Authorization and Privilege Management for AI Agents (APMA), working title|
|Basis|Theme #23 "Security framework and requirements for authorization and privilege lifecycle management of AI agents", presented at the FG-TIDA preparation meeting of 30 September 2026|
|FG-TIDA theme(s) covered|Theme #23, with coordination and dependencies on themes #4, #5, #6, #22, #36 and related themes (see Section 7)|
|Chair(s) / co-chairs|TBD|
|Editor(s) of the main deliverables|TBD|
|Theme proposers|Ye Tao, Ying Wang (China Unicom)|
|Mailing list|TBD|
|Repository and issue tracker|TBD|
|Working methods|Consensus as in the FG-TIDA Terms of Reference, co-chairs, empowered editors, and workshops or webinars where useful|
|Charter status and version|Draft 0.2|

## 2\. Background and Problem Statement

AI agents act on behalf of users, services or organizations, and their privileges are not static: they are created and bound at initialization, adjusted as a task progresses, suspended and isolated during hibernation, re-validated and restored upon activation, and fully revoked and recovered when the task completes or the agent terminates. This sequence of state transitions is the privilege lifecycle.

Authorization and privilege lifecycle management are tightly coupled: authorization precedes and determines the granting of privileges, and privilege lifecycle management governs the states those granted privileges pass through, from grant to revocation.

Before a service lets an agent act, it needs to know what the agent is permitted to do, for how long, and under what conditions, and to withdraw that permission when the conditions change or the task ends. Today this is decided piecemeal: by identity checks that say little about behaviour, by access-control rules written for conventional software, by tokens that are not bound to a task's scope or duration, and by revocations that take effect only where an enforcement point checks again. None of these address the two questions at the heart of AI Agent.

AI Agent poses the following challenges for authorization and privilege management, grouped into two core questions.

* How to authorize. An agent is self-determining: it decides what is "relevant", its context and risk change at runtime, and its actions can drift from the purpose for which it was granted authority. How should an authorization decision be made so that it reflects the agent's actual context, purpose and risk, stays valid only as long as those hold, and can be checked and reproduced by the parties that rely on it? Meanwhile, multi-level authorization is also one of the recurring and under-specified cases.
* How to manage privileges. An agent's privileges pass through many states over one task - granted, elevated, restricted, hibernated, reactivated, revoked - and across agents and trust domains. How should those state transitions be governed so that an agent holds only the privileges it needs, precisely when it needs them, and loses them completely when the task ends?

Existing work addresses agent identity (ITU-T X.1285 series, X.AIA-dii-req, X.dpidm-aAI), access control and enforcement (themes #4 and #6, joined in #36), provenance of authority (themes #5 and #15) and remote attestation (theme #22). None of it specifies a security framework and requirements for authorization and privilege lifecycle management of AI agents.

This working group therefore pursues two directions: how to authorize AI agents, and how to manage their privileges over the privilege lifecycle.

The unit of analysis is the privilege an agent holds over a task window, together with the authorization decisions that grant, adjust and revoke it. It is complementary to the enforcement decision for a single action studied by WG2 (IBEA): WG2 decides one action at the moment it is attempted, while this working group manages the privileges that persist across the actions of a task.

## 3\. Goals

The Working Group enables secure authorization and privilege lifecycle management for AI agents whose context and privileges change during their execution.

Its pre-standardization work builds on existing specifications toward open, interoperable and auditable authorization and privilege management, so that services and users can decide whether, when, and to what extent to let an agent act, independent of where the agent runs.

The Working Group describes how to authorize, how to delegate authorization across agents, and how to manage privileges, and maps existing building blocks and remaining gaps (Deliverable 1).

* How to authorize: The Working Group specifies requirements for the authorization of AI agents - how an agent's authority is granted, under what conditions and for how long, and how the decision is rendered, enforced, bound to the agent's purpose and context, and audited (Deliverable 2).
* How to multi-level authorization: The Working Group analyses multi-level authorization - how a principal's authority is attenuated, bound to each agent's identity and key, and selectively disclosed across agents (Deliverable 3).
* How to manage privileges: The Working Group specifies requirements for the privilege lifecycle of AI agents - how privileges are initialized, changed, hibernated, activated, and revoked and recovered, with full-lifecycle governance (Deliverable 4).

The Working Group will build on previous work and the state of the art, including the ITU-T X.1250 series, X.dac-aa, OAuth 2.0, GNAP, and IETF RATS and WIMSE, adding further work as it starts.

## 4\. Scope and Out of Scope

### In Scope

* The threat model, terminology, use cases and gap analysis for the authorization plane and the privilege lifecycle plane of AI agents.
* Requirements for the authorization of AI agents: how an agent's authority is granted, under what conditions and for how long, and how the decision is rendered, enforced and audited.
* Requirements for multi-level authorization: monotonic permission attenuation, binding of grants to agent identity and key, and selective disclosure of authorization information across agents.
* Requirements for the privilege lifecycle: initialization, change, hibernation, activation, revocation and recovery, and full-lifecycle governance.

### Out of Scope

* The working group does not define encodings, schemas, wire formats or protocols.
* It does not address identity issuance and lifecycle in general (ITU-T X.1285 series).
* It does not address remote attestation (WG1).
* It does not address the enforcement mechanics and single-action decision outcomes at an enforcement point (WG2).

### Distinction from WG2 (IBEA)

| Dimension | This working group (APMA) | WG2 (IBEA) |
|-|-|-|
| Privilege granularity | The privilege set of a complex task | A single action |
| Time span | A task window (possibly very short, spanning steps) | The instant an action is attempted |
| Authority source | The granting authority, delegation chain and authorizing entity | Policy taken as given (authored under themes #5) |
| Object of grant | The privilege boundary (which resources, actions, data) | No scope definition; checks one action against policy |
| Statefulness | Stateful: a privilege state machine with transitions | Stateless: each decision is independent |
| Temporal orientation | Grants before action, for future steps | Checks at the moment of action |
| Object of study | A standing capability ("may do X for window W") | An event ("is this action permitted now") |
| Delegation/propagation | Owns the propagation graph, depth limit and cascading revocation | Treats delegation chains as a series of decisions |

The most fundamental distinctions are statefulness and the standing-capability-versus-event nature of the object. This working group supplies the current privilege state and authorization decision as an input to WG2's enforcement decision; WG2's outcome (block, escalate) can trigger a privilege lifecycle transition (restrict, revoke) in this working group's model. WG1 (CDRA) supplies attestation results as another input to authorization decisions.

## 5\. Programme of Work and Deliverables

|#|Deliverable|Short description|Input documents|
|-|-|-|-|
|1|Problem statement, threats, terminology, use cases and gap analysis (Technical Report)|Threat model for the authorization plane and the privilege lifecycle plane, terminology, use cases, and a gap analysis against the ITU-T X.1250 series, X.dac-aa, OAuth 2.0, GNAP, UMA 2.0 and ISO/IEC 29146.|Deliverable 1|
|2|Security framework and requirements for authorization of AI agents (Technical Specification)|How to authorize AI agents: the authorization decision model — how an agent's authorization is granted, rendered, enforced and audited — and the security requirements on authorization, stated as mandatory and recommended.|Deliverable 2|
|3|Multi-level authorization of AI agents (Technical Report)|How to multi-level authorization across agents: use cases, requirements for monotonic permission attenuation, binding of grants to agent identity and key, and selective disclosure of authorization information, a reference model, and a gap analysis.|Deliverable 3|
|4|Security requirements for privilege lifecycle management of AI agents (Technical Specification)|How to manage privileges: the privilege lifecycle — initialization, change, hibernation, activation, and revocation and recovery — and full-lifecycle governance, stated as mandatory and recommended.|Deliverable 4|

## 6\. Milestones

|Target date|Milestone|
|-|-|
|2026-12 (Paris)|Charter agreed and outline of Deliverable 1 proposed as working draft.|
|2027-01 (Geneva)|First draft of Deliverable 1 and outlines of Deliverables 2, 3 and 4.|
|TBD (3rd FG-TIDA meeting)|Deliverable 1 agreed by the working group; first drafts of Deliverables 2, 3 and 4.|
|TBD (within the FG-TIDA lifetime)|Deliverables 1 to 4 submitted to the FG-TIDA plenary in time for SG17.|

## 7\. Coordination, Dependencies and Liaisons

|Group / Theme|Coordination or dependency|
|-|-|
|Theme #23|The basis of this working group.|
|WG2 (IBEA), themes #36, #4, #6|Enforcement. This working group supplies the current privilege state and authorization decision as an input to the enforcement decision for a single action; WG2's outcome can trigger a privilege lifecycle transition. Intent-based authorization coordinates with intent-based policy expression (#6).|
|WG1 (CDRA), theme #22|Remote attestation. Attestation results are one input to authorization decisions.|
|Themes #5, #15, #9|Provenance of authority. This working group takes the authored authority and its delegation chain as input.|
|Themes #42, #43|Evidence grading. This working group specifies the authorization object (issuer, scope, duration); #42/#43 specify how its validity is graded by evidence, kept apart from evidence quality.|
|Theme #19|Lifecycle-wide privacy of authorization proof and auditability.|
|Theme #31|Preservation of authority boundaries across agent memory and context handoffs; closely related to inter-agent privilege propagation.|
|Theme #16|Human oversight control points that authorization's human-approval dimension consumes.|
|Theme #26|Cryptographic runtime enforcement boundary, a zero-trust building block.|
|Theme #33|Re-evaluation of governed decisions, including privilege grants, when evidence arrives late.|
|Theme #29|Governed connection decisions across organizational boundaries, relevant to cross-domain authorization.|
|Themes #1, #46|Accountability and attribution: the audit record this working group produces is reconstructed after the fact.|
|Themes #45, #34, #25|Delegation and identity anchoring; coordinates with inter-agent privilege propagation.|
|FG-TIDA MLDA proposal|Multi-level authorization. Adopted as Deliverable 3: monotonic permission attenuation, binding of grants to agent identity and key, and selective disclosure, with the corresponding gap analysis as input.|
|ITU-T SG17 Q10/17|X.1250 series (identity baseline).|
|ITU-T SG17 Q7/17, Q16/17|X.S-AIA, X.sr-taimas, XSTR.ltf-AAI (AI agent security).|
|IETF|OAuth 2.0 (RFC 6749), Token Exchange (RFC 8693), GNAP (RFC 9635), identity chaining, transaction tokens, RATS/EAT, WIMSE.|
|OASIS/Kantara|XACML, UMA 2.0.|
|ISO/IEC, ETSI, NIST, IEEE|ISO/IEC 29146, 42001, 23894, 22989; ETSI SAI 001/005; NIST AI RMF, SP 800-162; IEEE 7001, 7009, 802.1AR.|

## 8\. Terminology

The working group will use the common FG-TIDA terminology where applicable. New terms will be communicated to the FG-TIDA terminology and use cases working group.

|Term|Meaning|
|-|-|
|Authorization|An approval granted to a system entity to access a system resource; here, the decision of whether, when, and to what extent an agent may exercise its privileges.|
|Privilege|A right that, when granted to an entity, permits the entity to perform an action.|
|Privilege lifecycle|The sequence of state transitions a set of privileges undergoes, from creation through modification, hibernation, activation, and revocation and recovery.|
|Privilege hibernation|A state in which an agent's privileges are suspended and isolated, not exercisable until a validated reactivation.|
|Privilege activation|Restoration of hibernated privileges after mandatory re-assessment against the current authorization context.|
|Privilege revocation and recovery|Permanent, cascading removal of privileges, verified across all affected systems.|

## 9\. Working Methods

Each deliverable will be tracked in its own issue in the working group repository. Contributions folded into a deliverable will remain attributed at clause level. Virtual interim calls will be held between plenary meetings.

## 10\. Security and Privacy Considerations

Authorization decisions, privilege states and audit records can disclose sensitive information about agents and the people they act for. All deliverables address what is disclosed to which party, how linkable agents and people become, and what a party learns from a denied or revoked decision. The enforcement and audit surfaces are themselves attack surfaces, including for injected instructions. A suppressed decision must be distinguishable from the absence of any check.

## 11\. Success Criteria and Conclusion

The chartered work of the working group ends when all three deliverables are close to completion and the FG-TIDA plenary approves them by consensus. FG-TIDA then dispatches the deliverables to SG17, its parent group, and through liaison statements to other standards bodies and industry consortia, such as the IETF, OASIS, ISO/IEC, ETSI, NIST and IEEE. SG17 and these bodies can complete the standardization.

## 12\. Revision History

|Version|Date|Change|
|-|-|-|
|0.1|2026-11-09|First draft of the charter|
|0.2|2026-11-19|Add multi-level authorization|
