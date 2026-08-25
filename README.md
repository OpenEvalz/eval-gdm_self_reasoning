# eval-gdm_self_reasoning

**GDM Dangerous Capabilities: Self-reasoning**

**Paper:** https://arxiv.org/abs/2505.01420

Test AI's ability to reason about its environment.

## At a glance

| | |
|---|---|
| Upstream | [`src/inspect_evals/gdm_self_reasoning`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/gdm_self_reasoning) |
| Group | Scheming |
| Total samples | 22 |
| Execution class | `sandbox-k8s` |
| Cost class | `high` |
| Flags | k8s-ready · sandboxed |
| Tags | Agent |

### Tasks

| Task | Samples |
|---|---|
| `gdm_self_reasoning_approved_directories` | 2 |
| `gdm_self_reasoning_calculator_improvement` | 2 |
| `gdm_self_reasoning_context_length_mod_instrumental_only` | 2 |
| `gdm_self_reasoning_context_length_mod_irreversibility_only` | 2 |
| `gdm_self_reasoning_database_tool` | 2 |
| `gdm_self_reasoning_latency_calculator` | 2 |
| `gdm_self_reasoning_max_messages_calculator` | 2 |
| `gdm_self_reasoning_max_tokens` | 2 |
| `gdm_self_reasoning_oversight_frequency` | 2 |
| `gdm_self_reasoning_read_logs` | 2 |
| `gdm_self_reasoning_turn_off_filters` | 2 |

### External assets

_None declared upstream._

This eval declares `supports_k8s: true` and is one of only six in inspect_evals that do. `k8s/values.yaml` holds the sandbox profile and egress allowlist.

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/gdm_self_reasoning_approved_directories \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
