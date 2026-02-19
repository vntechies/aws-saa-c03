# 🛠️ DEVELOPER TOOLS - AWS SAA-C03
## 📌 Tổng Quan
Developer Tools chiếm **5-8% câu hỏi**. Focus vào CI/CD services.

## 1️⃣ AWS CODECOMMIT
**CodeCommit** = Managed Git repository.
- **Private Git repos**, **Encryption** at rest/transit
- **Integration**: CodeBuild, CodePipeline

## 2️⃣ AWS CODEBUILD
**CodeBuild** = Managed build service, compile code, run tests.
- **Serverless**, **Pay per minute**, **Docker-based**
- **buildspec.yml**: Build instructions

## 3️⃣ AWS CODEDEPLOY
**CodeDeploy** = Automated deployment to EC2, Lambda, ECS, on-premises.
### Deployment Types:
- **In-Place**: Update existing instances
- **Blue/Green**: New instances, swap traffic
### Deployment Configs:
- **AllAtOnce**, **HalfAtATime**, **OneAtATime**, **Custom**
**🎯 Exam Tip**: "Automated deployment" = CodeDeploy

## 4️⃣ AWS CODEPIPELINE
**CodePipeline** = CI/CD orchestration, automate release pipelines.
- **Stages**: Source, Build, Test, Deploy
- **Actions**: CodeCommit, CodeBuild, CodeDeploy, Lambda, etc.
**🎯 Exam Tip**: "CI/CD pipeline" = CodePipeline

## 5️⃣ AWS CODEARTIFACT
**CodeArtifact** = Artifact repository (Maven, npm, PyPI).
- **Store dependencies**, **Proxy public repos**

## 6️⃣ AWS CODEGURU
**CodeGuru** = ML-powered code reviews and performance recommendations.
- **Reviewer**: Code quality
- **Profiler**: Runtime performance

## 📋 DEVELOPER TOOLS COMPARISON
| Service | Purpose |
|---------|---------|
| **CodeCommit** | Git repository |
| **CodeBuild** | Build and test |
| **CodeDeploy** | Automated deployment |
| **CodePipeline** | CI/CD orchestration |
| **CodeArtifact** | Artifact repository |
| **CodeGuru** | Code review (ML) |

**🎯 Exam Tip**: "Full CI/CD" = CodeCommit → CodeBuild → CodeDeploy (orchestrated by CodePipeline)

**Next**: [09-analytics-bigdata.md](09-analytics-bigdata.md) 🚀
