### 1. Is GitHub Copilot free to use for everyone?

https://github.com/features/copilot/plans

- [ ] No
- [x] Yes

### 2. Which option below is NOT a possible way to grant access to Copilot for members of an organization?

Article Grant access to Copilot for members of an organization -https://docs.github.com/ja/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-access-to-github-copilot-in-your-organization/granting-access-to-copilot-for-members-of-your-organization

- [x] As a member of an Organization, you can enable Copilot directly from your account settings.
- [ ] Via your Enterprise settings, enable GitHub Copilot for selected organizations or all organizations.
- [ ] Via your Organizations settings, enable GitHub Copilot for either selected teams or users or the entire organization.
- [ ] You can use GitHub's REST API to grant access to GitHub Copilot for teams, or specific users, in your organization.

### 3. What IDEs does GitHub Copilot support? (Choose two.)

https://docs.github.com/ja/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot

- [x] Visual Studio Code, Xcode, Vim/NeoVim
- [x] Azure Data Studio, Visual Studio, IntelliJ IDEA
- [ ] Visual Studio, NetBeans, Eclipse
- [ ] Visual Studio, BlueJ, NetBeans

### 4. What command is used to install the GitHub Copilot extension on the CLI?

https://docs.github.com/ja/copilot/managing-copilot/configure-personal-settings/installing-github-copilot-in-the-cli

- [x] gh extension install github/gh-copilot
- [ ] gh copilot install
- [ ] gh copilot setup
- [ ] gh extension add copilot

### 5. What are some of the principles of Prompt Engineering? (Choose three.)

https://docs.github.com/ja/copilot/using-github-copilot/prompt-engineering-for-github-copilot

- [x] Focus on a single, well-defined task
- [x] Ensure instructions are detailed and explicit
- [x] Provide rich context for AI
- [ ] Write long, complex instructions
      If you want Copilot to complete a complex or large task, break the task into multiple simple, small tasks.

### 6. How can you exclude specific files from GitHub Copilot?

https://docs.github.com/ja/copilot/managing-copilot/configuring-and-auditing-content-exclusion/excluding-content-from-github-copilot

- [ ] Editing the file .gitignore
      .gitignore is used to exclude the file from git, not copilot
- [x] Browsing to the repository settings on GitHub and adding the paths to exclude
- [ ] Configuring exclusions in the Copilot configuration file
- [ ] Using a command in the terminal

### 7. Which is true about Copilot's Content exclusions? (Choose two)

https://docs.github.com/ja/copilot/managing-copilot/configuring-and-auditing-content-exclusion/excluding-content-from-github-copilot

- [x] Context exclusions can be configured at the repository and organization level
- [x] Copilot offers different plans with privacy considerations
- [ ] Copilot completely ignores excluded files
      Copilot may use information from an excluded file if the information is provided by the IDE.
- [ ] Content exclusions do not affect code completion
- [ ] Content exclusions are applied instantly
      After you add or change content exclusions, it can take up to 30 minutes to take effect

### 8. Which of the following describes the GitHub Copilot Editor configuration file?

https://docs.github.com/ja/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot

- [ ] A JSON file with security settings
- [x] A Markdown file with natural language instructions for customizing Copilot Chat responses
- [ ] A YAML file with build instructions
- [ ] An XML file with deploy settings

### 9. Which of the following describes how to use the GitHub Copilot's Productivity API?

https://docs.github.com/ja/copilot/rolling-out-github-copilot-at-scale/analyzing-usage-over-time-with-the-copilot-metrics-api

- [ ] To collect audit logs
- [ ] To exclude specific files
- [x] To collect usage metrics from organization members
- [ ] To automatically update Copilot

### 10. Which of the following integrates GitHub Copilot Chat with external tools?

https://docs.github.com/ja/enterprise-cloud@latest/copilot/using-github-copilot/using-extensions-to-integrate-external-tools-with-copilot-chat

