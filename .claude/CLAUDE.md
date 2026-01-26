@/mnt/d/data-science/agentic-ai-data-science-methodology/DSM_Custom_Instructions_v1.1.md

# Quick Reference

## DSM Location
- Central DSM Repository: /mnt/d/data-science/agentic-ai-data-science-methodology/

## Key Paths (in DSM repository)
- Methodology: DSM_1.0_Data_Science_Collaboration_Methodology_v1.1.md
- Appendices: DSM_1.0_Methodology_Appendices.md
- PM Guidelines: DSM_2.0_ProjectManagement_Guidelines_v2_v1.1.md
- SW Engineering: DSM_4.0_Software_Engineering_Adaptation_v1.0.md

## Document References
- Environment Setup: Section 2.1
- Exploration: Section 2.2
- Feature Engineering: Section 2.3
- Analysis: Section 2.4
- Communication: Section 2.5
- NLP Domain: Appendix D.2
- Session Management: Section 6.1

## Project References (lectures/)
- sprint1-summary.md - NLP foundations
- sprint-2 - Guided sentiment project
- sprint-3.md - This project requirements
- masterschool_nlp_llms_01.ipynb - Hands-on NLP
- embedding_demo.ipynb - Embeddings comparison
- milvus_demo.ipynb - Vector database demo
- train.csv - Project dataset

## Project Documentation (docs/)
- plan/DisasterTweets_Sprint3_Plan.md - Project plan
- checkpoints/ - Daily checkpoint files (s03_dXX_checkpoint.md)
- dsm-feedback.md - DSM methodology feedback log

## Development Environment
- Local: VSCode + Jupyter kernel (nlp-llms-kernel)
- Virtual env: .venv (Python 3.10)
- Final deliverable: Must run in Google Colab

## Author
**Alberto Diaz Durana**

## Working Style
- Confirm understanding before proceeding
- Be concise in answers
- Do not generate files before providing description and receiving approval

## Command Execution
- Execute read-only commands (git status, ls, cat, grep, find) without asking
- Show write commands (git commit, git push, rm, mv, pip install) for my approval first

## Plan Mode Protocol
Before implementing any significant feature or change:
1. Thoroughly explore the codebase to understand existing patterns
2. Identify similar features and architectural approaches
3. Consider multiple approaches and their trade-offs
4. Ask clarifying questions if approach is unclear
5. Design a concrete implementation strategy
6. Present plan for user approval before writing/editing any files

## Code Output Standards
- Print statements show actual values (shapes, metrics, counts)
- Avoid generic confirmations: "Complete!", "Done!", "Success!"
- Let results speak: Show df.shape, not "Data loaded successfully!"

## Notebook Development Protocol (for Data Science projects)

When generating notebook cells:
1. Generate ONE cell at a time
2. Wait for user approval OR execution output before generating next cell
3. Never generate multiple cells without explicit request
4. Number each cell with a comment (e.g., `# Cell 1`, `# Cell 2`)
