# Hugging Face token for `docker compose up`

The backend uses the NER model `anvorja/panoncology-biomedical-ner-sp`, which
is **gated** on Hugging Face Hub: it's publicly visible, but downloading the
weights requires approved access and authentication. Everyone who runs this
`docker-compose.yml` needs their **own** read-only token.

## Steps

1. Create an account at https://huggingface.co if you don't have one.
2. Request access to the model: https://huggingface.co/anvorja/panoncology-biomedical-ner-sp
   ("Request access" or "Agree and access repository" button). Access is
   approved by the model's author.
3. Once approved, create a token at https://huggingface.co/settings/tokens:
   - Type: **Fine-grained**
   - Repository: `anvorja/panoncology-biomedical-ner-sp`
   - Permission: **Read** (repository contents) only — never Write.
4. Save the token in this file (create it, it's not versioned):

   ```bash
   printf '%s' 'hf_YOUR_PERSONAL_TOKEN' > secrets/hf_token.txt
   chmod 600 secrets/hf_token.txt
   ```

5. Launch the stack (pulls the published images, builds nothing):

   ```bash
   docker compose up
   ```

## Security notes

- `secrets/hf_token.txt` must never be shared, committed, or pushed to any
  repository — it's in `.gitignore`.
- Docker mounts the file as a *secret* at `/run/secrets/hf_token` inside the
  backend container. It's never an environment variable, so it never shows
  up in `docker inspect` or logs.
- A read-only (Read) token doesn't allow pushing or administering the
  model's repository, but it does let you download and keep a local copy of
  the weights — like with any gated model download.
- The token is never baked into the Docker image: `clinicalanntag/clinical-ann-tag-api`
  on Docker Hub doesn't contain any secret.
