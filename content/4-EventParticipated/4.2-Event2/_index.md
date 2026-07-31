---
title: "Event 2"
date: 2026-07-11
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: “SLA, System Monitoring, AWS Security Agent, and AWS Cloud Practitioner”

### Event Objectives

- Explain the role of an SLA and how to monitor signals that genuinely affect users.

- Introduce a continuous process for identifying risks, monitoring signals, responding to incidents, and improving the system afterward.

- Present how AWS Security Agent can support design reviews, source-code analysis, and application security testing.

- Examine the benefits and limitations of automating security testing with an AI Agent.

- Provide an overview of the AWS Certified Cloud Practitioner certification and its four exam domains.

### Key Highlights

#### From SLA to Monitoring What Really Matters

- **An SLA is a formal commitment**: A Service Level Agreement defines the service level that a provider commits to delivering to a customer. It establishes expectations, accountability, performance measurement, and risk-management responsibilities.
- **Monitoring is part of risk management**: Monitoring is not limited to observing system health. It should detect risks before they affect the SLA and the customer experience.
- **Identify risks**: Teams first determine which situations can affect availability, performance, or the user's ability to complete an important task.
- **Monitor signals**: After identifying a risk, the system should collect the relevant metrics, logs, and alarms so abnormal behavior can be detected early.
- **Respond to incidents**: When an alarm activates, the system can notify responders through Amazon SNS, initiate a standard operating procedure, and begin recovery actions.
- **Continuously improve**: After an incident, the team reviews its cause, evaluates the response, and refines monitoring to prevent the same problem from recurring.

#### The Monitoring Pyramid

The monitoring pyramid demonstrates how low-level technical data should connect to real-world outcomes at higher levels:

1. **Customer Experience** - At the top of the pyramid, this reflects whether end users can complete their journeys and receive the expected experience.
2. **Business** - Tracks measurements such as successful-login rate, order volume, transaction-completion rate, and revenue.
3. **Application** - Observes latency, error rate, and request count to evaluate application behavior.
4. **Infrastructure** - Monitors CPU, memory, disk, and network utilization.
5. **Cloud Provider** - At the foundation, this represents the status of services such as Amazon EC2, Amazon RDS, Elastic Load Balancing, and Amazon S3.

- **Healthy infrastructure does not guarantee happy users**: Health checks can pass while users remain unable to sign in, place an order, or complete another important task.
- **Infrastructure cannot describe the entire service experience**: Stable CPU and memory measurements do not prove that a business workflow is functioning correctly.
- **Responsibility is shared**: AWS is responsible for the cloud infrastructure within its scope, while the application team remains responsible for configuration, data, application logic, and user experience.
- **Understand the user journey**: Effective monitoring begins with understanding what users need to accomplish and what could cause that journey to fail.

#### Securing Web Applications with AWS Security Agent

- **Manual penetration testing can take weeks**: Traditional assessments commonly require significant time for preparation, execution, verification, and reporting.
- **Specialist services can be expensive**: According to the session, third-party penetration-testing engagements can range from USD 5,000 to USD 20,000, depending on scope and complexity.
- **Quality depends on expertise**: Test results are heavily influenced by a penetration tester's experience, methodology, and analytical ability.

#### Frontier Agents and Full-Lifecycle Security

- **Powered by Amazon Bedrock**: Security Agent can plan and execute complex security workflows with a high level of automation.
- **Design Review**: The Agent analyzes architecture documents and evaluates designs against frameworks such as PCI DSS, the NIST Cybersecurity Framework, and the AWS Well-Architected Framework.
- **Code Security**: The Agent scans pull requests for vulnerabilities and sensitive information accidentally committed to source code, such as passwords or API keys.
- **Active Penetration Testing**: In an environment explicitly authorized by its owner, the Agent can simulate user behavior, validate vulnerabilities through multi-step test sequences, and produce evidence and attack-path diagrams for review.
- **Development workflow integration**: Security Agent can integrate with GitHub or GitLab pull requests, add comments to relevant lines, and propose Auto-PR Fixes.
- **Full-lifecycle protection**: Design Review, Code Security, and Active Penetration Testing introduce security throughout development instead of waiting until the end of a project.

