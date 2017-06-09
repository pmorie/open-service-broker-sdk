# Proposal: Enable simple, forward-compatible broker implementations

## Abstract

## Motivation

The OSB API is subtle to implement.  In order for the SDK to be successful, it
must be possible to implement a correctly functioning broker that is compliant
with the API's prescribed broker behaviors without deep knowledge of the OSB
API spec itself.  Once a user writes a broker using the SDK, they need to be
able to consume fixes and new features of the SDK in a forward compatible
manner.

## Goals

The nucleus of the value of the SDK comes from the following goals.  It should
be possible to:

1.  Quickly build a reliable broker without worrying about the details of the OSB API
2.  Easily move a broker built with the SDK onto a newer version

### Use cases

1. As a broker developer, I want to be able to implement a spec-compliant
   broker without being an expert in the OSB API, so that I can focus on my
   business logic and not building broker machinery
2. As a broker developer, I want to be able to pick up bug fixes and new
   features in the SDK without a major rebase of my own code, so that I can
   iterate on my business logic and product instead of spending time battling SDK
   API changes

## User experience

### The 80% use case

API should be:

```go
type BrokerBehaviors interface {
	Provision(*ServiceInstance)   (*ProvisionResult, error)
	Deprovision(*ServiceInstance) (*DeprovisionResult, error)
	Bind(*ServiceBinding)         (*BindResult, error)
	Unbind(*ServiceBinding)       (*BindResult, error)
}
```

### Updating to a new SDK version

## Implementation strategy

Forward compatible user API

User extensions in separate packages:

```go

// broker-sdk-package/types.go

type ServiceInstance struct {
	ServiceInstanceExtension
}

// sdk-user-package/types.go

// ServiceInstanceExtension holds the fields for the SDK user.  Users of the
// SDK can add fields to ServiceInstanceExtension without interfering with the
// broker mechanics.
type ServiceInstanceExtension struct {}
```
