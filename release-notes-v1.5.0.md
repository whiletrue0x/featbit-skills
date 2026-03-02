```markdown
# v1.5.0 - AWS Deployment Skill

## What's New

- ✨ Added new `featbit-deployment-aws` skill for deploying FeatBit on AWS
- ☁️ ECS Fargate with ALB — references `featbit-terraform-aws` as the recommended Terraform baseline
- ⚡ Explains fault tolerance (same-AZ redundancy) vs. true High Availability (cross-AZ task placement)
- 🌐 Step-by-step guide: ALB target group routing, health check paths, multi-AZ spread constraints
- ☸️ EKS/Kubernetes deployments redirected to the `featbit-deployment-kubernetes` skill
- 📈 Updated plugin from 14 to 15 specialized skills

---

**Full Changelog**: https://github.com/featbit/featbit-skills/compare/v1.4.0...v1.5.0
```
