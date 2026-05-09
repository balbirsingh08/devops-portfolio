# Contributing to DevOps Portfolio

First off, thank you for considering contributing to this DevOps portfolio project! It's aimed at demonstrating professional-grade infrastructure practices, and we value contributions that improve its quality, documentation, and real-world applicability.

## 📋 Code of Conduct

This project and everyone participating in it is governed by a covenant of professional respect and collaboration. We're committed to providing a welcoming and inclusive environment.

## 🎯 How Can I Contribute?

### Reporting Bugs
- Check if the bug has already been reported in [Issues](https://github.com/balbirsingh08/devops-portfolio/issues)
- If not, create a new issue with:
  - Clear, descriptive title
  - Exact reproduction steps
  - Expected vs. actual behavior
  - Environment details (OS, Docker version, Kubernetes version, etc.)
  - Screenshots if applicable

### Suggesting Enhancements
- Use GitHub Issues with the `enhancement` label
- Clearly describe the use case and expected benefits
- Provide examples if the feature involves new deployment patterns

### Pull Requests
1. **Fork** the repository
2. **Create a branch** for your changes: `git checkout -b feature/your-feature-name`
3. **Make changes** following our standards (see below)
4. **Test locally** using Docker Compose and Kubernetes
5. **Commit** with clear, descriptive messages following conventional commits:
   - `feat: add prometheus alerting rules`
   - `fix: correct kubernetes resource limits`
   - `docs: update deployment procedures`
6. **Push** to your fork
7. **Submit a Pull Request** with detailed description of changes

## 📐 Code Standards

### YAML Files (Kubernetes, Docker Compose, Ansible)
- Use 2-space indentation
- Validate with `yamllint` before commit
- Add comments explaining non-obvious configurations
- Use semantic naming for resources

```yaml
# Example with comments
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: production
data:
  # Application log level (debug, info, warn, error)
  LOG_LEVEL: "info"
  # Database connection pool size
  DB_POOL_SIZE: "20"
```

### Terraform Files
- Use `terraform fmt` for consistent formatting
- Add descriptions to all variables and outputs
- Use meaningful local variable names
- Include comments for complex logic

```hcl
# Example with documentation
variable "replica_count" {
  description = "Number of application replicas in Kubernetes deployment"
  type        = number
  default     = 3

  validation {
    condition     = var.replica_count > 0 && var.replica_count <= 10
    error_message = "Replica count must be between 1 and 10."
  }
}
```

### Shell Scripts
- Use `shellcheck` for linting
- Add shebang: `#!/bin/bash`
- Include error handling and exit codes
- Document complex functions

```bash
#!/bin/bash
# Deploy application with health checks
# Usage: ./deploy.sh <environment> <version>

set -euo pipefail  # Exit on error, undefined vars, pipe failures

main() {
  local environment="${1:-staging}"
  local version="${2:-latest}"
  
  # Validation
  if [[ ! "$environment" =~ ^(staging|production)$ ]]; then
    echo "Error: Invalid environment. Use 'staging' or 'production'"
    exit 1
  fi
}
```

### GitHub Actions Workflows
- Use descriptive job and step names
- Include inline comments for conditional logic
- Pin action versions (avoid `@latest`)
- Document secrets required for the workflow

## 🧪 Testing Requirements

### For CI/CD Changes
- Test workflow locally with `act` if possible
- Verify all matrix combinations
- Check artifact uploads work correctly
- Validate deployment steps with dry-run flags

### For Kubernetes Manifests
```bash
# Validate syntax
kubectl apply -f kubernetes/ --dry-run=client

# Check for best practices
kubeval kubernetes/*.yaml

# Lint with kubesec
kubesec scan kubernetes/deployment.yaml
```

### For Terraform Changes
```bash
# Format and validate
terraform fmt -recursive
terraform validate

# Plan with explicit target
terraform plan -out=tfplan

# Check for security issues
tfsec .
```

### For Ansible Changes
```bash
# Syntax check
ansible-playbook --syntax-check playbook.yml

# Dry-run
ansible-playbook --inventory inventory/hosts.ini --check playbook.yml

# Lint
ansible-lint playbook.yml
```

## 🔍 Pre-commit Hooks

This repo uses pre-commit hooks. After cloning:

```bash
pip install pre-commit
pre-commit install
```

Hooks automatically run on `git commit`:
- `yamllint` - Validates YAML syntax
- `terraform fmt` - Formats Terraform code
- `shellcheck` - Checks shell scripts
- `end-of-file-fixer` - Ensures files end with newline
- `trailing-whitespace` - Removes trailing spaces

## 📝 Documentation Standards

- Keep README.md current with your changes
- Document new features in relevant subdirectory READMEs
- Add inline comments for non-obvious logic
- Use Markdown formatting consistently
- Include examples for complex features

## 🎨 Commit Message Conventions

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Formatting changes (no logic change)
- `refactor` - Code refactoring without feature/fix
- `perf` - Performance improvements
- `test` - Test additions/modifications
- `chore` - Build process, dependencies, tooling

**Examples**:
```
feat(kubernetes): add network policy for pod isolation

Implement a deny-all default network policy and allow specific ingress
rules for the frontend tier to communicate with backend services.
Improves security posture of the cluster.

Closes #42
```

```
fix(terraform): correct EKS security group ingress rules

The current ingress rule was using incorrect CIDR notation (0.0.0.0/32
instead of 0.0.0.0/0), blocking legitimate traffic.
```

## 🔐 Security Considerations

- Never commit secrets, API keys, or credentials
- Use `.gitignore` for sensitive files
- Reference secrets via environment variables
- Follow cloud provider security best practices
- Scan images with Trivy before pushing

## 🚀 Deployment Impact

Before submitting a PR, consider:
- Will this change affect production deployments?
- Are there any backwards compatibility concerns?
- Do infrastructure changes require multiple apply stages?
- Have rollback procedures been documented?

## 📊 Performance & Scalability

- Optimize Docker image sizes
- Test Kubernetes manifests with resource limits
- Validate Terraform apply times with large state
- Document performance considerations

## 🎓 Learning Resources

- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Ansible Documentation](https://docs.ansible.com/)

## ❓ Questions?

- Check existing issues for similar questions
- Review existing code comments and documentation
- Reach out via GitHub Discussions

## 📜 License

By contributing to this project, you agree that your contributions will be licensed under its MIT License.

---

Thank you for contributing to making this a comprehensive, production-ready DevOps portfolio! 🙌