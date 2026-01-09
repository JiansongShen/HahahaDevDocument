# How to Contribute

Thanks for your interest in the Hahaha project! We welcome all kinds of contributions—code, documentation, tests, and design suggestions. This guide helps you get started quickly.

## Before you contribute

### 1. Understand the project

Before starting, please:

- Read the [Project Introduction](../hahaha/introduce.md) to understand goals and architecture
- Check the [Roadmap](../hahaha/roadmap.md) to understand planning
- Read the [Tech Stack](../hahaha/tech-stack.md) to understand tools and choices

### 2. Pick a task

Find a task you’re interested in from `doc/TODO.md` in the main project repository:

```markdown
- [ ] Implement optimizer (help wanted)
```

Once you decide to take it, add your info:

```markdown
- [/] Implement optimizer - [Your Name](Your GitHub link) (your email)
```

## Development environment

### System requirements

- **OS**: Linux, macOS, or Windows (WSL)
- **Compiler**: GCC 11+, Clang 14+, or MSVC 2022+
- **CMake**: 3.20+
- **Python**: 3.8+ (for build scripts and tests)

### Clone and build

1. **Fork the project**

```bash
git clone https://github.com/YOUR_USERNAME/Hahaha.git
cd Hahaha
```

2. **Create a development branch**

Before starting development, please create a new branch **based on the dev branch**. The branch name should clearly describe your work:

```bash
# For new features
git checkout -b feature/[feature-name]-implement
# Example: feature/adam-optimizer-implement

# For bug fixes
git checkout -b bug/[bug-description]-fix
# Example: bug/tensor-broadcast-fix

# For documentation updates
git checkout -b docs/[doc-content]-update
# Example: docs/api-documentation-update
```

**Branch naming suggestions**:
- Use lowercase letters and hyphens
- Keep names concise but descriptive
- Avoid overly long branch names

3. **Install dependencies**

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install build-essential cmake ninja-build

# macOS (Homebrew)
brew install cmake ninja

# Arch Linux
sudo pacman -S cmake ninja

# Fedora/CentOS
sudo dnf install cmake ninja-build
# or
sudo yum install cmake ninja-build
```

4. **Build**

```bash
# Use Meson (recommended)
meson setup builddir
ninja -C builddir

# After building, the library files will be in the builddir directory
```

### IDE setup

Recommended IDEs:

- **CLion**: native support for CMake and Meson projects
- **VS Code**: install C++ extensions and CMake Tools or Meson tools
- **Visual Studio**: supports CMake projects

## Coding standards

For detailed coding standards, please refer to: [Code Style Documentation](code-style.md)

## Commit message format

Commit messages should describe the change clearly:

```text
type(scope): short summary

Details (optional)

Fixes #123
```

Common types:

- `feature`: new feature
- `fix`: bug fix
- `docs`: documentation updates or comment improvements
- `style`: formatting only
- `refactor`: refactoring
- `test`: tests
- `chore`: build/tooling

## Development workflow

### 1. Create a branch

```bash
# Keep dev branch up to date (if upstream is configured)
git checkout dev
git pull upstream dev

# Create a feature branch (use clear naming)
git checkout -b feature/your-feature-name
# Or a bug fix branch
git checkout -b bug/issue-description-fix
# Or a documentation branch
git checkout -b docs/documentation-update
```

### 2. Implement

- Follow the necessary [coding standards](code-style.md)
- Add necessary comments
- Write unit tests
- Update related docs

### 3. Test

Run the test suite:

```bash
# Build and run tests with Meson
meson setup builddir
ninja -C builddir
ninja -C builddir test

# Or use CMake
mkdir cmake-build && cd cmake-build
cmake .. -GNinja
ninja
ctest

# Run specific test modules
# Tensor tests
ninja -C builddir test-tensor
# Autograd tests
ninja -C builddir test-autograd
# ML tests
ninja -C builddir test-ml
```

### 4. Format

Use the project formatting script:

```bash
# Format code
./format.sh

# Check formatting
./format.sh --check
```

## Submitting a Pull Request

### 1. Push your branch

```bash
git add .
git commit -m "feature: implement tensor addition

- add Tensor::add method
- support broadcasting
- add corresponding unit tests"

