# Affording the user control over where computations are carried out

## Authors

- Matthew Tylee Atkinson (Samsung Electronics)

- Ehsan Toreini (Samsung Electronics)

## Table of Contents

<!-- toc -->

- [Introduction](#introduction)
- [User-Facing Problem](#user-facing-problem)
  * [Goals](#goals)
  * [Non-goals](#non-goals)
- [User research](#user-research)
  * [Use Cases](#use-cases)
- [Proposed Approach](#proposed-approach)
  * [Proposed execution place enumeration values](#proposed-execution-place-enumeration-values)
  * [Indicating the 'direction' of the developer's preference](#indicating-the-direction-of-the-developers-preference)
  * [Dependencies on non-stable features](#dependencies-on-non-stable-features)
  * [Examples](#examples)
- [Alternatives considered](#alternatives-considered)
- [Accessibility, Internationalization, Privacy, and Security Considerations](#accessibility-internationalization-privacy-and-security-considerations)
  * [User preference fingerprinting](#user-preference-fingerprinting)
- [Stakeholder Feedback / Opposition](#stakeholder-feedback--opposition)
- [References & acknowledgements](#references--acknowledgements)

<!-- tocstop -->

## Introduction

As the tasks that web applications and content perform for users become more sophisticated, computations may be performed by components running in different places, provided by different providers.

People using the web have a variety of goals, resources, and tolerances for privacy and security trade-offs. However these user requirements are not reflected, nor catered for, by platforms, including the web. The best that users can currently expect is to find an OS toggle that limits ML computations to "on-device" (though this term could mean many things, and is not consistent across devices and platforms).

We propose a partial API that could be applied to any spec that carries out significant computations (particularly those that involve the user's personal data). This would allow the User Agent (UA) the opportunity to constrain how the computations are carried out, on behalf of the user, and according to their needs.

> [!NOTE]
> The approach proposed here is in draft state. An alternative would be to not necessarily require a specific API surface, but just to ensure that specs can cater to the user's preferences and constraints using a standard taxonomy (as proposed below).

## User-Facing Problem

The user has constraints or preferences that dictate where they would be able to accept computations being done on their behalf. They need the UA to honour those constraints when executing those computations.

Developers also have similar preferences and constraints, and so a system is needed whereby the needs of the user (taking precedence) and developer can be accommodated.

### Goals

- Specify a range of standard "places" where computations could occur, so UAs can ensure they are carried out in support of the user's needs.

- Specify the means by which the user's and developer's needs can be reconciled (if possible).

### Non-goals

- Specifying the User Interface that would allow users to indicate their preferences to their UA.

- Defining model-loading infrastructure.

- Addressing wider issues around privacy and security that relate to downloading of additional (OS, UA) components.

- Addressing the lifecycle of components (provided by a number of different possible sources). This is just about the API shape.

- Addressing issues of what locally-running ML can see in the UA.

## User research

*This section to be expanded.*

### Use Cases

0. UA calling OS-provided APIs (e.g. existing OS-provided spelling and translation APIs).

1. Sites using an API provided by the browser, e.g. Translation API.

2. Sites using their own model to provide a computation, e.g. Web Neural Network API.

## Proposed Approach

* Define a standard set of places where computations could run.

  Refer to the proposed enumeration values below.

* Add to the API a required parameter that indicates where the developer would like the computation to run.

  This could be done each time a model is called, or during some (required) API set-up function.

* Add an optional parameter that indicates the direction of the preference.

  I.e. will the developer accept either _more_ or _less_ privacy-preserving locations than the one they requested?

  The default is to assume that the developer will accept _more_ privacy-preserving locations.

* The UA picks the most privacy-preserving location that the user and developer agree on.

### Proposed execution place enumeration values

We propose an enumeration of progressively less privacy-preserving options.

1. On my devices (technically secure) - `personal-secure`

   On the local device, in the secure area of the CPU _OR_ in the secure area of the CPU of another device I own that is connected securely to the local one.

2. On my devices - `personal`

   On the local device _OR_ on another device I own that is connected securely to the local one.

3. Remote (technically secure) - `remote-secure`

   Various flavors of private computation on other people's computers, ranging from homomorphic encryption to TEEs.

4. Remote (legally trusted) - `remote-attested`

   "Service providers" (in the GDPR sense) for the user, who have committed not to use or expose the data outside that particular user's context.

5. Remote (no guarantee) - `remote`

   Distinct data controllers (again in the GDPR sense), who could use the user's data for their own purposes. This secondary use might help subsidize free or cheaper service, but it also makes this category challenging.

### Indicating the 'direction' of the developer's preference

It is assumed that as we go down the above list of proposed computation locations, they become less privacy-preserving.

It could also be the case that the level of 'capability' from step to step is an increasing function, but that's not important right now.

When the site calls for a computation, it also has a preference as to where the computation is run.

By default, we assume that the site will tolerate being run in that location _or a more privacy-preserving one_. This aligns with the [UA principle that the user should be safe for the user to visit a web page](https://www.w3.org/TR/web-user-agents/#protection).

If the developer wishes the computation to run in their chosen location or a _less_ privacy-preserving location, this can be specified.

Thus we have another two-value enum to indicate the direction of the developer's preference:

* Run in _more_ privacy-preserving locations (the default) - `more-privacy-preserving`

* Run in _less_ privacy-preserving locations - `less-privacy-preserving`

### Dependencies on non-stable features

None

### Examples

Some possible API shapes (all names and details subject to feedback)...

```js
model.run(..., 'device-secure')
```

```js
model.runOn('remote-attested')
```

```js
model.runOn('remote-attested', 'more-privacy-preserving')
```

```js
model.run(..., {
  runOn: 'remote-attested',
  or: 'less-privacy-preserving'
})
```

## Alternatives considered

*This section to be expanded.*

## Accessibility, Internationalization, Privacy, and Security Considerations

*This section to be expanded.*

### User preference fingerprinting

There is a risk that sites will simply probe for computations to be run in different locations, in order to determine the user's constraints.

To mitigate this, the API should require user activation, _or_ provide sufficient rate limiting.

## Stakeholder Feedback / Opposition

*This section to be expanded.*

## References & acknowledgements

* [Initial public TAG post on design review for "On-device Web Speech API" after discussion on this](https://github.com/w3ctag/design-reviews/issues/1038#:~:text=A.%20The%20same,this%20category%20challenging.)