- [x] GitHub Copilot Extensions
- [ ] GitHub Copilot Marketplace
- [ ] GitHub Copilot Integrations
- [ ] GitHub Copilot Open

### 11. How can you provide GitHub Copilot with context to generate tailored responses for your repository?

https://docs.github.com/ja/enterprise-cloud@latest/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot

- [x] By creating a file named `.github/copilot-instructions.md` in the repository
- [ ] By sending an email to GitHub support with your project details
- [ ] By modifying the `.gitconfig` file to include custom instructions
      Modifying the `.gitconfig` file does not provide custom instructions to GitHub Copilot.
- [ ] By creating a GitHub issue named `copilot-instructions` in the repository with the necessary context
      Creating a GitHub issue does not provide custom instructions to GitHub Copilot.

### 12. Can GitHub Copilot use semantic information from a file that is ignored by GitHub Copilot content exclusions?

https://docs.github.com/ja/copilot/managing-copilot/configuring-and-auditing-content-exclusion/excluding-content-from-github-copilot#limitations-of-content-exclusions

- [x] Yes, if the information is provided by the IDE indirectly.
- [ ] No, it will ignore all information from excluded files.
      It's possible that Copilot may use semantic information from an excluded file if the information is provided by the IDE indirectly. Examples of such content include type information and hover-over definitions for symbols used in code, as well as general project properties such as build configuration information.

### 13. What happens when you exclude content from GitHub Copilot? (Choose two)

https://docs.github.com/ja/copilot/managing-copilot/configuring-and-auditing-content-exclusion/excluding-content-from-github-copilot#about-content-exclusions-for-copilot

- [x] Code completion will not be available in the affected files.
- [x] The content in affected files will not inform code completion suggestions in other files.
- [ ] The content in affected files will continue to inform GitHub Copilot Chat's responses.
- [ ] Code completion will be unaffected in the affected files.

### 14. What is the easiest way to get started with GitHub Copilot?

https://docs.github.com/ja/copilot/using-github-copilot/getting-started-with-github-copilot

- [ ] Request access from GitHub Support and wait for approval before using GitHub Copilot.
- [ ] Use the Copilot website and paste your code when asking for suggestions.
- [x] Install the Copilot extension in your preferred environment, such as Visual Studio Code.
- [ ] Create a new public GitHub repo and enable Copilot to scan your code and make suggestions.

### 15. What does GitHub Copilot analyze to offer relevant suggestions as you are developing new code?

https://docs.github.com/ja/copilot/using-github-copilot/best-practices-for-using-github-copilot#guide-copilot-towards-helpful-outputs

- [ ] Analyzes the context in all files within the repository.
- [x] Analyzes the context in the current file and related files.
- [ ] Analyzes only the context within the current file.
- [ ] Analyzes only the context within the current line of code.

### 16. Which of the following options best describes GitHub Copilot?

https://docs.github.com/ja/copilot/about-github-copilot/what-is-github-copilot

- [x] An AI coding assistant that helps developers by suggesting code and completing code snippets.
- [ ] A version control system that tracks and manages changes in a codebase.
- [ ] A code editor that provides debugging and error-checking features.
- [ ] A tool that automatically tests and deploys code to production environments.

### 17. How does GitHub Copilot handle data retention for code suggestions in the IDE?

https://resources.github.com/learn/pathways/copilot/essentials/how-github-copilot-handles-data/

- [x] Suggestions are held temporarily in memory and discarded after use, not written to disk
- [ ] All suggestions are permanently stored in a local database for future reference
- [ ] Suggestions are automatically saved to GitHub repositories for version control
- [ ] Code snippets are cached on disk for 30 days before being deleted

### 18. Which steps occur when GitHub Copilot's proxy service processes a prompt?

https://resources.github.com/learn/pathways/copilot/essentials/how-github-copilot-handles-data/

- [x] Tests for toxic language, relevance checks, and detection of prompt hacking attempts
- [ ] Translation to multiple programming languages and syntax validation
- [ ] Automatic code compilation and execution in a sandbox environment
- [ ] Direct transmission to public repositories for reference checking

