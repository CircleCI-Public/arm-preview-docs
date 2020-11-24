# CircleCI Arm resource preview docs

We are excited to announce the preview of Arm resources on CircleCI. The purpose of this repository is to walk you through the setup steps required to use an Arm resource.

## What are Arm resources on CircleCI?

CircleCI offers multiple kinds of environments for you to run jobs in. In your CircleCI config you can choose the right environment for your job using the [`resource_class`](https://circleci.com/docs/2.0/configuration-reference/#resource_class) key.

We have added two resources as part of the [`machine` executor](https://circleci.com/docs/2.0/configuration-reference/#machine-executor-linux):

* `arm.medium` - 2 vCPU, 8GB RAM
* `arm.large` - 4 vCPU, 16GB RAM

As these are `machine` executor resources, each class is a dedicated VM that’s created just for your job and destroyed after the job has finished running.

## Getting access to the Arm resources

**Requirements** to get access to the Arm preview:

* Your organization must have an active Performance, Scale, or Custom plan on CircleCI.

**Steps** to get access:

* Contact your CircleCI Customer Success Manager (CSM). If you don’t have a CSM, submit a new request in the [CircleCI Support Center](https://support.circleci.com/hc/en-us).

## Using Arm resources

Once we’ve enabled Arm resources for your organization, update your `.circleci/config.yml` file to use the Arm resource. Here is an example config:

```yaml
# .circleci/config.yml
version: 2.1

jobs:
  build-medium:
    machine:
      image: ubuntu-2004:202011-01
    resource_class: arm.medium
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"
  build-large:
    machine:
      image: ubuntu-2004:202011-01
    resource_class: arm.large
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"

workflows:
  version: 2
  build:
    jobs:
      - build-medium
      - build-large
```

## Limitations

* There may be up to 2 mins of spin-up time before your job actually starts running. This time will decrease as more preview customers start using Arm resources.
* Only one image is currently available, `ubuntu-2004:202011-01`. It contains most of the tools you’ll likely need, from Docker to `docker-compose` to Python to `jq`. If there is software you require that’s not available in the image, please [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues) to let us know.

## Pricing

We are currently finalizing the pricing for Arm resources and will share it soon. For now, we won’t bill you for the Arm preview usage.

## How to provide feedback

Please [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues) with any feedback. The specific areas where we would appreciate feedback:

* Software pre-installed in the image. Do you find everything you need in the image? Are there any bugs that you’ve noticed in the pre-installed software?
* Naming of the resource class. Is the `arm.medium` and `arm.large` naming intuitive? Would you expect different names for these resources?
