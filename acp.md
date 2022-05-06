# ACP algorithms

## Granted Access Modes

```javascript
function grantAccessModes(policies, context) {
    const allow = new Set, deny = new Set

    // Gather allowed and denied access modes from satisfied policies
    for (const policy of policies)
        if (isSatisfiedPolicy(policy, context)) {
            for (const mode of policy.allow)
                allow.add(mode)

            for (const mode of policy.deny)
                deny.add(mode)
        }

    // Deny overrules allow.
    for (const mode of deny)
        allow.delete(mode)

    return allow
}
```


## Satisfied Policy


```javascript
function isSatisfiedPolicy(policy, context) {
    // If any 'none of' matcher is satisfied then the policy is not satisfied.
    for (const matcher of policy.noneOf)
        if (isSatisfiedMatcher(matcher, context))
            return false

    // If any 'all of' matcher is not satisfied then the policy is not satisfied.
    for (const matcher of policy.allOf)
        if (!isSatisfiedMatcher(matcher, context))
            return false

    // If any 'any of' matcher is satisfied then the policy is satisfied.
    for (const matcher of policy.anyOf)
        if (isSatisfiedMatcher(matcher, context))
            return true

    // At this point there are
    // - no satisfied 'none of' matchers,
    // - no unsatisfied 'all of' matchers and
    // - no satisfied 'any of' matchers.

    // Hence, the policy is satisfied if it has
    // - an 'all of' condition and
    // - no 'any of' condition.
    return policy.allOf.size !== 0 && policy.anyOf.size === 0
}
```


## Satisfied Matcher

```javascript
function isSatisfiedMatcher(matcher, context) {

}
```