### 19. Which set of principles correctly represents Microsoft's six key principles for Responsible AI that guide GitHub Copilot's development?

https://learn.microsoft.com/en-us/training/modules/responsible-ai-with-github-copilot/3-six-principles-of-responsible-ai

- [x] Fairness, Reliability and Safety, Privacy and Security, Inclusiveness, Transparency, and Accountability
- [ ] Efficiency, Speed, Accuracy, Innovation, Reliability, and Security
- [ ] Privacy, Performance, Accessibility, Scalability, Maintainability, and Testing
- [ ] Security, Development, Operations, Maintenance, Support, and Documentation

### 20. Which of the following is a potential benefit of using GitHub Copilot to enhance developer workflows?

https://docs.github.com/ja/copilot

- [x] It can suggest code snippets to increase developer productivity.
- [ ] It completely replaces the need for code review in every project.
- [ ] It automatically merges pull requests without human approval.
- [ ] It only works with software written in a single programming language.

### 21. Which statement correctly describes GitHub Copilot's CLI command functionality?

https://docs.github.com/ja/copilot/using-github-copilot/using-github-copilot-in-the-command-line

- [x] Users can get command explanations using 'gh copilot explain' and command suggestions using 'gh copilot suggest'
- [ ] Commands are automatically executed without user confirmation when using 'gh copilot suggest'
- [ ] The 'gh copilot explain' command modifies system files without showing the explanation
- [ ] Suggested commands are directly executed without being copied to the clipboard first

### 22. What is the primary purpose of the '/tests' slash command in GitHub Copilot?

https://docs.github.com/ja/copilot/using-github-copilot/guides-on-using-github-copilot/writing-tests-with-github-copilot

- [x] It generates a suite of unit tests for the currently open file, using context from existing test files if available
- [ ] It runs all existing unit tests in the project without generating new ones
- [ ] It only validates the syntax of existing test files without creating new tests
- [ ] It permanently removes all existing test files to start fresh

### 23. How is seat usage calculated for GitHub Copilot at the enterprise level during a billing cycle?

https://docs.github.com/ja/enterprise-cloud@latest/copilot/managing-copilot/managing-copilot-for-your-enterprise/managing-access-to-copilot-in-your-enterprise/viewing-copilot-license-usage-in-your-enterprise

- [x] Number of seats × (Days elapsed / Total days in billing cycle)
- [ ] Total number of commits × Number of active developers
- [ ] Number of code suggestions × Number of accepted completions
- [ ] Total repository size × Number of organizations

### 24. How does GitHub Copilot's matching public code feature work?

https://docs.github.com/ja/copilot/using-github-copilot/finding-public-code-that-matches-github-copilot-suggestions

- [x] It searches for matches by comparing code suggestions against an index of public GitHub repositories, which is refreshed every few months
- [ ] It performs real-time searches across all GitHub repositories, including private ones
- [ ] It only matches code from repositories that were created in the last 24 hours
- [ ] It checks code against external code hosting platforms outside of GitHub

---------------------↑ まで完了------------------------------

### 25. What are the post-processing checks performed on GitHub Copilot's responses?

https://resources.github.com/learn/pathways/copilot/essentials/how-github-copilot-handles-data/

- [x] Toxic language, relevancy, code quality (including security vulnerabilities), unique identifiers, and optional public code matching
- [ ] Only syntax validation and code formatting checks
- [ ] Performance benchmarking and memory usage optimization
- [ ] Just checking for compilation errors and runtime exceptions

### 26. Which elements can GitHub Copilot use as context when generating suggestions?

https://docs.github.com/ja/copilot/using-github-copilot/getting-started-with-github-copilot#using-context-in-github-copilot

- [x] Content in the current file, neighboring files, repository URLs, file paths, and previous chat interactions
- [ ] Only the current line of code being edited without any surrounding context
- [ ] Exclusively external documentation from the internet
- [ ] Just the project's README file and nothing else
