# Static digital editions, but with a little help of centralized and shared dynamic services

Author1: Peter Andorfer
Author1: Austrian Centre for Digital Humanities at the Austrian Academy of Sciences
Author1: peter.andorfer@oeaw.ac.at

<!-- Preamble separator. Do not remove this. -->

## Abstract

Digital edition projects often depend on web applications that must remain available long after project funding ends. At the Austrian Centre for Digital Humanities (ACDH), maintaining a growing number of TEI/XML-based editions led us to rethink our technical infrastructure. Instead of operating a dedicated application stack for every project, we increasingly publish editions as static websites generated directly from TEI/XML. This approach minimizes operational complexity while preserving the functionality needed by researchers and readers. To support this workflow, we developed dse-static-cookiecutter, a static-site framework built around TEI, XSLT, and related XML technologies. Static websites reduce maintenance costs and improve long-term sustainability, but they also limit access to dynamic features such as full-text search, OAI-PMH endpoints, and corpus-linguistic services. Rather than reintroducing complexity into each edition, we provide these capabilities through a small number of centralized services shared across many projects. This article describes the evolution of our infrastructure, the design principles behind dse-static-cookiecutter, and the shared services that complement static editions.

## Introduction

Digital editions are a core activity at ACDH. Most are developed within third-party funded research projects and are expected to remain publicly available after funding has ended. While creating an edition is usually well funded, maintaining it for years or decades is much more difficult. The challenge is not preserving the TEI/XML data itself, but sustaining the software stack required to present and search that data.

Over the past decade (2016–2021) we experimented with several architectural approaches. Initially, multiple editions shared a common framework based on eXist-db[^1]. The idea was attractive: a single XML database, a shared code base, and project-specific customizations. In practice, however, the combination of diverse project requirements, database administration, and performance tuning proved difficult to scale. A problematic query in one project could affect others, and framework-level changes always carried the risk of introducing regressions.  
The next step was to separate projects into individual code bases while still relying on shared infrastructure. This improved development flexibility but increased maintenance costs. Eventually, containerization allowed each project to run its own instance of eXist-db. Operational isolation improved reliability, but the number of services requiring maintenance continued to grow.[^2]  
At the same time, our deployment environment evolved repeatedly, from custom Docker[^3] orchestration scripts to Portainer[^4] and ultimately to a Kubernetes[^5] cluster. Each infrastructure migration required effort across a steadily increasing portfolio of applications. The experience highlighted a fundamental problem: although individual projects differed in content and presentation, they all required a significant amount of operational support.  
These experiences motivated a different strategy. Instead of treating every edition as a dynamic application, we began exploring whether editions could be published as static websites while retaining access to selected dynamic features through shared services. The institute’s understanding of static websites is consistent with the general concept of the static website paradigm: serving pre-existing HTML files rather than relying on dynamic web services that retrieve data from databases and generate and serve HTML content on demand.

## Methodology

### Static publishing

The result of this work is dse-static-cookiecutter, a framework for generating static digital editions from TEI/XML. The project is based on the Python cookiecutter package[^6], and provides a reproducible template for new editions while still allowing project-specific customization. Currently we are running around 30 websites derived from the dse-static-cookiecutter.[^7]  
A key design goal was to remain close to the technologies already familiar to digital-editing practitioners. Many earlier editions relied on XSLT transformations executed within eXist-db. dse-static-cookiecutter preserves this model. TEI/XML documents are transformed into HTML using Saxon and Apache Ant. Both tools are mature, open-source, widely used in XML workflows, and integrate well with editors such as Oxygen XML Editor.  
The build process is defined through an Ant build file that specifies how source documents are transformed into website pages, which XSLT files are applied on which TEI/XML files. Because the transformation happens during the build phase rather than at request time, the resulting website consists entirely of static files that can be hosted on inexpensive and highly reliable infrastructure.  
To avoid duplication, the framework uses XSLT imports as a lightweight templating mechanism. Shared components such as navigation menus, page layouts, and footers are defined once and reused throughout the project. This keeps projects maintainable without introducing additional templating languages or dependencies.  
Features that would previously have relied on database queries are generated during the build process. For example, tables of contents and document overviews are created automatically from the source material. Register functionality requires a different strategy. Because static websites cannot query a database dynamically, information must be denormalized during the build. Generic Python utilities process the TEI corpus, collect references to entities such as persons or places, and store the resulting relationships in a form that can be rendered directly by the website.[^8]