git push origin feature/tensor-add
```

### 2. Create the PR

On GitHub:

1. Open your fork
2. Click “Compare & pull request”
3. Fill in the PR description:
   - summarize what changed
   - link related issues (if any)
   - describe how you tested
   - @mention appropriate reviewers

### 3. Review

After the PR is created:

- wait for CI/CD checks to pass
- respond to reviewer comments
- update code based on feedback
- after approval, wait for merge

## Other notes

### Working on an already-claimed task

If the task you want already has an owner:

1. contact the owner (via issue or PR)
2. discuss how to split the work
3. create a sub-branch based on the owner’s branch:

```bash
git checkout feature/optimizer-implementation
git checkout -b feature/optimizer-implementation-adam
```

### Types of contributions

Besides code, we also welcome:

- **Docs improvements**: fix doc issues, add examples
- **Testing**: improve coverage, add edge-case tests
- **Performance**: optimize algorithms or memory usage
- **Tooling**: build scripts, CI/CD workflows
- **Design discussions**: record architecture decisions in ADRs

### Recognition

Your contributions may be recognized via:

- adding your info to the contributors list
- recording your implementation reasoning in the docs
- participating in project decision discussions

## Documenting implementation reasoning (encouraged)

### Why we encourage it

Hahaha emphasizes **educational value**. Great code with clear documentation has a much bigger impact. It helps others understand your work, and also:

1. **Knowledge transfer**: future contributors can learn the ideas and details faster
2. **Collaboration**: improves teamwork and code reviews
3. **Learning value**: provides thinking processes and design decisions for learners
4. **Maintenance**: helps people understand existing logic when changes are needed

### What to document

When you implement, optimize, or fix bugs in code, we encourage you to record your reasoning in the relevant documentation.

**Document location guidelines**:

Documents should be placed in the `src/<lang>/explains/` directory (we encourage you to use English), and the directory structure should correspond to the code directory structure. For example:

- If you implement, optimize, or fix bugs in `core/include/ml/optimizer/AdamOptimizer.h`, you can add relevant content to `src/en/explains/ml/optimizer.md`
- If you implement, optimize, or fix bugs in `core/include/math/TensorWrapper.h`, you can add relevant content to `src/en/explains/math/tensor-wrapper.md`
- If you implement, optimize, or fix bugs in `core/include/compute/graph/ComputeNode.h`, you can add relevant content to `src/en/explains/compute/graph.md`

If the corresponding document file does not exist, you can create a new file. The document directory structure should clearly reflect the design thinking of the code.

We fully support using AI to help with writing: you can describe your ideas verbally and let AI polish them (especially for hard-to-write formulas), or keep your own style with minimal polishing—we respect your choice.

That said, we hope to see real personal understanding and thought, rather than purely AI-generated text that is hard to follow.

#### 1. New feature implementation

Possible things to record:

- **Design reasoning**: why this approach, and what alternatives existed?
- **Algorithm**: the math principles and steps
- **Architecture**: how it integrates with other modules; interface principles
- **Performance**: time/space complexity and optimizations (if any)
- **Edge cases**: how special inputs are handled (if needed)

Example doc structure:

```markdown
## Feature name

### Implementation approach
[describe your design and decisions]

### Algorithm
[math principles and steps]

### Architecture
[integration with the existing system]

### Performance
[complexity and optimization notes]
```

#### 2. Optimizing existing functionality

When you optimize or refactor code, update docs to record:

- **Motivation**: why the optimization is needed
- **Improvements**: measurable wins (speed, memory, etc.)
- **Compatibility**: whether APIs changed and how you kept backwards compatibility
- **Trade-offs**: benefits and costs of the change

Example update:

```markdown
## Optimization notes

### [Feature implemented by the optimization](link-to-your-change)
- **What changed**: [details]
- **Improvement**: compute time -30%, memory -20%
- **How**: [technical details]
- **Compatibility**: fully backwards compatible
```

#### 3. Bug fixes

When fixing bugs, record:

- **Symptoms**: observed behavior and impact scope
- **Root cause**: why it happened
- **Fix**: why you chose this fix
- **Verification**: how you validated correctness

### Where to place docs

We recommend placing docs in locations corresponding to the code implementation, for example:
- Core algorithm explanations can be placed in `src/<lang>/hahaha/` (e.g. `src/en/hahaha/`)
- Architecture design docs can be placed in `src/<lang>/design/` (e.g. `src/en/design/`)
- API interface documentation can be in header file comments and synced to relevant docs

### Practical tips

1. Try writing design docs before implementation—it helps clarify thinking
2. Combine docs with code comments: comments for details, docs for overview
3. Periodically review docs during code review
4. Start small: improve docs for existing features to get familiar

### Benefits of documenting

Contributors who document their thinking often get:

- **Faster reviews**: clear docs help reviewers understand your implementation approach quickly
- **Documentation and code sync PRs**: you can submit code implementation and documentation as separate PRs, reference the documentation PR in the code PR, and we will merge them together after review
- **Community recognition**: your thinking becomes valuable knowledge for the project
- **Learning opportunities**: documentation deepens your own understanding
- **More impact**: your design ideas may shape the project's future

We believe: **good code + good docs = excellent contributions**. We don’t strictly require it, but we genuinely encourage it—it increases the value of your work and makes the project better.

## Getting help

If you run into issues while contributing:

- check the [FAQ](../appendix/faq.md)
- ask in GitHub Issues
- join community discussions

We aim to make contributing enjoyable and efficient!
