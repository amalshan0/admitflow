# Data, model and asset provenance

Owner: **Shan**. The root [MIT license](../LICENSE) applies to project source;
third-party material retains its own terms. No claim is made that MIT relicenses
provider models or dependencies.

| Material | Origin and use | Terms / evidence |
| --- | --- | --- |
| Synthetic development catalogue and synthetic student answers | Authored development examples used in bounded evaluations | Project material; labelled synthetic |
| Current JAIN Kochi public reference catalogue | 64 programme options and campus information discovered through SerpAPI and checked against official public pages; summaries indexed for exploration | Official URLs preserved in [the snapshot](../backend/data/jain-kochi/catalogue.json); public availability is not an open-license grant or admissions authorization; third-party rights retained |
| OpenAI interpretation/reranking | Configured `openai/gpt-4.1-mini`; hosted requests, no downloaded weights or training | Provider service terms; configured model, not a licensing grant |
| OpenAI embeddings | `text-embedding-3-small`, 1536 dimensions | Provider service terms; synthetic live checks |
| Docling layout weights | Offline CPU layout extraction | [Heron model card](https://huggingface.co/docling-project/docling-layout-heron), Apache-2.0 |
| Docling TableFormer bundle | Offline table structure extraction; downloaded model inventory | [Docling models card](https://huggingface.co/docling-project/docling-models); downloaded README declares CDLA-Permissive-2.0; retain upstream notices |
| Python/JavaScript dependencies | Versions locked in backend/uv.lock and frontend/package-lock.json | Each dependency retains its own license |

No external corpus was used to fine-tune a model. Institution uploads must be approved
reference material with permission for the intended use, never applicant personal
files. Model binaries and credentials are excluded from Git. This inventory describes
used assets; it is not a legal assurance about future institution uploads.

Current product context is maintained in PRODUCT/SPEC/STATUS. Public-source retrieval
records are dated snapshots, not a guarantee of current catalogue availability or
authorisation to process university admissions. The separate development sample fee is
not a university fee. Retain original URLs and review dates when refreshing approved data.
