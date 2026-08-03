# Using IDC with an AI assistant

You can ask an AI assistant to search IDC, size and refine a cohort, check licenses, generate citations, and hand you a download command — in plain conversation, without writing any code.

There are two ways to give an assistant that ability, and a third option if you would rather write the code yourself.

## Which one should I use?

| Your situation | Use |
|---|---|
| You want the fastest start, or your assistant cannot run code | **[Hosted server (MCP)](../mcp/README.md)** — paste one URL into your client. Nothing to install, no account, no API key. |
| You want the widest coverage, and your assistant can run Python (Claude Code, Cursor, Codex, Gemini CLI; claude.ai or Claude Desktop with code execution enabled) | **[IDC agent skill](skill/README.md)** — adds BigQuery, DICOMweb, direct bucket access, and the digital-pathology and clinical-data guides that the hosted server does not expose. |
| You are writing the script, application, or notebook yourself | **[REST API](../api/README.md)** — plain HTTP/JSON, in any language. |

These are not mutually exclusive. A good default is to start with the hosted server, then add the skill when you need the extra reach — the skill's own setup guide recommends running both.

## What you can ask for

Once connected, requests like these work without further prompting:

* *"Find breast MRI in IDC, show the counts and total size, and give me a download command."*
* *"Which IDC collections have prostate MRI with expert segmentations?"*
* *"How much CT data is there for lung cancer, and how much of it is cleared for commercial use?"*
* *"Generate citations for the collections in my cohort."*

The assistant discovers the valid filter values first, sizes the cohort, and returns the download commands on its own.

## What all three paths share

Whichever path you pick, you are querying the same thing in the same way:

* **One metadata index.** All three read the same IDC metadata — the same collections, the same attributes, the same values.
* **One data model.** Program → collection → patient → study → series, as described in [Core concepts](../api/idc-api-concepts.md#the-data-model).
* **One recommended workflow.** Ground the values you intend to filter on, build and size the cohort, then retrieve. See [the query surfaces and how they relate](../api/idc-api-concepts.md#the-query-surfaces-and-how-they-relate).
* **One set of obligations.** Files transfer directly from public S3/GCS buckets, and the CC BY / CC BY-NC terms and citation requirements are identical regardless of how you found the data.

The hosted server and the REST API additionally share a single backend, so a capability is implemented once and exposed in both.

## Precautions

{% hint style="warning" %}
**Check the assistant's work before you act on it.** Assistants can invent plausible-looking collection names, modality codes, or attribute values. A well-behaved agent grounds every filter value against IDC first — but you should still sanity-check that the collections it names are real ones, and **always look at the reported case/series counts and the total download size before starting a download**. IDC holds well over 100 TB.
{% endhint %}

Two more things worth knowing:

* **Version 3 is in beta.** The hosted server and REST API are released together as version 3, currently in beta; the contract may still change before the final `3.0.0` release. Feedback is welcome on the [IDC support forum](https://discourse.canceridc.dev).
* **Licensing still applies.** Most IDC data is CC BY, but a subset is CC BY-NC and cannot be used commercially. Ask the assistant for the license breakdown of your cohort, and include the citations it generates when you publish.

## In this section

* [Hosted server (MCP)](../mcp/README.md) — what the server is and how it relates to the REST API.
  * [Connecting a client](../mcp/getting-started.md) — point claude.ai, Claude Desktop, or any MCP client at it.
  * [Tools and resources](../mcp/tools.md) — the full list of capabilities it exposes.
* [IDC agent skill](skill/README.md) — what the skill adds, and when it is worth the extra setup.
  * [Setting up the skill](skill/setup.md) — installation, and the settings that trip people up.
