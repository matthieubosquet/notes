# ACP algorithms

## Effective Policies

```javascript
function gatherEffectivePolicies(acr, ancestorAcrs) {
    const effectivePolicies = new Set

    // Direct access controls of own ACR.
    for (const accessControl of acr.accessControls)
        for (const policy of accessControl.appliedPolicies)
            effectivePolicies.add(policy)

    // Member access controls of ancestor ACRs.
    for (const acr of ancestorAcrs)
        for (const accessControl of acr.memberAccessControls)
            for (const policy of accessControl.appliedPolicies)
                effectivePolicies.add(policy)

    return effectivePolicies
}
```


## Granted Access Modes

```javascript
function grantAccessModes(policies, context) {
    const allowedAccessModes = new Set, deniedAccessModes = new Set

    // Gather allowed and denied access modes from satisfied policies
    for (const policy of policies)
        if (isSatisfiedPolicy(policy, context)) {
            for (const mode of policy.allowedAccessModes)
                allowedAccessModes.add(mode)

            for (const mode of policy.deniedAccessModes)
                deniedAccessModes.add(mode)
        }

    // Deny overrules allow.
    for (const mode of deniedAccessModes)
        allowedAccessModes.delete(mode)

    return allowedAccessModes
}
```


## Satisfied Policy

```javascript
function isSatisfiedPolicy(policy, context) {
    // If any 'none of' matcher is satisfied then the policy is not satisfied.
    for (const matcher of policy.noneOfMatchers)
        if (isSatisfiedMatcher(matcher, context))
            return false

    // If any 'all of' matcher is not satisfied then the policy is not satisfied.
    for (const matcher of policy.allOfMatchers)
        if (!isSatisfiedMatcher(matcher, context))
            return false

    // If any 'any of' matcher is satisfied then the policy is satisfied.
    for (const matcher of policy.anyOfMatchers)
        if (isSatisfiedMatcher(matcher, context))
            return true

    // At this point there are
    // - no satisfied 'none of' matchers,
    // - no unsatisfied 'all of' matchers and
    // - no satisfied 'any of' matchers.

    // Hence, the policy is satisfied if it has
    // - an 'all of' condition and
    // - no 'any of' condition.
    return policy.allOfMatchers.size !== 0 && policy.anyOfMatchers === 0
}
```


## Satisfied Matcher

```javascript
function isSatisfiedMatcher(matcher, context) {
    // An empty matcher is never satisfied.
    if (matcher.agents.size === 0 && matcher.clients.size === 0 && matcher.issuers.size === 0 && matcher.vcs.size === 0)
        return false

    // For each attribute, if any values are defined, then at least one of them must match the context.

    if (matcher.agents.size !== 0) {
        let isMatch = false

        for (const agent of matcher.agents)
            if (agentMatches(agent, context)) {
                isMatch = true
                break
            }

        if (!isMatch)
            return false
    }

    if (matcher.clients.size !== 0) {
        let isMatch = false

        for (const client of matcher.clients)
            if (clientMatches(client, context)) {
                isMatch = true
                break
            }

        if (!isMatch)
            return false
    }

    if (matcher.issuers.size !== 0) {
        let isMatch = false

        for (const issuer of matcher.issuers)
            if (issuerMatches(issuer, context)) {
                isMatch = true
                break
            }

        if (!isMatch)
            return false
    }

    if (matcher.vcs.size !== 0) {
        let isMatch = false

        for (const vc of matcher.vcs)
            if (vcMatches(vc, context)) {
                isMatch = true
                break
            }

        if (!isMatch)
            return false
    }

    // At this point, the matcher is satisfied because
    // - there was at least one defined attribute and
    // - at least one value of each defined attribute matched the context.
    return true
}

const publicAgent = "https://www.w3.org/ns/solid/acp#PublicAgent"
const authenticatedAgent = "https://www.w3.org/ns/solid/acp#AuthenticatedAgent"
const creatorAgent = "https://www.w3.org/ns/solid/acp#CreatorAgent"
const ownerAgent = "https://www.w3.org/ns/solid/acp#OwnerAgent"
function agentMatches(agent, context) {
    if (agent === publicAgent)
        return true

    if (agent === authenticatedAgent && context.agent !== null)
        return true

    if (agent === creatorAgent && context.creators.includes(context.agent))
        return true

    if (agent === ownerAgent && context.owners.includes(context.agent))
        return true

    if (agent === context.agent)
        return true
}

const publicClient = "https://www.w3.org/ns/solid/acp#PublicClient"
function clientMatches(client, context) {
    if (client === publicClient)
        return true

    if (client === context.client)
        return true
}

const publicIssuer = "https://www.w3.org/ns/solid/acp#PublicIssuer"
function issuerMatches(issuer, context) {
    if (issuer === publicIssuer)
        return true

    if (issuer === context.issuer)
        return true
}

function vcMatches(vc, context) {
    if (context.vcs.includes(vc))
        return true
}
```