The generated websites remain interactive through carefully selected JavaScript client-side libraries. Register tables can be sorted and filtered, for example, using Tabulator[^9]. At the same time, the underlying HTML remains functional even if a future browser no longer supports a particular library. Preserving graceful degradation is an important part of the long-term sustainability strategy.  
Most projects are built and deployed through GitHub Actions and published via GitHub Pages. The workflow definition doubles as executable documentation because it records both the required software and the sequence of build steps. For institutions preferring alternative infrastructure, the framework also generates a Dockerfile that can build and serve the site through Nginx.

### Shared dynamic services

Static websites greatly reduce operational complexity, but some features are difficult to provide without server-side infrastructure. Rather than embedding these services into every project, ACDH operates a small number of shared systems.  
The most widely used service is full-text search. We selected Typesense because it provides fast, typo-tolerant search, faceting, and keyword-in-context functionality while remaining comparatively easy to deploy and maintain.[^10] Each edition contributes its searchable content to a central Typesense instance.[^11] Individual projects can still create customized search interfaces,[^12] but the underlying infrastructure is shared.  
Centralization introduces dependencies. Changes to a search API may require updates across multiple clients. However, the impact of a failure is limited. Even if search becomes temporarily or permanently unavailable, the edition itself remains accessible because the website is static and self-contained.  
A second shared service addresses metadata interoperability. The dse-static-oai-pmh application provides OAI-PMH functionality for static editions. Participating projects expose structured metadata, and the service handles protocol-specific requests on their behalf. This allows static websites to participate in established harvesting ecosystems without requiring each project to operate its own server-side implementation.[^13]  
The OAI-PMH service also functions as a discovery layer. Because it exposes information about participating editions in a machine-readable format, it provides a convenient entry point for downstream services. We use this capability to harvest texts for linguistic enrichment and corpus analysis.  
The harvested material is processed by noske-fcs,[^14] a service that enriches texts and exposes them through SRU-compatible interfaces.[^15] This infrastructure enables integration with CLARIN’s Federated Content Search and related research environments. A central NoSketch Engine installation provides additional corpus-linguistic functionality across multiple editions.[^16] Researchers gain access to advanced search and analysis capabilities, while individual projects avoid the burden of maintaining specialized infrastructure.  
An important characteristic of this architecture is the separation of concerns. Editions focus on presenting and preserving scholarly content. Shared services provide functions that benefit from centralization, such as search, harvesting, and linguistic processing. This division allows both layers to evolve independently.

## Conclusion

The move from dynamic applications to static websites has significantly reduced the maintenance burden associated with digital editions at ACDH. Static hosting eliminates many operational concerns, including database administration, application security updates, and server-side performance tuning. In many cases, publication through GitHub Pages further reduces infrastructure requirements.  
The approach also improves transparency. The technologies involved, TEI, XSLT, Ant, and a small set of supporting bash and Python scripts are familiar to many digital-humanities practitioners. The build process is explicit and reproducible, and projects retain full control over their code.  
Equally important, the cookiecutter template encourages consistency across projects. New editions begin with a common foundation, making it easier to onboard developers and share knowledge. At the same time, projects remain free to customize their presentation and workflows.  
Static publishing is not a complete replacement for dynamic infrastructure. Search, metadata harvesting, and linguistic services still require servers. However, centralizing these capabilities changes the economics of maintenance. Instead of operating dozens of similar services, an institution can maintain a small number of robust platforms that support many projects simultaneously.  
For organizations responsible for long-term stewardship of digital editions, this combination of static publication and shared dynamic services offers a practical balance between sustainability and functionality. It reduces operational risk, lowers maintenance costs, and creates opportunities for cross-project discovery and analysis while preserving the flexibility required by individual scholarly editions.

## Notes

