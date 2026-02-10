#analytics-development-lifecycle 

**Plan** - business question
**Develop** - SQL models with documentation and metadata, modular
**Test** - tests baked into code base, check for nulls, catch issues
**Deploy** - changes go live, can tie dbt to git repository, CI/CD
**Operate** - Manage production pipelines, schedule jobs, monitor runtimes, set up alerts, 
**Observe** - track what happens, did tests pass etc
**Discover** - Discover, explore logic, find answers, can trace lineage
**Analyse** - BI tool, conversation


**dbt as the Data Control Plane**

- dbt orchestrates and governs the ADLC across your data ecosystem.
    
- It ensures consistency in how data is developed, tested, documented, and deployed.
    
- By serving as the “control plane,” dbt integrates with the modern data stack to enforce trust, scalability, and readiness for AI-driven use cases.

**dbt Mesh** - domain level ownership without silos
**dbt catalog** - navigate, understand, improve, searchable
**dbt semantic** - define metrics once and use everywhere
**dbt build** - use dbt studio, canvas, VS Code ext, to write code
**Scheduler and CI** - CI/CD and orchestration
**Test & alert** - monitors everything

**The Analytics Development Lifecycle (ADLC)**

- Provides a structured process for building, testing, reviewing, and deploying analytics.
    
- Encourages iteration and collaboration so teams can confidently move from idea to production.
    
- Aligns data work with software engineering best practices—version control, testing, and continuous improvement.