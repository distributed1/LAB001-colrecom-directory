# 1. high-level content and recommendation design

Date: 2023-06-15

## Status

proposed

## Context

We are requested to build a headless platform to allow users to create Content, visualize this Content in the Catalog and be able to Recommend a specific Content with a simple 0-5 points score.

Our fictional scenario requires high availability and performance.  
We expect 80/100 of reads vs. updates,

## Decision

We are choosing a CQRS distributed approach.  
We will count on a Content Management Service that will be responsive to manage the creation and update of the content and recommendations.  
This service will have a relational storage system.

The catalog will provide a calculated view of the content for the users. We will choose no-relational storage for this view.

Considerations:
* We want to avoid calculate the total score for a content in the content service; we will store each Recommendation and Content in their respective tables and trigger a domain event when they are created
* The catalog would be responsive for now to calculate the avg Recommendation score and update the Content representation in its database

![](../diagrams/adr0001.png)

## Consequences

We assume the complexity of adding read-update segregation to gain performance and separate scaling for both the catalog and content management system service independently.