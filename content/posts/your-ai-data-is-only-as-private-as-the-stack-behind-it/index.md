---
date: 2026-09-10
description: "A privacy policy tells you what a vendor promises, not what actually happens to your data once it enters the AI stack behind the model. This post looks at the gap between data residency and data sovereignty, why \"we don't train on your data\" isn't the end of the conversation, and what leaders should actually be asking before their next AI contract."
image: "/posts/your-ai-data-is-only-as-private-as-the-stack-behind-it/images/data-sovereignty-hero.png"
lastmod: 2026-09-10
showTableOfContents: false
tags: ["ai", "data privacy", "enterprise technology", "technical leadership"]
title: "Your AI Data Is Only as Private as the Stack Behind It"
type: "post"
---

![Your AI Data Is Only as Private as the Stack Behind It](/posts/your-ai-data-is-only-as-private-as-the-stack-behind-it/images/data-sovereignty-hero.png)

I have sat in enough client rooms to have lost count of how many times someone has pasted a contract, a roadmap, or a customer list into an AI chat window without a second thought. Nobody asks where it goes; they just want to know if the answer is good.

I understand why, but it has always troubled me.

## Data residency is not data sovereignty

These two ideas often get mixed together. Data residency is primarily about where your data is geographically stored and, depending on the service, processed. Data sovereignty is broader: it is about who can exercise legal or operational control over that data, which jurisdictions apply, which companies operate the infrastructure, who holds the keys, and who can ultimately be required to provide access. That distinction matters.

Under the US CLOUD Act, for example, a provider subject to US jurisdiction can be required to produce data within its possession, custody, or control, even when that data is stored somewhere else. That does not mean a US order magically cancels GDPR: European law has its own rules around transfers and disclosures to third-country authorities. But it does mean that choosing an EU region for data storage is not necessarily the same thing as removing US jurisdictional exposure. The physical location of the server is only one part of the answer.

Most companies do not think deeply about this until a client, a regulator, or a security review forces the question. By then, somebody has usually already started pasting things into the chat window.

## Where your prompt actually goes

The mental model most people carry is simple: I type something, a model answers, and that's it.

The real path can be considerably longer. Depending on the product and architecture, an interaction may involve inference infrastructure, request and security logs, abuse-monitoring systems, cached context, audit records and subprocessors. Add RAG or enterprise search and you may introduce embeddings, vector databases and document stores as well.

Then agents arrive. An agent retrieving documents, reading Jira, querying a knowledge base, writing traces, calling APIs and using MCP tools can move information through several systems before it gives you a single answer. This is the part I think organizations still underestimate: you are not just choosing a model, you are choosing a data supply chain.

## "We don't train on your data" is not the end of the conversation

To be fair to the large AI providers, their enterprise privacy controls have improved considerably. OpenAI states that it does not use business or API data for model training by default and offers Zero Data Retention for eligible API customers. Google says customer data in Vertex AI is not used to train its models without permission and documents configurations for achieving zero data retention. Anthropic's current policy states that standard API inputs and outputs are deleted from its backend within 30 days, while qualifying customers can obtain Zero Data Retention for eligible products and models.

Those are meaningful controls, but "no training," "no retention," and "the data stays in Europe" are three different promises. Zero Data Retention can also be product-, feature-, or model-specific. Administrative audit information may follow different retention rules, safety systems can have their own exceptions, and connectors can introduce completely different vendors and contractual boundaries. Even within the same company, a consumer chat application, an enterprise workspace and an API can operate under very different terms.

That is why I don't think a privacy statement is enough. I want to know which entity is processing the data, where inference happens, what gets logged, which subprocessors are involved, which features persist content, how deletion works, what happens when someone enables a connector, and what evidence exists that those controls actually work.

A privacy policy is where that conversation starts, not where it ends. In my experience, I have yet to see a vendor produce a clear data flow diagram without being asked twice.

## The provider is only one part of the attack surface

There is another part of this conversation that isn't really sovereignty in the strict legal sense, but absolutely belongs in the same risk assessment: everything surrounding the provider itself.

In March 2026, malicious versions of LiteLLM, a widely used open-source gateway for connecting applications to different AI providers, were published as part of a broader software supply-chain attack. CloudSEK later reconstructed potential exposure affecting more than 2,500 organizations and roughly 434,000 CI/CD pipelines, though their report makes a distinction worth preserving: exposure does not mean every organization was successfully compromised.

Mercor subsequently confirmed it had been affected by the same incident. Its investigation found that sensitive information belonging to a limited subset of its experts had been affected, while the impact on customer information was described as very limited. IBM's 2026 X-Force Threat Intelligence Index adds another uncomfortable data point: more than 300,000 sets of ChatGPT credentials were advertised on the dark web during 2025, largely driven by infostealer malware.

None of that means the frontier AI labs are uniquely careless. It means the system around the model is becoming large enough that asking whether you trust the model provider is no longer sufficient. You need to understand who else is in the room.

## Europe is starting to treat sovereignty as an architecture requirement

The regulatory direction in Europe is increasingly difficult to ignore. The AI Act is part of that picture, although its high-risk system requirements have now been pushed further out following the 2026 AI Omnibus. More directly relevant to sovereignty is the European Commission's proposed Cloud and AI Development Act. It proposes four sovereignty assurance levels for cloud and AI infrastructure, ranging from keeping data and processing inside the EU through progressively stronger requirements around third-country independence, EU ownership and control, supply-chain transparency, and protection from foreign interference. That tells me more about where this discussion is heading than another privacy-policy update does.