[^1]: Our first framework was cr-xq-mets ([https\://github.com/acdh-oeaw/cr-xq-mets](https://github.com/acdh-oeaw/cr-xq-mets)), a fork of SADE (Scalable Architecture for Digital Editions). It was later followed by wdbplus ([https\://github.com/dariok/wdbplus](https://github.com/dariok/wdbplus)). While development of cr-xq-mets ceased several years ago, wdbplus remains actively maintained and is currently used by institutions such as the Herzog August Bibliothek in Wolfenbüttel and the Technical University of Darmstadt. Both frameworks rely on the eXist-db ([https\://exist-db.org](https://exist-db.org)).
[^2]: Those apps were based on the dsebaseapp ([https\://github.com/KONDE-AT/dsebaseapp](https://github.com/KONDE-AT/dsebaseapp)).
[^3]: [https\://www\.docker.com/](https://www.docker.com/)
[^4]: [https\://www\.portainer.io/](https://www.portainer.io/)
[^5]: [https\://kubernetes.io/](https://kubernetes.io/)
[^6]: [https\://github.com/cookiecutter/cookiecutter](https://github.com/cookiecutter/cookiecutter)
[^7]: See [https\://github.com/acdh-oeaw/dse-static-cookiecutter\#projects-using-dse-static-cookiecutter-by-start-date](https://github.com/acdh-oeaw/dse-static-cookiecutter#projects-using-dse-static-cookiecutter-by-start-date) for a list of projects.
[^8]: A set of utility functions and common line scripts is published in the acdh-tei-pyutils ([https\://pypi.org/project/acdh-tei-pyutils/](https://pypi.org/project/acdh-tei-pyutils/)) Python package.
[^9]: [https\://www\.tabulator.info/](https://www.tabulator.info/)
[^10]: Early iterations of dse-static-cookiecutter based editions implemented a static full text search using projectEndings/staticSearch ([https\://doi.org/10.5281/zenodo.6329800](https://doi.org/10.5281/zenodo.6329800)). But due to the possibility of running our own Typesense instance we didn’t pursue this pure static approach any longer.
[^11]: Our institutional Typesense instance currently holds 125 collections indexing around 2 million documents using between 7 and 8 GB memory.
[^12]: The search pages in the individual edition-web-sites are built with [InstantSearch.js](http://InstantSearch.js) ([https\://www\.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/js](https://www.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/js)) and typesense-instantsearch-adapter ([https\://github.com/typesense/typesense-instantsearch-adapter](https://github.com/typesense/typesense-instantsearch-adapter)).
[^13]: [https\://dse-static-oai-pmh.acdh-dev.oeaw.ac.at/](https://dse-static-oai-pmh.acdh-dev.oeaw.ac.at/) currently exposes metadata of 21 projects.
[^14]: [https\://github.com/acdh-oeaw/noske-fcs](https://github.com/acdh-oeaw/noske-fcs)
[^15]: [https\://fcs.acdh.oeaw.ac.at/](https://fcs.acdh.oeaw.ac.at/)
[^16]: [https\://corpus-search.acdh.oeaw.ac.at/crystal/](https://corpus-search.acdh.oeaw.ac.at/crystal/)

## References (CSE Style Recommended)

Andorfer P. 2026\. Digitale Editionen als statische Webseiten: Zur nachhaltigen Publikation TEI-XML-basierter Editionen mit dem dse-static-cookiecutter. Zeitschrift für digitale Geisteswissenschaften. 11\. Available from: [https\://doi.org/10.17175/2026\_003](https://doi.org/10.17175/2026_003)
Typesense \[Internet\]. Available from: [https\://typesense.org/](https://typesense.org/)  
Audrey Roy Greenfeld et al. Cookiecutter \[software\]. GitHub. Available from: [https\://github.com/cookiecutter/cookiecutter](https://github.com/cookiecutter/cookiecutter)  
Austrian Centre for Digital Humanities. dse-static-cookiecutter \[software\]. GitHub. Available from: [https\://github.com/acdh-oeaw/dse-static-cookiecutter](https://github.com/acdh-oeaw/dse-static-cookiecutter)
Austrian Centre for Digital Humanities. dse-static-oai-pmh \[software\]. GitHub. Available from: [https\://github.com/acdh-oeaw/dse-static-oai-pmh](https://github.com/acdh-oeaw/dse-static-oai-pmh)
Austrian Centre for Digital. noske-fcs \[software\]. GitHub. Available from: [https\://github.com/acdh-oeaw/noske-fcs](https://github.com/acdh-oeaw/noske-fcs)
Holmes M, Takeda J. 2020\. projectEndings/staticSearch: v0.7 (first formal release) \[software\]. Zenodo. Available from: [https\://doi.org/10.5281/zenodo.6329800](https://doi.org/10.5281/zenodo.6329800)
