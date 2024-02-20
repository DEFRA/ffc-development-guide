# Farming and Countryside Programme (FCP) Development Guide

This is a repository of standards and supporting guidance for all developers working within the Farming and Countryside Programme (FCP).

The purpose of the standards are to ensure that delivery supports the Architecture Vision set out by the programme and better enable developer agility and mobility through consistent patterns and practices.

The guide also provides help and support for common setup and problem solving.

## Defra standards

The standards and guidance contained here is intended to be an FCP context specific layer over Defra's wider development standards.

This guide assumes that Developers already understand and follow these [core standards](https://github.com/DEFRA/software-development-standards/).

## Platform

### FCP Platform
To support rapid, highly assured delivery, the programme has delivered common Azure environments, PaaS components, delivery pipelines and supporting tools.

These are collectively referred to as the `FCP Platform`.

All FCP developed services are currently deployed to the FCP Platform.  

Guidance for use of the FCP Platform is hosted within this repository.

### ADP Platform
Following the success of the FCP Platform, Defra has created a Defra wide iteration known as the `ADP Platform`.

The ADP Platform is built on the same principles as the FCP Platform, but takes into account lessons learned throughout the lifetime of the FCP Platform to provide a better experience for teams utilising it.

It is the long term intention of the programme to migrate all FCP Platform hosted services to the ADP Platform once it becomes mature enough to provide feature parity.

Guidance for use of the ADP Platform is hosted within the [ADP Documentation](https://defra.github.io/adp-documentation/).

### Choosing a Platform
When starting a new project, the choice of platform should be made in consultation with the FCP Architecture team.  However, it is expected that all new projects will be hosted on the ADP Platform unless there is a compelling reason not to.

Early engagement with the ADP Platform team is encouraged to ensure that the platform is suitable for the project's needs.
