---
title: "Event 1"
date: 2024-06-06
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “Modern Cloud Infrastructure Engineering”

### Event Objectives

- Share the career development roadmap from IT Helpdesk to Senior Sysadmin.

- Equip foundational knowledge and hands-on practice on application containerization technology with Docker.

- Introduce Serverless architecture solutions for real-time games.

- Analyze cyber security solutions combining AWS WAF and Machine Learning NIDS.

- Guide building GraphRAG systems with Amazon Neptune and Amazon Bedrock.

- Share the art of effective teamwork and applying tools in work management.

### Speakers

- **Tran Trung Vinh** - System Administrator at Central Retail Group

- **Bao Huynh** - Junior Cloud Native Developer at Endava Vietnam, Founder / Head Lab - ITea Lab

- **Le Hoang Gia Dai** - AWS Cloud Engineer & Cyber Security Engineer

- **Viet Phat** - AI Major at Swinburne University of Technology

- **Nguyen Quoc Bao** - Speaker

- **Truong Huy Phuoc** - Speaker

### Key Highlights

#### The Career Journey from IT Helpdesk to Senior Sysadmin

- **Practical roadmap**: Graduating from prestigious universities is not mandatory. The progression from IT Helpdesk → Junior Sysadmin → Senior Sysadmin requires continuously accumulating practical knowledge, self-learning, and deeply understanding infrastructure.
- **Operational mindset & troubleshooting**: Ensure system availability, handle emergency incidents, and establish monitoring with Kubernetes, Grafana/Prometheus.
- **MNC interview experience**: Emphasize real projects, architectural design capability, incident response scenarios, and troubleshooting skills.
- **Advice**: Go deep into 1–2 core skills, prioritize building a real-world portfolio over certifications, and remain persistent.

#### Containerization Fundamentals and Hands-on Practice with Docker

- **Virtualization vs. Containerization**: Compare the differences between **Traditional Virtualization** using Hypervisors (where each VM runs a separate OS) and **Application Containerization** (which shares the OS kernel, making it lighter, booting extremely fast, and optimizing resources).
- Master the **core Docker components**:
    - **Docker Image** - static packaged blueprint
    - **Docker Container** - executable instance of an Image
    - **Dockerfile** - packaging script
    - **Docker Volume** - persistent data storage
    - **Docker Network** - inter-container connectivity
- **CLI Command system**: Introduce key command groups that make managing multi-container setups easy, such as `docker run`, `exec`, `logs`, `stop`, `build`, `pull`, `push`, `docker-compose up`, `logs`, `build`.
- **Real-world applications**: Widely used in CI/CD pipelines, Microservices architecture, Cloud-native applications, and legacy modernization.

#### Multiplayer Serverless Game Architecture with AWS WebSockets and Godot

- **UDP/ENet connection model**: Ultra-low latency, suitable for FPS/Racing games.
- **HTTP Polling connection model**: Simple, high latency, resource-intensive.
- **WebSocket connection model**: Full-duplex two-way communication, reliable, ideal for turn-based games, lobby, chat.
- **Serverless on AWS**: Game Client → API Gateway WebSocket → AWS Lambda → Amazon DynamoDB.
- Compare the WebSocket + Lambda model with AWS GameLift when developing games with high real-time physics calculation demands.

#### Cyber Attack Detection Combining AWS WAF and Machine Learning NIDS

- **Limitations of WAF**: Effectively protects L7 (HTTP/HTTPS) against SQL Injection, XSS, Bot traffic, but is insufficient to detect Zero-day attacks or complex anomalous behaviors.
- **ML-based NIDS**: A Network Intrusion Detection System (NIDS) based on trained Machine Learning that helps identify complex attack types such as DoS/DDoS, Brute Force, FTP/SSH attacks.
- Real-time data streams should be analyzed using ML, correlating NIDS events and AWS WAF on a real-time dashboard to enhance automated monitoring and response capabilities.
- ML accuracy depends on data quality and handling Class Imbalance; combining WAF with ML optimizes a multi-layered security model.

#### Building Modern GraphRAG Applications with Amazon Bedrock and Amazon Neptune