#### Important Limitations

- **Strong authentication can interrupt automation**: MFA, biometrics, and mTLS introduce verification steps that an Agent may not be able to complete automatically.
- **Limited business context**: The Agent can struggle to identify business-logic abuse when it lacks a deep understanding of company rules, roles, and workflows.
- **Complexity increases execution time**: Larger applications with many user journeys require more testing time, making scope control and runtime monitoring essential.
- **Human review remains necessary**: Development and security teams must evaluate automated findings before treating them as final conclusions or applying proposed fixes.
- **Testing requires authorization**: Penetration testing must be limited to systems that the tester owns or has explicit permission to assess, with a defined scope and rules of engagement.

#### Inside the AWS Cloud Practitioner Exam

- **A foundational certification**: AWS Certified Cloud Practitioner focuses on cloud thinking and a broad understanding of AWS services.
- **No advanced programming requirement**: Candidates are not expected to write code or configure a detailed production system during the exam.
- **Domain 1 - Cloud Concepts (24%)**: Covers cloud benefits, deployment models, elasticity, scalability, and cloud economics.
- **Domain 2 - Security and Compliance (30%)**: Focuses on the shared responsibility model, IAM, data protection, compliance, and foundational security services.
- **Domain 3 - Cloud Technology and Services (34%)**: Covers compute, storage, databases, networking, analytics, and common service-selection scenarios.
- **Domain 4 - Billing, Pricing, and Support (12%)**: Covers pricing models, cost-management tools, AWS Support, and available support resources.

### Key Takeaways

#### Monitoring Must Connect to Users

- Dashboards should not be built only from CPU, memory, or individual service-status measurements.
- Every technical signal should connect to a risk, business impact, or specific user journey.
- An alarm is useful only when it has an owner, a notification path, and a clear response procedure.
- Monitoring should be updated after every incident to include previously missed signals.

#### Security Automation Requires Control

- AI Agents can reduce the time required for design assessment, code review, and validation of certain vulnerabilities.
- Agents cannot completely replace security professionals, especially for complex authentication and business-logic flaws.
- Security should be included throughout the development lifecycle instead of relying only on penetration testing before release.
- Active testing must operate within clearly defined authorization, scope, and safety boundaries.

#### Cloud Practitioner Builds an AWS Foundation

- The certification helps learners understand the common language of cloud before specializing in architecture or operations.
- Security and Compliance together with Cloud Technology and Services represent most of the exam, so they should receive significant attention in a study plan.
- Study should focus on why a service is selected and when it is appropriate rather than memorizing disconnected service names.

### Applying the Lessons to Study and Work

- **Define SLI, SLO, and SLA**: Select indicators that represent service quality, establish internal objectives, and make suitable commitments to customers.

- **Design dashboards from the top down**: Start with customer experience and business metrics, then connect them to application, infrastructure, and cloud-provider measurements.

- **Build an incident-response process**: Connect alarms to Amazon SNS or another suitable notification channel, assign ownership, and prepare recovery procedures.

- **Conduct post-incident reviews**: Analyze causes, missed signals, and required improvements after every incident.

- **Integrate security into development**: Review architecture before coding, scan pull requests, and perform active tests only in explicitly authorized environments.

- **Track Agent limitations**: Control scope, runtime, authentication requirements, and business context before automating an assessment.

- **Prepare for Cloud Practitioner by domain**: Study all four domains, practise scenario-based questions, and prioritize shared responsibility, security, core services, and cost management.

### Event Experience

#### Connecting SLA to Real User Experience

- The session clarified that monitoring is not only about infrastructure; it must measure whether users can complete their intended tasks.
- The monitoring pyramid provides a clear way to connect cloud status with application behavior, business results, and customer experience.

#### A New Perspective on Security Automation

- Security Agent demonstrates how AI Agents can participate from design review through source-code analysis and authorized penetration testing.
- The limitations emphasize that automation still requires a clear scope, human oversight, and an understanding of business context.

#### A Clearer AWS Learning Direction

- The four-domain structure helps beginners identify the knowledge required for AWS Cloud Practitioner.
- The certification is best understood as a foundation rather than a replacement for practical experience.

![Event 2](<../../images/4-Event/Event2.png>)