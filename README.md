## Amazon ECS "Set Container Environment in Task Definition" Action for GitHub Actions

Sets environment variables in a container definition in an Amazon ECS task definition JSON file, creating a new task definition file.

**Table of Contents**

<!-- toc -->

- [Usage](#usage)
- [License Summary](#license-summary)
- [Security Disclosures](#security-disclosures)

<!-- tocstop -->

## Usage

To insert the environment variable `APP_ENV` with the value `production` in the environment section for the `web` container in the task definition file, and then deploy the edited task definition file to ECS:

```yaml
- name: Set environment variable in Amazon ECS task definition
  id: set-env-var-web-container
  uses: clarifydata/amazon-ecs-task-definition-set-environment@v1
  with:
    task-definition: task-definition.json
    container-name: web
    env-var-name: APP_ENV
    env-var-value: production

- name: Deploy to Amazon ECS service
  uses: aws-actions/amazon-ecs-deploy-task-definition@v1
  with:
    task-definition: ${{ steps.set-env-web-container.outputs.task-definition }}
    service: my-service
    cluster: my-cluster
```

If you need to set multiple environment variables for one or more containers, chain multiple executions of this action
together using the output value from the first action for the `task-definition`
input of the second:

```yaml
- name: Set first environment variable in Amazon ECS task definition
  id: set-first-env-var-web-container
  uses: clarifydata/amazon-ecs-task-definition-set-environment@v1
  with:
    task-definition: task-definition.json
    container-name: web
    env-var-name: APP_ENV
    env-var-value: production

- name: Modify Amazon ECS task definition with second environment variable
  id: set-second-env-var-web-container
  uses: clarifydata/amazon-ecs-task-definition-set-environment@v1
  with:
    task-definition: ${{ steps.set-first-env-var-web-container.outputs.task-definition }}
    container-name: web
    env-var-name: APP_PORT
    env-var-value: 8080

- name: Deploy to Amazon ECS service
  uses: aws-actions/amazon-ecs-deploy-task-definition@v1
  with:
    task-definition: ${{ steps.set-second-env-var-web-container.outputs.task-definition }}
    service: my-service
    cluster: my-cluster
```

See [action.yml](action.yml) for the full documentation for this action's inputs and outputs.

## License Summary

This code is made available under the MIT license.

## Security Disclosures

If you would like to report a potential security issue in this project, please do not create a GitHub issue. Instead, please follow the instructions [here](https://aws.amazon.com/security/vulnerability-reporting/) or [email AWS security directly](mailto:aws-security@amazon.com).
