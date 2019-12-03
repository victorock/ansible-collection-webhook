# webhook_handler
Tower Webhook Handler for CI/CD pipelines

Events References:  

Github: https://developer.github.com/v3/activity/events/types/  
Gitlab: https://docs.gitlab.com/ee/user/project/integrations/webhooks.html  

Example of `.tower.yaml` file hosted in the repository, defining:

- `workflows` the workflows adopting pre-existing job_templates in tower.

- `pipelines` the pipelines based on webhook events.

```YAML
---
workflows:
  # Workflow to Build
  - name: "myapp.build"
    schema:
      - job_template: "myapp.build.step1"
        success:
        - job_template: "myapp.build.step2"
          success:
          - job_template: "myapp.build.step3"

  # Workflow to Test
  - name: "myapp.test"
    schema:
      - job_template: "myapp.test.step1"
        success:
        - job_template: "myapp.test.step2"
          success:
          - job_template: "myapp.test.step3"

  # Workflow to Deploy
  - name: "myapp.deploy"
    schema:
      - job_template: "myapp.deploy.step1"
        success:
        - job_template: "myapp.deploy.step2"
          success:
          - job_template: "myapp.deploy.step3"

  - name: "myapp.pipeline"
    schema:
      - workflow_template: "myapp.pipeline.build"
        success:
        - job_template: "myapp.pipeline.test"
          success:
          - job_template: "myapp.pipeline.deploy"

# Only triggers workflows
pipelines:
  - name: "myapp.build"
    on: 
    - "push"
  - name: "myapp.pipeline"
    on:
    - "release"
```
