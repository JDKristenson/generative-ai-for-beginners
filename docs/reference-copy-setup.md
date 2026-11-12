# Setting up this reference copy

This repository preserves Microsoft's **Generative AI for Beginners** curriculum and its upstream samples. It is not one independently authored application with a single startup command. Keep the upstream authorship, credits and [MIT license notice](../LICENSE?WT.mc_id=academic-105485-koreyst) with copies or substantial portions of the material. See the [upstream project](https://github.com/microsoft/generative-ai-for-beginners?WT.mc_id=academic-105485-koreyst), the [course setup lesson](../00-course-setup/README.md?WT.mc_id=academic-105485-koreyst), and [contribution instructions](../CONTRIBUTING.md?WT.mc_id=academic-105485-koreyst).

## Scope and verification status

**Every setup, build, dependency-check and launch command below is inferred from source and NOT RUN for this guide.** Reading manifests and notebook cells establishes intent, not a working installation, successful model response or end-to-end verification. No dependencies were installed and no curriculum code, build, test suite or provider API was executed to prepare this document.

Choose one lesson and provider before installing anything. Commands below assume a POSIX shell and start at the repository root unless noted. On Windows, adapt shell syntax and use the virtual environment's Windows interpreter path. No universal Node.js runtime requirement is declared by the inspected package manifests; select a runtime compatible with the selected sample and its dependency versions. The checked-in Conda environment specifies Python 3.10.0, while the representative notebook records Python 3.11.9; neither is proof of compatibility for every lesson.

## Three different setup surfaces

- **Root documentation tooling:** the [root package manifest](../package.json?WT.mc_id=academic-105485-koreyst) declares only a `convert` script, not root `build`, `start` or `test` scripts. Although it also lists AI SDK dependencies, installing it does not install all nested TypeScript projects or make the curriculum one app. The [Docsify entry page](../index.html?WT.mc_id=academic-105485-koreyst) renders documentation and loads scripts/styles from a CDN.
- **Python lessons and notebooks:** the [root Python requirements](../requirements.txt?WT.mc_id=academic-105485-koreyst) are a shared dependency list, not a universal executable or a lockfile. Lessons can have their own requirements and different providers. A notebook's saved outputs are not a fresh check.
- **TypeScript samples:** individual lesson folders have their own `package.json`, lockfile, compiler configuration and entry point. Install and check inside the selected sample, not just at the root.

The [devcontainer configuration](../.devcontainer/devcontainer.json?WT.mc_id=academic-105485-koreyst) runs dependency-install hooks; opening it is not a read-only operation. Inspect its hooks and the [Conda environment](../.devcontainer/environment.yml?WT.mc_id=academic-105485-koreyst) before using that route. The local paths below avoid relying on those hooks.

## Optional: root documentation-to-PDF exporter

The [exporter configuration](../docsifytopdf.js?WT.mc_id=academic-105485-koreyst) takes `docs/_sidebar.md` as its contents source and targets `pdf/readme.pdf`. This is documentation export, not a curriculum build or sample test.

```bash
# Inferred setup/export commands — NOT RUN.
# From the repository root; review package installation scripts first.
npm ci
npm run convert
```

Installation/export can require network access and browser tooling used by `docsify-to-pdf`. PDF generation, browser availability and output correctness remain unverified. Do not run this exporter as a substitute for checking a lesson's own code.

## Python example: lesson 06 OpenAI notebook

Read the [lesson](../06-text-generation-apps/README.md?WT.mc_id=academic-105485-koreyst) and the actual [OpenAI assignment notebook](../06-text-generation-apps/python/oai-assigment.ipynb?WT.mc_id=academic-105485-koreyst) first; `oai-assigment.ipynb` is the checked-in filename. Its executable cells import `OpenAI` and `load_dotenv`, read `OPENAI_API_KEY`, and call `client.chat.completions.create` with `gpt-3.5-turbo`. Some prose uses a different key name: follow the executable cell's `OPENAI_API_KEY`. Model availability and current account access must be checked separately.

Use a dedicated environment outside the checkout. The [lesson-specific requirements](../06-text-generation-apps/python/requirements.txt?WT.mc_id=academic-105485-koreyst) pin `openai==1.55.1` and `python-dotenv==1.0.1`; use those for this OpenAI example rather than assuming that the broader root requirements fit every sample. Jupyter and its kernel tooling are additional local authoring tools, not declared by that lesson manifest.

```bash
# Inferred setup commands — NOT RUN. Start at the repository root.
python3 -m venv "$HOME/.venvs/genai-lesson06-openai"
. "$HOME/.venvs/genai-lesson06-openai/bin/activate"
python -m pip install -r 06-text-generation-apps/python/requirements.txt
python -m pip install notebook ipykernel

# Inferred local dependency check — NOT RUN; does not call a model.
python -m pip check
```

After setup, select this environment's Python interpreter as the notebook kernel in your editor. Alternatively, launch Jupyter from the activated environment:

```bash
# Inferred notebook-editor launch — NOT RUN. This does not itself run cells.
python -m notebook 06-text-generation-apps/python/oai-assigment.ipynb
```

