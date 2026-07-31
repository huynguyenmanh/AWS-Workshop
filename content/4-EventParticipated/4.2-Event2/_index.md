---
title: "Event 2"
date: 2026-06-13
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---



# Summary Report: “Scalable AWS Architecture & DevOps Fundamentals workshop”

### Event Objectives

- Share how to design and deploy scalable system architectures, specifically highlighting a URL shortener service on the AWS platform.
- Provide a practical perspective and share real-world experiences regarding the roles and daily tasks of DevOps and Data Analytics Engineers in enterprises.
- Guide the learning roadmap, equip students with foundational skills, and provide career development orientations in the IT industry starting from university.
- Introduce standard recruitment processes and working cultures in multinational corporations (MNCs).

### Speakers

- **Dat Pham** - Data Analytics Engineer
- **Cuong Nguyen** - Process Engineer
- **Trong H. Truong** - DevOps Engineer at Endava Vietnam
- **Danh Hoang Hieu Nghi** - AI Engineer, AWS Community Builder, AWS Student Builder Group Leader
- **Dinh Trung Kien** - Lead Developer at startup
- **Nguyen Minh Tho** - Student

### Key Highlights

#### The Journey of Starting and Growing with Cloud Computing

Starting from Student Curiosity <br>
→ Learning from tech communities <br>
→ Practicing through hands-on labs <br>
→ Building personal projects & portfolios → Becoming an AWS Partner <br>
→ Sharing knowledge back to the community (Share Back).<br>
Getting the job is not the destination but merely the beginning of a long journey of learning and contribution.

#### Introduction to URL Shortener System Design on AWS

- The basic architecture has the **advantages** of easy deployment and low cost but faces **limitations** such as security risks, single points of failure, high latency, and difficulties in scaling.
- To optimize performance and security, services like Amazon CloudFront, AWS WAF, and AWS Amplify can be integrated.
- Using a Key Generation Service (KGS) on Amazon ECS to pre-generate short keys and push them into the Amazon ElastiCache (Redis) helps optimize overall system speed.
- At the Backend, a SpringBoot application on Amazon ECS fetches the short key from Redis to map it with the destination URL and stores it in a DynamoDB database.

#### Real-World Work and Career Roadmap of a DevOps Engineer

First, to truly understand DevOps, it's not just about writing CI/CD pipelines, configuring the cloud, or managing Docker/Kubernetes; it also requires a deep understanding of how applications run in the real world.<br>
**Required Foundational Knowledge:**
- Prioritize mastering Linux fundamentals
- Basic Networking knowledge
- Programming languages (e.g., Python, Golang)
- Git
- CI/CD
- Containers

**Working Mindset:** Tools may change, but fundamentals stay.
A good DevOps engineer needs to develop system thinking, stay curious and keep learning, automate boring tasks, and make things clear and easy for everyone in the team.

#### Career Orientation and Skills of a Data Analytics Engineer

- **Real-world responsibilities:** Varies by industry domain, focusing on building reports, designing dashboards to track trends, conducting root cause analysis, and collaborating across departments to solve operational problems.
- **Required skills:** Critical thinking to evaluate information objectively, effective communication skills, data storytelling capabilities, and the ability to find optimal solutions based on data.
- **Career progression:** Follower → Learner → Problem Solver → System Thinker → Super Star.

#### Corporate Culture in Multinational Corporations (MNCs)

- **Standardized recruitment process:** Candidates go through multiple screening rounds, from ATS systems to interviews assessing technical skills and culture fit.
- **Working environment**: Cultivates a respectful and caring workplace that values diversity and encourages comprehensive development.
- **"No-Blame Post-Mortem" culture**: When operational incidents occur, the company focuses on root cause analysis to improve systems and processes rather than assigning blame to individuals.

### Key Takeaways

#### Architectural Thinking & System Design

- **Scalability mindset**: Always aim for a scalable and flexible system. Understand how to transition from a simple monolithic architecture to a highly scalable distributed architecture, utilizing caching and NoSQL databases.
- **Separation of Concerns**: Designing a separate Key Generation Service (KGS) helps reduce the direct load on the primary database when generating short URLs.
- **Latency & Security optimization**: Combine Amazon CloudFront, AWS WAF, and API Gateway to protect the system and deliver an optimal end-user experience.

#### DevOps Mindset & System Operations

- Master **fundamental knowledge** such as Linux, Networking, and Containers instead of relying heavily on tools that constantly change over time.
- Embrace **System Thinking**, viewing an application throughout its entire lifecycle (Build, Test, Deploy, Monitor, Fix) rather than just completing isolated tasks.
- Learn how to analyze incidents and always look for ways to improve the system to prevent recurring errors and proactively mitigate potential future vulnerabilities.

#### Career Roadmap & Professional Conduct

- A career development roadmap should be built through practical products, certifications, community contributions, and continuous learning.
- Besides technical expertise, soft skills—especially communication and active listening—play a crucial role in an international environment.

### Applying to Work

- **Caching**: Optimize data queries by utilizing Redis as a cache.
- **DynamoDB**: NoSQL database → build applications that require high security, fast response times, flexibility, and auto-scaling capabilities.
- **Automate CI/CD pipelines**: Use Python/Golang to write automation scripts and optimize Dockerfiles, eliminating repetitive manual tasks.
- **Enhance Data Storytelling**: Design dashboards focusing on key business metrics and perform Root Cause Analysis (RCA).
- **Build hands-on portfolio**: Participate in practical AWS labs and proactively share back in the community to improve practical skills and expand professional networks.

### Event Experience

Attending the event was an incredibly valuable and rewarding experience, helping me expand my mindset on AWS architectural design, truly understand the nature of DevOps and Data Analytics roles, and shape my tech career roadmap.

#### Learning from highly skilled speakers
- Experienced speakers from the AWS Community, Endava, Colgate-Palmolive, and Kamereo brought highly enriching and authentic real-world stories.
- Their sharing went beyond theory, diving deep into practical technical problems and management perspectives at large corporations.

#### Hands-on technical exposure
- Listening to the breakdown of each component in the URL Shortener architecture helped me clearly visualize how to combine cloud services to solve latency and scalability challenges.
- Understanding the real perspective of an experienced DevOps Engineer helped clear up common misconceptions about the profession, revealing the hidden challenges and guiding me to choose the right learning focus.

#### Impact on Mindset & Career Orientation
- Adopting the "No-Blame Post-Mortem" mindset changed my perspective on mistakes at work: an incident is an opportunity to fortify the system, not to assign blame.
- Clearly seeing the growth roadmap from a student to an AWS Community Builder and AWS Partner gave me a strong motivation to persistently accumulate knowledge and aim for new milestones.

#### Networking and discussions
- The event fostered an open atmosphere, providing opportunities for direct exchange and networking with industry experts.
- Reinforced the importance of **community involvement** (such as the AWS Student Builder Group and First Cloud AI Journey) to expand relationships and grow together.

#### Lessons learned
- Foundational knowledge and problem-solving mindsets are long-term assets, whereas tech tools will constantly change.
- Career success requires a harmonious combination of deep technical expertise and communication, inclusion, and cultural understanding skills.
- Always proactively learn, build practical products, and be ready to share knowledge back with the community.

#### Some event photos
*Add your event photos here* 
> In conclusion, the event provided a fresh perspective, valuable experiences, insights, and immense inspiration from the speakers, giving me a much clearer vision of my future career development path.