The market appears to be going in the same direction. Gartner forecasts European sovereign-cloud IaaS spending growing from around $6.9 billion in 2025 to $12.6 billion in 2026, an increase of roughly 83 percent, and worldwide sovereign-cloud IaaS spending is forecast to exceed $80 billion this year. This is no longer just a conversation between lawyers and security teams. It is starting to affect architecture and procurement, and I have watched teams discover these requirements halfway through a deal, which is exactly the wrong moment to find out that nobody knows the answer.

## Keeping the model closer to home

This is one reason I have become increasingly interested in open-weight and self-hosted AI. Not because I think everything should move back on-premises: it shouldn't. But self-hosting has become a serious architectural option rather than something you choose only when no good cloud alternative exists.

There are several capable open-weight model families now, including Llama, Qwen and DeepSeek, but Mistral is the one I have spent the most time looking at. I attended the [Mistral AI Now Summit](https://ainowsummit.com/) in May 2026, and one thing that stood out was just how central sovereignty and deployment control had become to their enterprise story.

Their current model lineup makes the argument more interesting. Mistral Small 4 is available under Apache 2.0, while Mistral Medium 3.5 is released as open weights under a modified MIT license. Medium 3.5 is a dense 128B model that Mistral says can be self-hosted on as few as four GPUs. That's still substantial infrastructure, but it puts a genuinely capable model within reach of an enterprise GPU cluster.

The surrounding stack has matured as well. Le Chat is now Vibe (a rename I still don't love), combining enterprise knowledge work and coding, while Studio covers building and governing AI applications and Forge handles model customization. More importantly for this discussion, Mistral supports enterprise deployments on-premises and in private cloud environments.

But there is an important caveat: self-hosted does not automatically mean sovereign, and it certainly does not automatically mean secure. Someone now owns patching, capacity, identity and access management, monitoring, backups, incident response and model upgrades. You still need to understand telemetry, support access, connectors and every external dependency around the system. You haven't removed responsibility. You have moved it.

For organizations handling genuinely sensitive workloads, that can be exactly the trade they want. For everybody else, it needs to be an architectural decision rather than an ideological one.

## What this means for leaders

Before signing another AI contract, start by classifying what you are actually going to give the system. For low-sensitivity, non-regulated workloads, a well-run enterprise cloud service with appropriate contractual terms, retention controls and access management may be perfectly reasonable, but the bar should rise as the data becomes more sensitive.

Client contracts, proprietary source code, personal or otherwise regulated data, confidential business reports, unreleased strategy, and anything else capable of materially hurting the organization if exposed deserve more than "the vendor says it doesn't train on our prompts".

Maybe the answer is Zero Data Retention. Maybe it is a specific data region, customer-managed encryption keys, tighter connector governance, or a private cloud deployment. And sometimes the answer genuinely is to run the model inside infrastructure you control.

The point is not that cloud AI is unsafe. The point is that convenience is not a security model. The excitement around AI has made it remarkably easy to skip the same architecture, procurement and security questions we would ask of any other system touching sensitive company data, and we shouldn't.

Next time I'm sitting in one of those client rooms and someone reaches for the paste button, I don't necessarily want them to stop. I just want them to hesitate for a second and ask one question: where is this information actually going?

That's a habit worth the whole company catching.

## Sources

- [U.S. Department of Justice — CLOUD Act Resources](https://www.justice.gov/dag/cloudact)
- [European Data Protection Board — Guidelines 02/2024 on Article 48 GDPR](https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-022024-article-48-gdpr_en)
- [OpenAI — Enterprise Privacy](https://openai.com/enterprise-privacy/)
- [OpenAI — Zero Data Retention for Frontier Models](https://openai.com/index/offering-zero-data-retention-for-frontier-models/)
- [Anthropic Privacy Center — How long do you store my organization's data?](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data)
- [Google Cloud — Vertex AI and Zero Data Retention](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/vertex-ai-zero-data-retention)
- [CloudSEK — LiteLLM Supply Chain Attack: 2,500+ Companies Exposed](https://www.cloudsek.com/blog/ai-supply-chain-breach-2500-companies-434000-cicd-pipelines)
- [Mercor — Update on Mercor Security Incident](https://www.mercor.com/blog/update-on-mercor-security-incident/)
- [IBM — 2026 X-Force Threat Intelligence Index](https://www.ibm.com/think/x-force/threat-intelligence-index-2026-securing-identities-ai-detection-risk-management)
- [European Commission — AI Omnibus Enters into Force](https://digital-strategy.ec.europa.eu/en/news/ai-omnibus-enters-force)
- [European Commission — Cloud and AI Development Act](https://digital-strategy.ec.europa.eu/en/policies/cloud-and-ai-development-act)
- [Gartner — Worldwide Sovereign Cloud IaaS Spending Will Total $80 Billion in 2026](https://www.gartner.com/en/newsroom/press-releases/2026-02-09-gartner-says-worldwide-sovereign-cloud-iaas-spending-will-total-us-dollars-80-billion-in-2026)
- [Mistral AI — Models](https://docs.mistral.ai/models)
- [Mistral AI — Mistral Medium 3.5](https://mistral.ai/news/vibe-remote-agents-mistral-medium-3-5/)
- [Mistral AI — Studio](https://mistral.ai/products/studio/)
- [Mistral AI — Forge](https://mistral.ai/products/forge/)
