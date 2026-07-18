# Clinical Ann Tag

Clinical NER annotation tool for Spanish-language oncology text, and its
entity-recognition API (XLM-RoBERTa). Published as part of a thesis / academic
paper.

<!-- TODO: link to the paper once published -->

## Requirements

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose (bundled
  with a recent Docker Desktop / Docker Engine).
- A [Hugging Face](https://huggingface.co) account with approved access to
  the `anvorja/panoncology-biomedical-ner-sp` model (see below).

## Get model access

The NER model is **gated** on Hugging Face Hub: it's publicly visible, but
downloading the weights requires approved access and a personal token.

1. Go to Hugging Face: [anvorja/panoncology-biomedical-ner-sp](https://huggingface.co/anvorja/panoncology-biomedical-ner-sp)
2. Request access to the model by clicking **"Request access"** (or "Agree
   and access repository"). Access is approved by the model's author.
3. Once approved, generate your personal token in **Settings > Access
   Tokens**:
   - Type: **Fine-grained**
   - Repository: `anvorja/panoncology-biomedical-ner-sp`
   - Permission: **Read** only — never Write.

Full token setup instructions (including security notes) are in
[`secrets/README.md`](secrets/README.md).

## Quick start

```bash
git clone <url-of-this-repo>
cd <this-repo>

# 1. Set up your Hugging Face token (one time)
printf '%s' 'hf_YOUR_PERSONAL_TOKEN' > secrets/hf_token.txt
chmod 600 secrets/hf_token.txt

# 2. Launch the stack — pulls the published images, builds nothing
docker compose up
```

- Frontend: http://localhost:3000
- API: http://localhost:8000 (`/api/v1/health` to check status)

Optional environment variables are documented in
[`.env.example`](.env.example).

## Published images

- [`clinicalanntag/clinical-ann-tag-api`](https://hub.docker.com/r/clinicalanntag/clinical-ann-tag-api) — FastAPI backend + NER model
- [`clinicalanntag/clinical-ann-tag-ui`](https://hub.docker.com/r/clinicalanntag/clinical-ann-tag-ui) — Next.js frontend

## License

<!-- TODO -->
