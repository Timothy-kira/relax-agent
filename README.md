# Relax Agent

Reusable skills for cloud experiments, evidence review and distributed collaboration across Kaggle, Colab, GitHub and Codex.

This private repository starts the shared skill section. It contains no historical task logs, credentials, account configuration or experiment bundles. The existing plugin implementation remains in its existing repository while the new integrations are validated.

## Skill section

Approved native skills belong in `skills/<skill-name>/SKILL.md`. Each skill has standard `name` and `description` frontmatter. Topic tags, tested environment, hardware limitations and exact evidence references describe its applicability. A managed export can include a non-executable `relax-source.json` provenance witness.

The skill section is initially empty. A directory does not establish that a host has discovered a skill or that its method has been independently reproduced.

## One catalogue, several sources

The plugin's existing topic-skill index remains the local management and retrieval entry point. Local learned skills, this repository and explicitly configured team repositories retain their own provenance and access controls. The existing remote catalogue, when populated by the plugin, uses `.relax-agent/topic-skills/catalog.json`; it must not be maintained as a second independent index.

Track source repository, native skill name, immutable revision and content hash. A locally reviewed compatible Relax skill should be considered before a comparable K-Dense recommendation. An incompatible or unreviewed skill cannot win merely because of its source. K-Dense remains a supplementary method source with its own pinned cache and licence attribution.

Marketplaces vary: use a documented provider update mechanism when available; otherwise record the installed version and offer manual or local-only checks. Do not imply that every marketplace has a compatible update API.

## Updates and sharing

1. Discover catalogue metadata without executing remote skill text.
2. Fetch a selected exact revision and inspect its content, provenance, conditions and changes.
3. Review it before selecting or installing it. Retain the previous revision for rollback; existing experiments keep their original pins.
4. Before sharing, remove private details and review the candidate and evidence references. Submit only the necessary files to the explicitly selected repository and branch. Access and merge decisions follow that repository's policy.

Updating the index does not silently update installed instructions. Publishing a revision does not grant access to a private repository or independently reproduce its evidence.

## Event-driven indexing

The planned integration reuses one event dispatcher with handlers for the relevant workflow: terminal experiment review can queue a sanitized candidate; skill approval can update local metadata; catalogue changes can invalidate the cached projection. Handlers use persistent event identities to avoid duplicate effects.

An event hook alone is not a daily scheduler. A 24-hour freshness check can run on the next relevant workflow event and perform a bounded remote metadata check. If no event occurs, it does not run. An always-daily refresh would need an explicitly enabled scheduler, such as GitHub Actions. Neither approach requires model polling. Offline clients retain their last verified cache and show its freshness.

The new retrieval, refresh and workflow bindings are under implementation and require their own validation. This repository bootstrap is not evidence that those integrations or native host discovery have passed.
