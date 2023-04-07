---
sort: 6
---

# Linked Data Interactions

The Linked Data Interactions Repo (LDI) is a bundle of basic SDKs used to receive, generate, transform and output Linked Data available as open-source components on [GitHub](https://github.com/Informatievlaanderen/VSDS-Linked-Data-Interactions). The repository has the following structure
- ldi-api → contains a bundle of generic interfaces and classes to be used in the LDI SDKs
- ldi-core → contains various SDKs which can be wrapped in a desired implementation framework
- Implementation modules → the repository contains wrappers for [Apache Nifi](https://github.com/Informatievlaanderen/VSDS-Linked-Data-Interactions/tree/main/ldi-nifi/ldi-nifi-processors) and our own [LDI Orchestrator](https://github.com/Informatievlaanderen/VSDS-Linked-Data-Interactions/tree/main/ldi-orchestrator).
  
The sections below give an overview of the available SDKs.

## LDES Client

The [LDES Client SDK]((https://github.com/Informatievlaanderen/VSDS-Linked-Data-Interactions/tree/main/ldi-core/ldes-client)) contains the functionality to replicate and synchornise an LDES, and to persist its state for that process. More information on the functionalites can be found [here](/docs/LDES_client.md).

This is achieved by configuring the processor with an initial fragment URL. When the processor is triggered, the fragment will be processed, and all relations will be added to the (non-persisted) queue.

As long as the processor runs, a queue that accepts new fragments to process is maintained. The processor also keeps track of the mutable and immutable fragments already processed.

It will be ignored when an attempt is made to queue a known immutable fragment. Fragments in the mutable fragment store will be queued when they're expired. Should a fragment be processed from a stream that does not set the max-age in the Cache-control header, a default expiration interval will be used to set an expiration date on the fragment.

Processed members of mutable fragments are also kept in state. They are ignored if presented more than once.

## Version Object Creator

The Version Object Creator building block is responsible for converting a state object into a version object. 
This is the case in which entities, such as addresses, do not understand the concept of time. 

## Version Materializer
The Version Materializer building block is responsible for converting a version object back into a state object. 

## NGSI-V2 to NGSI-LD

As we are working with Linked Data, NGSI-V2 can not be used in a Linked Data Event Stream. 
This building block is responsible for converting NGSI-V2 data to NGSI-LD format in case the data publishers works with a context broker that only supports NGSI-V2.

Here you can find a [workflow of the IoW case](https://github.com/Informatievlaanderen/VSDS-LDES-E2E-testing/blob/6228fe5add3f8f47354b53b723ded69a945ec4d3/use-cases/iow/workflow.json) where this processor is used.

## SPARQL Construct Transfomer
SPARQL Construct is a query language used in semantic Web technologies to create RDF (Resource Description Framework) graphs from existing RDF data. It allows users to specify a pattern of data they wish to extract from the RDF data and construct a new graph based on that pattern.

The SPARQL Construct query language provides a powerful way to create new RDF data by using existing data as the input. It can be used to transform RDF data into different formats, as well as to simplify the structure of RDF data by aggregating or filtering data. 

This SPARQL Construct Transfomer building block can be used to execute model transformations.

## "RDF4J PUT"-processor
The RDF4J PUT Processor building block is responsible for writing RDF data to triple stores that support the RDF4J API. This enables the import of LDES data into triple stores such as [GraphDB](https://www.ontotext.com/products/graphdb/).