Do not use **Run All** as a smoke test. The notebook includes shell installation cells and several live model requests, including interactive input. Its `! source ...` activation runs in a shell subprocess and should not be relied on to switch the notebook's active kernel. With the environment already prepared, skip those setup cells, verify the selected kernel, and review each model-call cell before deliberately executing it.

The [GitHub Models variant](../06-text-generation-apps/python/githubmodels-assignment.ipynb?WT.mc_id=academic-105485-koreyst) is a separate provider path using the Azure AI Inference SDK; the two-package OpenAI lesson requirements alone are not a complete setup for that variant. Do not reuse an OpenAI credential or infer a token permission policy from this guide.

## TypeScript examples: lessons 06 and 07

The lesson 06 recipe app has its own [manifest](../06-text-generation-apps/typescript/recipe-app/package.json?WT.mc_id=academic-105485-koreyst), [compiler configuration](../06-text-generation-apps/typescript/recipe-app/tsconfig.json?WT.mc_id=academic-105485-koreyst), and [source entry point](../06-text-generation-apps/typescript/recipe-app/src/main.ts?WT.mc_id=academic-105485-koreyst).

```bash
# Inferred sample-local setup/check commands — NOT RUN.
cd 06-text-generation-apps/typescript/recipe-app
npm ci
npm exec -- tsc --noEmit
# Optional emitted build, separately from the no-output type check:
npm run build
```

The build script is `rimraf build && tsc`, but `tsconfig.json` directs compiler output to `dist`. Do not assume the cleanup command removes prior `dist` output. The manifest's `preserve` script is not a `prestart` hook: run the desired check/build explicitly. No test script is declared in this sample.

The lesson 07 chat app is another package with its own [manifest](../07-building-chat-applications/typescript/chat-completions-app/package.json?WT.mc_id=academic-105485-koreyst) and [source](../07-building-chat-applications/typescript/chat-completions-app/src/main.ts?WT.mc_id=academic-105485-koreyst). In a new shell at the repository root, the analogous checks are:

```bash
# Inferred sample-local setup/check commands — NOT RUN.
cd 07-building-chat-applications/typescript/chat-completions-app
npm ci
npm exec -- tsc --noEmit
npm run build
```

Both entry points call `main()` immediately. They load `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_API_KEY` through `dotenv`, and contain the literal deployment placeholder `deployment-name-completion`. Configure a deployment you are authorized to use and replace that placeholder in your local working copy before any live run; adding an arbitrary deployment environment variable will not change these source literals.

Only after reviewing the cost/data boundary below, the deliberate live-run command in either sample directory would be:

```bash
# Inferred LIVE provider run — NOT RUN. Not a local-only validation.
npm start
```

`npm start` uses nodemon to execute `src/main.ts` through ts-node. File changes can trigger another execution and another request. The recipe app makes two completion requests on its successful path; the chat example makes one. Both catch and log errors, so process output or a zero exit status alone is not proof of a successful provider response. Compiler compatibility, package installation, deployment access and responses are all unverified here.

## Credentials, cost and the data boundary

1. **Separate local checks from external execution.** Reading files, checking notebook JSON, `pip check` and `tsc --noEmit` do not establish inference success. Package installation downloads third-party code and may run lifecycle scripts. Executing a notebook cell or a sample entry point can send prompts and credentials to a provider, consume quota and incur charges. Check current provider terms, model availability, pricing, spending limits and resource ownership first; a historical “free to try” statement is not a guarantee.
2. **Keep credentials out of the reference copy.** Prefer a secret manager or narrowly scoped process environment. If using a local `.env`, keep it untracked, verify it is ignored with `git check-ignore`, and do not paste credentials into source, notebook cells, saved outputs, screenshots, issue reports or logs. Never print a token to confirm setup. Review which `.env` file `dotenv` will discover for the chosen working directory.
3. **Use synthetic, non-sensitive inputs.** Prompts, conversation history, retrieved documents, embeddings and uploaded images may cross into external services depending on the lesson. Confirm permission, provider retention terms and the minimum necessary data before sending anything. Do not use customer, personal, confidential or proprietary data for a tutorial smoke test.
4. **Review downstream actions separately.** Search/RAG lessons can download or index datasets, image lessons can generate/store assets, and function-calling lessons can invoke other tools or services. A configuration that works for lesson 06 does not authorize those operations or establish their safety.
5. **Keep environments independent.** Do not install all lesson requirements into a global Python environment or mix provider credentials by default. Record the selected lesson, interpreter/runtime, resolved dependency versions, local checks and any deliberately authorized live call separately. Delete or redact sensitive notebook outputs before sharing.

## Contributing and recording actual checks

Preserve upstream credits, governance and license text. Follow [CONTRIBUTING.md](../CONTRIBUTING.md?WT.mc_id=academic-105485-koreyst), including relative links beginning with `./` or `../`, required tracking queries, and the upstream CLA process where applicable. This supplemental guide does not replace lesson instructions or change their authorship.

When you actually check a sample, record its exact path, command, runtime and decisive result. Distinguish **source inspected**, **dependencies installed**, **local checks passed**, and **live provider behavior observed**. For this guide, only source inspection and document checks have been performed; all illustrative installation, export, type-check, build and launch commands remain **NOT RUN**.
