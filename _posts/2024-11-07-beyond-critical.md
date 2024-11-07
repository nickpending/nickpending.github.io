---
layout: post
title: "Beyond Critical: Why Vulnerability Management is Failing Security Teams"
date: 2024-11-07
category: [security]
tags: [security, cybersecurity, vulnerability-management, cvss, epss, vulnerability-scanning, bugbounty]
---
I've come to recognize two distinct mindsets in security: those grounded in technical reality, and those I call the "boogeyman crew." The latter operates on fear rather than facts, treating every scanner-flagged "CRITICAL!" finding as an equally urgent disaster. Their world is populated by various specters: the compliance boogeyman who'll condemn them for not addressing every high-severity CVE regardless of context, the dreaded auditor who'll condemn them for not following frameworks to the letter and, of course, the truly bad actors lying in wait.

Reality demands more nuance. Yes, threat actors are actively scanning for and exploiting vulnerabilities - that's not the boogeyman, that's just another Tuesday. But crucially, they can only exploit vulnerabilities where specific conditions are met. We're not just beholden to tools and scoring frameworks - we're their hostages. Compliance demands, audit requirements, and the specter of breaches force us to dance to their tune, while the tools themselves fail to provide the ammunition we need to push back against unreasonable demands. We're caught in an impossible position: forced to act on "critical" findings without the meaningful information needed to defend rational prioritization decisions.

The security industry has a persistent blind spot in vulnerability management: our fixation on "critical" vulnerabilities without understanding their actual exploitability. This disconnect between theoretical severity and practical exploitability creates significant operational challenges for security teams. We're held captive by tools and scoring frameworks that leave us desperately short of the evidence needed to justify focusing on what truly matters.

## The Reality Gap

Current vulnerability scanning approaches rely predominantly on basic detection methods that fall far short of actual exploitation verification. Common techniques include:

- Banner grabbing and version fingerprinting
- CPE matching and package enumeration
- Default credential checking
- Known file path detection
- Basic port and service enumeration
- Hash matching against known vulnerable versions
- Pattern matching against known security misconfigurations

Most of these checks run in unauthenticated contexts, without access to internal system state or configuration details. Even authenticated scans often just compare version numbers against vulnerability databases, ignoring crucial runtime configurations, compile-time options, and environmental dependencies.

These rudimentary techniques masquerade as sophisticated analysis through complicated scoring algorithms and weighted risk factors. But at their core, they're still just matching strings and version numbers. A scanner might flag a "critical" Spring4Shell vulnerability because it detected a Java application running Spring, without confirming if the application actually uses the vulnerable code paths or required configurations.

The result? An overwhelming volume of alerts that creates a fundamental contradiction - when everything is critical, nothing truly is. Security teams end up drowning in findings that lack the context needed for meaningful prioritization.

Consider a typical scenario: Your vulnerability scanner identifies 100 "critical" CVEs across your environment. Resources allow for remediating 20 this sprint. The pressing question becomes: which vulnerabilities actually present the most immediate risk? Our current frameworks - CVSS and EPSS - while valuable, don't provide the full context needed to make this decision effectively.

## Understanding Framework Limitations

### CVSS: Structural Blindspots
CVSS provides a structured way to think about vulnerability characteristics. However, by focusing solely on theoretical scoring criteria, it misses the concrete dependencies that determine real-world exploitability. The framework measures potential impact without considering whether the conditions for that impact can actually materialize.

### EPSS: The Probability Puzzle
EPSS represents an evolution in vulnerability prioritization, but contains a crucial limitation: it predicts the likelihood of exploitation attempts rather than exploitation success. This distinction matters significantly when prioritizing remediation efforts. A high EPSS score indicates probable exploitation attempts but says nothing about your environment's actual susceptibility.

Let's examine CVE-2021-40438 (Apache HTTP Server) as an illustrative example. Its EPSS score of 0.96 indicates near-certainty of exploitation attempts. However, successful exploitation requires specific preconditions:

1. mod_proxy must be enabled
2. A specific reverse proxy configuration must be present

Without these conditions, the vulnerability remains unexploitable - yet this crucial context isn't captured in our scoring systems.

## The Exploitation Dependency Challenge

When we ignore dependencies in vulnerability assessment, we're essentially:
- Misallocating resources by remediating unexploitable vulnerabilities
- Creating alert fatigue among security teams
- Missing opportunities for implementing simpler compensating controls
- Failing to effectively communicate risk to stakeholders

## Beyond Current Frameworks

The fundamental issue with both CVSS and EPSS isn't that they're wrong - it's that they're incomplete. CVSS provides severity metrics without exploitation context. EPSS offers probability insights about exploit attempts but can't tell you anything about your specific environment's susceptibility to those attempts.

This gap exists because:
1. Dependency analysis requires deep technical understanding of exploitation mechanics
2. Environmental conditions vary significantly across organizations
3. Documentation of exploitation prerequisites is often scattered or incomplete
4. Current scanning approaches prioritize breadth over depth, leading to high false positive rates

## The Information Paradox

Here's something that should raise eyebrows: when CVEs are first disclosed, the submitters typically know exactly what conditions are required for exploitation. This information exists. They documented it. They verified it. Yet somehow, mysteriously, these crucial details often vanish from the broader vulnerability write-ups and advisories that propagate through the industry.

This begs an uncomfortable question: why does critical exploitation context consistently get lost in translation? One might cynically observe that uncertainty and inefficiency in vulnerability management have proven remarkably profitable for certain segments of the security industry. Fear of the unknown drives spending, and precise exploitation requirements might make threats appear more manageable.

But let's consider a more constructive perspective: if we could consistently capture and share the exploitation prerequisites that CVE submitters already document, we could:
- Dramatically reduce false positives
- Accelerate accurate triage
- Enable more targeted remediation
- Lower operational costs
- Reduce team burnout

The information exists at the source. We're just not propagating it effectively through our security ecosystems.

## Implementation Strategy

For organizations looking to enhance their vulnerability management approach:

1. Focus on high-value systems and critical business assets first
2. Document exploitation prerequisites for your high-priority vulnerabilities
3. Build a knowledge base of common dependencies
4. Make dependency checking part of your triage process
5. Share findings with the broader security community

## Breaking the Cycle

The security industry needs to acknowledge that improving vulnerability context isn't just a technical challenge - it's also a matter of incentives and information flow. We should:

1. Demand better preservation of exploitation prerequisites from initial CVE documentation
2. Create standardized formats for sharing dependency information
3. Build tools that leverage this context rather than just severity scores
4. Reward clarity over complexity in vulnerability reporting

## Conclusion

Years of hands-on experience in exploitation research and defense have demonstrated that context defines exploitability. The evolution of vulnerability management requires moving beyond numeric scores to understanding exploitability in specific environments.

The future of vulnerability management lies not in generating more alerts or creating new scoring systems, but in providing better context for the ones we have. This shift won't happen overnight, but it's essential for scaling security operations in an increasingly complex digital landscape.

## Key References

- FIRST.org CVSS v3.1 Specification: https://www.first.org/cvss/v3.1/specification-document
- EPSS Documentation and Data: https://www.first.org/epss/
- Apache HTTP Server CVE-2021-40438: https://httpd.apache.org/security/vulnerabilities_24.html
