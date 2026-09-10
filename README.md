
Claude finished the response
Thought for 2s
Drafting step-by-step setup instructions for a repository.
Thought for 2s
1
Open your profile repo
Go to github.com/suryansh173/suryansh173 (create this repo first if it doesn't exist yet — name it exactly your username, keep it public, and check 'Add a README file'). This special repo is what GitHub renders on your profile page.
2
Create the workflow file
Click 'Add file' → 'Create new file'. In the filename box, type .github/workflows/github-stats.yml (typing the slashes auto-creates the folders). Paste in the full content of the github-stats.yml file I generated, then click 'Commit changes' at the bottom.
3
Generate a Personal Access Token
Go to github.com/settings/tokens → click 'Generate new token' → 'Generate new token (classic)'. Give it a name like 'stats-token', set an expiration, and check the boxes for 'repo' and 'read:user' scopes. Click 'Generate token' and copy the value immediately — you won't be able to see it again.
4
Add the token as a repo secret
In your suryansh173/suryansh173 repo, go to Settings → Secrets and variables → Actions → 'New repository secret'. Set the name to exactly ACCESS_TOKEN (case-sensitive) and paste the token you copied as the value. Click 'Add secret'.
5
Run the workflow manually
Go to the 'Actions' tab in your repo. Click on 'Generate GitHub Stats SVGs' in the left sidebar, then click the 'Run workflow' dropdown button on the right and confirm. Wait 1-2 minutes for both jobs (generate-stats and generate-snake) to finish — you'll see green checkmarks when done.
6
Check the generated files
After the run finishes, refresh your repo's main page — you should now see a 'generated' folder containing overview.svg and languages.svg. Also check the branch dropdown for a new 'output' branch, which holds the contribution snake SVG.
7
Replace your README
Open README.md in your repo, click the pencil icon to edit, delete everything, and paste in the updated README.md content I generated (the one pointing to raw.githubusercontent.com URLs). Commit the change.
8
Verify it's live
Go to github.com/suryansh173 (your profile page) and hard-refresh (Ctrl+Shift+R). You should now see your stats overview, top languages, streak, and contribution snake all rendering from your own repo instead of the shared public servers.
A couple of things worth flagging before you start:

Token expiration: if you set an expiry on the token (step 3), the workflow will silently stop updating once it expires — you'll need to regenerate it and update the secret. A "no expiration" or long expiry token avoids that maintenance.
First run timing: the schedule only kicks in going forward — you have to trigger it manually the first time (step 5) or the SVGs won't exist yet.
Branch name: the snake animation lands on a branch called output, not main — that's expected, not an error.
If anything shows a red ❌ in the Actions tab instead of green, send me a screenshot of the error log and I'll help debug it.

1789058474102_image.png
Thought for 4s
Diagnosing why the stats image workflow failed quickly.
Thought for 4s
That red ❌ on "Generate stats images" finishing in just 4 seconds usually means it failed right at authentication — most commonly because the ACCESS_TOKEN secret is missing, misnamed, or the token itself is invalid/expired.

Could you open that failed job and share what's inside? Click on "Generate stats images" in the Actions run, then expand the step that has the red X — the error message will tell us exactly what's wrong. A screenshot of that expanded log would help me pinpoint it.

In the meantime, here's a quick checklist to self-check:

1
Confirm the secret name matches exactly
Go to Settings → Secrets and variables → Actions in your repo. The secret must be named exactly ACCESS_TOKEN (all caps, underscore, no typos). If it's named anything else, the workflow won't find it.
2
Confirm the token wasn't left empty or truncated
When pasting the token value, make sure there's no extra space before/after it and that the full string was copied (GitHub tokens are usually 40+ characters starting with ghp_).
3
Confirm the token has the right scopes
Go back to github.com/settings/tokens, find your token, and verify it has both repo and read:user checked. If you're unsure, it's safest to just delete it and generate a fresh one with both boxes checked.
4
Regenerate the secret if in doubt
If anything looks off, delete the old ACCESS_TOKEN secret and re-add it with a freshly copied token value, then re-run the workflow from the Actions tab.
Once you've checked those (or if they all look correct), paste me the actual error text from the expanded log and I'll tell you exactly what to fix.

1789058518916_image.png
Searched the web
Found it — that error makes sense now. jstrieb/github-stats isn't a reusable GitHub Action (no action.yml), so uses: jstrieb/github-stats@master can't work the way I wrote it. It's meant to be forked entirely — its own internal workflow checks out the code and runs a Python script directly. My mistake for treating it as a plug-in action.

Good news: your snake job passed fine, so that part's done. Let's fix the stats images properly — this is actually the simpler fix.

1
Fork jstrieb/github-stats
Go to github.com/jstrieb/github-stats and click the 'Fork' button (top right). This creates a full copy at github.com/suryansh173/github-stats — a separate repo from your profile repo, with its own working Actions workflow already inside it.
2
Add your ACCESS_TOKEN to the fork
In your new suryansh173/github-stats fork, go to Settings → Secrets and variables → Actions → New repository secret. Name it ACCESS_TOKEN and paste the same personal access token you generated earlier (repo + read:user scopes).
3
Run the workflow in the fork
In the fork, go to the Actions tab, click on the workflow listed there, and click 'Run workflow'. Wait a minute or two for it to finish with a green checkmark.
4
Check the generated branch
After it finishes, use the branch dropdown in your suryansh173/github-stats fork and switch to the 'generated' branch. You should see overview.svg and languages.svg sitting in the repo root.
5
Update your profile README
Edit your suryansh173/suryansh173 README.md and point the two stats image tags at raw.githubusercontent.com/suryansh173/github-stats/generated/overview.svg and .../generated/languages.svg instead of the old paths. I'll give you the exact lines below — just paste them in.
Now let me clean up the workflow file in your profile repo — remove the broken generate-stats job since that's handled by the fork now, keeping just the working snake job:

Edited 2 files, ran a command, shared files
To wrap up:

Replace .github/workflows/github-stats.yml in your suryansh173/suryansh173 repo with this trimmed version (snake-only, still works fine).
Replace your README.md with the updated one — image URLs now point to the fork instead of the profile repo.
Fork jstrieb/github-stats and follow the 5 steps above to get overview.svg and languages.svg generating there.
Once the fork's workflow runs successfully and lands on its generated branch, everything on your profile — overview, languages, streak, and snake — will be self-hosted and rate-limit-proof. Ping me with a screenshot if the fork's workflow throws anything unexpected.


Github stats
YML 

Readme
Document·MD 

Write a message…


Claude is AI and can make mistakes. Please double-check cited sources.


Readme · MD
<h1 align="center">Hi 👋, I'm Suryansh Pratap Singh</h1>
<h3 align="center">Machine Learning & Deep Learning Engineer | Transformer-based NLP • RAG • Remote Sensing</h3>
 
<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=20&duration=3000&pause=1000&color=2E9EF7&center=true&vCenter=true&width=600&lines=Building+RAG+pipelines+from+scratch;Fine-tuning+Transformers+with+PyTorch;Training+GPT-style+models+on+TinyStories;Open+to+ML%2FAI+Engineering+roles" alt="Typing SVG" />
</p>
<p align="center">
  <a href="https://www.linkedin.com/in/suryans173a37464288/">
    <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" />
  </a>
  <a href="https://huggingface.co/suryansh173">
    <img src="https://img.shields.io/badge/HuggingFace-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black" />
  </a>
  <a href="https://leetcode.com/u/suryansh173/">
    <img src="https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=leetcode&logoColor=black" />
  </a>
  <a href="https://suryansh173.github.io/suryansh.github.io/">
    <img src="https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white" />
  </a>
  <a href="mailto:suryanshpratapsingh528@gmail.com">
    <img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" />
  </a>
</p>
---
 
### 🚀 About Me
 
- 🎓 B.Tech CSE, Jaypee University of Engineering and Technology (2022 – 2026)
- 🔬 Research Intern (HPC) @ Ramanujan Universe, JIIT Noida — built an Autoencoder–PCA–KMeans pipeline for NDVI crop clustering (87.5% ARI)
- 🧠 Building end-to-end ML systems: RAG pipelines, fine-tuned BERT sentiment models, and GPT-style transformers from scratch
- ☁️ Comfortable deploying on AWS (EC2, S3, IAM), with FastAPI/React frontends
- 📫 Reach me at **suryanshpratapsingh528@gmail.com**
---
 
### 🛠️ Tech Stack
 
**Languages**
<p>
<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white" />
<img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" />
</p>
**Machine Learning & AI**
<p>
<img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white" />
<img src="https://img.shields.io/badge/Scikit--Learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white" />
<img src="https://img.shields.io/badge/Transformers-FFD21E?style=flat-square&logo=huggingface&logoColor=black" />
<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white" />
<img src="https://img.shields.io/badge/ChromaDB-4B0082?style=flat-square" />
</p>
**Data & Backend**
<p>
<img src="https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white" />
<img src="https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white" />
<img src="https://img.shields.io/badge/Matplotlib-11557C?style=flat-square" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" />
<img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" />
</p>
**Cloud, Web & Tools**
<p>
<img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonaws&logoColor=white" />
<img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" />
<img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white" />
<img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white" />
<img src="https://img.shields.io/badge/VS%20Code-007ACC?style=flat-square&logo=visualstudiocode&logoColor=white" />
</p>
---
 
### 📊 GitHub Stats
 
<p align="center">
  <img src="https://raw.githubusercontent.com/suryansh173/github-stats/generated/overview.svg#gh-dark-mode-only" />
  <img src="https://raw.githubusercontent.com/suryansh173/github-stats/generated/languages.svg#gh-dark-mode-only" />
</p>
<p align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=suryansh173&theme=tokyonight&hide_border=true" alt="GitHub Streak" />
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/suryansh173/suryansh173/output/github-contribution-grid-snake-dark.svg#gh-dark-mode-only" alt="Contribution Snake" />
</p>
---
 
### 🏆 Featured Projects
 
| Project | Description | Tech |
|---|---|---|
| **[RAG-Based Document Query System](https://github.com/suryansh173)** | End-to-end RAG pipeline built from scratch (no LangChain) — PyMuPDF, ChromaDB, Groq-hosted LLaMA 3.1, deployed on AWS EC2 + React/Vercel | Python, FastAPI, ChromaDB, AWS |
| **[BERT Sentiment Analysis Web App](https://huggingface.co/suryansh173)** | Fine-tuned BERT on 1.6M tweets (Sentiment140), >85% accuracy, deployed on Hugging Face Hub with Streamlit UI | PyTorch, Transformers, Streamlit |
| **GPT-Style Transformer for Story Generation** | GPT-style transformer built from scratch in PyTorch, trained on TinyStories (~2.1M stories), ~30% faster training via mixed precision | PyTorch, tiktoken, NumPy |
 
---
 
<p align="center">
  <img src="https://komarev.com/ghpvc/?username=suryansh173&style=flat-square&color=blue" alt="Profile views" />
</p>
 

