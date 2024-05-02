# Farming and Countryside Programme (FCP) Development Guide

This is a repository of standards and supporting guidance for all developers working within the Farming and Countryside Programme (FCP).

The purpose of the standards are to ensure that delivery supports the Architecture Vision set out by the programme and better enable developer agility and mobility through consistent patterns and practices.

The guide also provides help and support for common setup and problem solving.

## Architecture Vision

The FCP Architecture vision is to deliver a modern, cloud native, event driven microservice ecosystem.

Containerised Node.js with JavaScript microservices are the primary delivery mechanism for the programme.  These are deployed to the Azure Kubernetes service (AKS) and are expected to be highly available, scalable and resilient.

For scenarios where Node.js is not appropriate, Defra's secondary framework and language is .NET and C#.

## Defra standards

The standards and guidance contained here is intended to be an FCP context specific layer over Defra's wider development standards.

This guide assumes that Developers already understand and follow these [core standards](https://github.com/DEFRA/software-development-standards/).

## GDS standards

The Government Digital Service (GDS) also have a set of standards that all government services must adhere to.  These are also assumed to be understood and followed by developers working within FCP.

- [Service Manual](https://www.gov.uk/service-manual/service-standard)
- [Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)