- **Limitations**: Traditional RAG based on Vector Search and Text Chunks struggles with complex questions or multi-hop query requirements, easily causing Hallucination.
- **GraphRAG**: Integrates Knowledge Graphs to help LLMs deeply understand linked context and increase answer accuracy.
- **Architecture on AWS**: Combines Amazon Bedrock with Amazon Neptune / Neptune Analytics.
- **Deployment models**: Fully Managed with Bedrock + Neptune Analytics or Open-source toolkit with LlamaIndex + Custom routes.

#### The Art of Effective Teamwork and Collaboration Workflow

"Many hands make light work" - Teamwork efficiency vastly surpasses individual effort thanks to synergy and workload sharing.
- 4 Golden Rules of Teamwork:
    - **Clear & Shared Goals**: Clear and unified common goals.
    - **Right Person, Right Place**: Assigning the right person to the right job based on competence.
    - **Open Communication & Active Listening**: Open communication and active listening.
    - **Personal Accountability**: Sense of personal responsibility for assigned tasks.
- **Applying tools to workflows**:
    - Manage source code and workflow via GitLab through Merge Requests, Code Review.
    - ClickUp project management tool.
    - Automated communication channels such as Discord/ClickUp Notifications.

### Key Takeaways

#### Architectural Thinking & System Design

- **Real-time Serverless Architecture**: Coordinate API Gateway WebSocket, Lambda, and DynamoDB to build cost-optimized two-way communication systems.
- **GraphRAG & Knowledge Graph**: How combining Vector DB and Graph DB (Amazon Neptune) enhances multi-hop reasoning capabilities for LLMs.
- **Application Containerization**: Deeply understand Docker's OS-level virtualization nature to build consistent, easily scalable development environments.

#### Cyber Security & System Operations Thinking

- **Defense in Depth**: Combining WAF (L7) and Machine Learning NIDS provides a proactive defense mechanism against anomalous behavior.
- **SysAdmin & Troubleshooting Mindset**: Deeply understand infrastructure fundamentals, manage incidents proactively, and establish continuous monitoring.
- **Automation & Resource Optimization**: Prioritize CI/CD automation, application packaging with Docker, and database cost optimization.

#### Collaboration Workflow

- **4 Golden Rules of Teamwork**: Clear goals, right person right place, open communication, and personal accountability.
- **Career Orientation**: Sustainable growth through core skills and practical products, avoiding chasing certification quantity.

### Applying to Work

- **Adopt Docker Containerization**: Package microservices using Dockerfile and Docker Compose to standardize Dev/Test/Prod environments.

- **Implement WebSocket Serverless**: Use API Gateway WebSocket and AWS Lambda to build real-time features (chat, game lobby).

- **Integrate AWS WAF & ML NIDS**: Configure WAF Web ACLs combined with ML anomaly detection models to protect systems.

- **Deploy GraphRAG with Amazon Bedrock**: Combine Amazon Neptune Analytics and Bedrock to solve complex enterprise document retrieval problems.

- **Apply 4 Golden Rules of Teamwork**: Manage Merge Requests on GitLab, track progress on ClickUp, and maintain personal accountability.

- **Focus on Core SysAdmin/DevOps Skills**: Practice infrastructure labs, hone troubleshooting skills, and complete a practical portfolio.

### Event Experience

#### Learning from highly skilled speakers
- Speakers from Central Retail Group, Endava, Swinburne University brought diverse real-world perspectives.
- Practical sharing covered everything from deep technical topics (WAF, ML NIDS, WebSockets, GraphRAG) to career experiences.

#### Hands-on technical experience
- Grasp the deployment scenario of Serverless WebSocket with Godot Engine and how to handle client disconnects.
- Understand how combining ML with AWS WAF protects systems and how GraphRAG is applied to modern AI.

#### Networking and exchange
- An open atmosphere helped connect students, developers, and industry experts.
- Reinforced bonding spirit thanks to the 4 golden rules of teamwork and digital collaboration tools.

#### Lessons learned
- Foundational knowledge (Linux, Networking, Docker, Cloud) and problem-solving mindsets are core assets.
- Emerging technologies (AI/ML, GraphRAG, Serverless) must go hand in hand with cyber security thinking and teamwork skills.

#### Some event photos
*Add your event photos here*
> Overall, the event provided not only technical knowledge but also transformed my mindset regarding application design, system modernization, and more effective cross-team collaboration.
