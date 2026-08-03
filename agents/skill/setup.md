# Setting up the agent skill

{% hint style="info" %}
The instructions below cover the common cases. The authoritative and always-current install guide is [USAGE.md](https://github.com/ImagingDataCommons/imaging-data-commons-skill/blob/main/USAGE.md) in the skill repository — check there if anything here does not match what you see.
{% endhint %}

## Coding agents (Claude Code, Cursor, Codex, Gemini CLI)

Install the skill with one command:

```bash
npx skills add ImagingDataCommons/imaging-data-commons-skill
```

This detects the agents installed on your system and offers to install the skill for them.

Then either invoke it explicitly with `/imaging-data-commons`, or just ask an imaging question and let the assistant load it on its own.

To confirm it worked, ask: *"What skills do you have for medical imaging data?"*

{% hint style="success" %}
**Recommended:** add the [hosted MCP server](../../mcp/README.md) alongside the skill. In Claude Code:

```bash
claude mcp add --transport http idc https://api.imaging.datacommons.cancer.gov/mcp
```
{% endhint %}

## claude.ai and Claude Desktop

**1. Install the skill.** Download the latest release ZIP from the [releases page](https://github.com/ImagingDataCommons/imaging-data-commons-skill/releases), then go to [claude.ai/customize](https://claude.ai/customize), select **Skills**, and upload the ZIP.

**2. Configure code execution.** This step is easy to miss, and skipping it is the most common reason the skill appears to be installed but silently fails — the assistant needs to run Python and reach the network to do anything useful. Under **Settings › Capabilities**:

* Under **Code execution and file creation**, enable **Allow network egress**.
* Set the domain whitelist to **Package managers only**.
* Add these to **Additional allowed domains**:
  * `*.github.com` and `*.githubusercontent.com`
  * `*.googleapis.com`
  * `*.s3.amazonaws.com`

To confirm it worked, ask: *"What do you know about the Imaging Data Commons?"*

For access that persists across conversations in Claude Desktop, create a **Project**, upload the ZIP to the project's knowledge base, and configure the same capability settings.

## Any other assistant

Any agent that supports the Agent Skills format can install it the same way as the coding agents above, substituting your agent's skills directory.

For an assistant with no skill support, load the skill's `SKILL.md` into the system prompt or paste it at the start of the conversation, and pull individual files from the `references/` folder when a task calls for them.

## Check that it worked

Ask the assistant which IDC data version it is working from. If it answers with a version number, the skill is loaded and its Python environment is working. If it cannot answer, or reports a network error, revisit the capability settings above.

## Updating

New releases track new IDC data versions, so update periodically:

* **Coding agents:** re-run `npx skills add ImagingDataCommons/imaging-data-commons-skill`, or `git pull` if you installed by symlinking a clone.
* **claude.ai / Claude Desktop:** download the new release ZIP and re-upload it.

Then **start a new conversation** — an assistant binds its skills at the start of a session, so an in-flight conversation keeps using the old version.
