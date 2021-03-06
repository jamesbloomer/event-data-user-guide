# Transparency, Authority and Trust

You can look at CED as a stream of assertions of relationships between things. When you interpret an assertion, you should know exactly who it is that is making the assertion.

## Source data

The first level of authority is the source itself:

1. Twitter tells asserts to the CED Agent that this Tweet ID has this text.
2. The web server asserts to the CED Agent that this webpage has this text.
3. DataCite asserts to Event Data that their metadata links this DOI to this one.

You could take a further step back:

1. The Twitter User asserts to Twitter that their tweet should have this text.
2. The web author asserts to the web server that the blog should have this text.
3. The DataCite member asserts to DataCite that this DOI is related to this other DOI.

The CED chain of provenance ends at the boundary of the CED system. We record what an external system or party told us. However, you may wish to bear this in mind when deciding how you interpret the data.

## Relations

Every subject-relation-object Event is an assertion made by the Agent. When deciding whether or not you trust an Event, you should look at the Agent ID and make your decision on that basis. Some Agents, like Crossref Metadata and DataCite Metadata, are operated by organisations that directly hold the data. Some Agents, like Reddit or Newsfeeds, observe things on the web and make assertions about what they saw. The Evidence Record provides the link between what the external party said and the Event.

## Landing Page to DOI mappings

When an Event links to a DOI but the URL in the corresponding `obj` metadata is different, the Agent has made a judgment about an article landing page. The Event therefore contains two assertions:

 - this webpage mentions this article landing page
 - this article landing page corresponds to this DOI

The Evidence Record for each Event documents this mapping in some detail.

Agents that perform this mapping have a few methods available. The first method is to look for the DOI embedded in the URL. This happens quite often, e.g. PLOS:

    http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0160617

There are other similar methods to this. If we are unable to get the DOI from the URL, we will visit the URL. If the landing page has the correct metadata tags in the HTML, we will use that. e.g. from the same PLOS Page:

    <head>
      <meta name="dc.identifier" content="10.1371/journal.pone.0160617" />
    </head>

This means that in cases like this, the landing page URL to DOI mapping is made on the **Publisher's authority**. As a consumer of the service, it is up to you to decide which data you want to trust.

We maintain a list of publisher domains (see the [Artifacts](artifacts)). This list is generated by following a sample of DOIs to see where they point to. We will not attempt to look at the Landing Page that is not on this list.

## Data Aggregator or Provider?

CED is a both a pipeline for providing access Events and a family of Agents, operated within and outside Crossref and DataCite. 

The NISO Code of Conduct describes an 'altmetric data aggregator as':

> Tools and platforms that aggregate and offer online Events as well as derived metrics from altmetric data providers (e.g., Altmetric.com, Plum Analytics, PLOS ALM, ImpactStory, Crossref).

CED is an 'aggregator' by this definition. We offer 'online events' but we **do not provide metrics**. For some Data Sources that originate within Crossref, CED is also a 'Provider' according to the NISO definition.

<a name="concept-duplicate"></a>
## Duplicate Data

When an Event occurs in the wild it may be reported via more than one channel. For example, a blog may have an RSS feed that the Newsfeed agent subscribes to. It may also be included in a blog aggregator's results. In this case the action of publishing the blog post might result in two Events in CED. Note that two Events that describe the same external action via two routes will have different Event IDs.

It is important that CED reports Events without trying to 'clean up' the data. The Evidence pipeline ensures that every input that should result in an Event, does result in an Event. 

Becuase it's up to consumers to interpret the data as required, one consumer may treat duplicates differently. Some consumers may treat the same event coming from two Agents as a corroboration, and therefore significant. Some consumers may wish to just ignore duplicates.

Each Agent will itself try to avoid generating the same event twice. E.g. if a single tweet results in an Event, that Event should only be created once. However, CED makes no *guarantees* that this won't happen.

<a name="concept-evidence-first"></a>
## Evidence First

Crossref Event Data contains data from external data providers, such as Twitter, Wikipedia and DataCite. Every Event is created by an Agent. In most cases, such as Twitter and Wikipedia, the Agent is operated by Crossref, not by the original data provider. In some cases, such as DataCite, the data provider runs the Agent themselves.

Converting external data into Events provides an evidence gap. It raises questions like:

 - what data was received?
 - how was it converted into Events?
 - if some other service got the same data, why did it produce different results?

<img src="../../images/evidence-first-evidence-gap.svg" alt="Evidence Gap" class="img-responsive">

#### Bridging the Evidence Gap

CED solves this by taking an **Evidence First** approach. For every piece of external data we receive we create an Evidence Record. This contains all the relevant parts of the input data and all supporting information needed to process it. This means that we can provide evidence for every Event. 

<img src="../../images/evidence-first-bridge.svg" alt="Bridging the Evidence Gap" class="img-responsive">

Evidence is important because it bridges the gap between generic primary Data Providers, such as Twitter, with the specialized Events in CED. They explain not only what data were used to construct an Event, but also the process by which the Event was created. Providing this explanation pinpoints the precise meaning of the Event within the individual context it comes from.

A number data providers (including, for example, CED) may produce equivalent data. Evidence enables two events from different providers to be compared and helps to explain any discrepancies. It allows the consumer to check whether they were working from the same input data, and whether they processed it the same way.

<a name="evidence-not-all"></a>
#### Not all Events need Evidence

Evidence bridges the gap between external data in a custom format and the resulting Events. There are two factors at play:

 - the format of the data, which is in some external format and needs to be processed into to Events
 - the fact that the original Data Source is controlled by a different party than the Agent that produces Events

Some sources understand and produce Events directly. Examples of these are the Datacite Metadata and Crossref Metadata sources. In these cases, the Events themselves are considered to be Primary Data.

We may also accept Events directly from external Data Sources, where the external source runs their own Agent and provides Events as primary data. In this case the Source may not be able or willing to provide any additional evidence beyond the Events themselves.

For more information see [Evidence Records](evidence-records) page.

