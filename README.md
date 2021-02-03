# CircleCI Arm resource preview docs

We are excited to announce the preview of Arm resources on CircleCI. The purpose of this repository is to walk you through the setup steps required to use an Arm resource.

## What are Arm resources on CircleCI?

CircleCI offers multiple kinds of environments for you to run jobs in. In your CircleCI config you can choose the right environment for your job using the [`resource_class`](https://circleci.com/docs/2.0/configuration-reference/#resource_class) key.

We have added two resources as part of the [`machine` executor](https://circleci.com/docs/2.0/configuration-reference/#machine-executor-linux):

* `arm.medium` - `arm64` architecture, 2 vCPU, 8GB RAM
* `arm.large` - `arm64` architecture, 4 vCPU, 16GB RAM

These are the images available:

* `ubuntu-2004:202101-01` - most recent, recommended for all users
* `ubuntu-2004:202011-01` - deprecated as of Feb 3, 2021

As these are `machine` executor resources, each class is a dedicated VM that’s created just for your job and destroyed after the job has finished running.

## Getting access to the Arm resources

**Requirements** to get access to the Arm preview:

* Your organization must be a CircleCI customer on a Free, Performance, Scale, or Custom plan.
  * If you have never used CircleCI before, you can [sign up for a free account](https://circleci.com/signup/).
  
At this moment, Arm resources are only available on our cloud offering. If you are a CircleCI Server customer, you can try out Arm resources by signing up for a free CircleCI cloud account.
  
**Steps** to get access:

* Fill out the Arm access form:

### [Arm access form](https://form.asana.com/?k=S8EKGU3o66ld_qYXsdOQww&d=5374345383152)

We will do our best to provision access within 1-2 days of receiving your access request, subject to capacity in our fleet.

## Using Arm resources

Once we’ve enabled Arm resources for your organization, update your `.circleci/config.yml` file to use the Arm resource. Here is an example config:

```yaml
# .circleci/config.yml
version: 2.1

jobs:
  build-medium:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"
  build-large:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.large
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"

workflows:
  build:
    jobs:
      - build-medium
      - build-large
```

## Limitations

* Most orbs that include executables are **not** compatible with Arm at this moment. If you run into issues with orbs on Arm, please [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues).
* We currently don’t provide support for 32-bit Arm architectures. Only 64-bit `arm64` architecture is supported.
* There may be up to 2 mins of spin-up time before your job actually starts running. This time will decrease as more preview customers start using Arm resources.
* If there is software you require that’s not available in the image, please [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues) to let us know.
* We may change and update the pre-installed software on the `ubuntu-2004:202011-01` image without prior notice during the preview period. Once the preview period is over, the images for Arm resources will be stable and will follow our standard image release cadence.

## Pricing and availability

The following Arm resource class is available to all CircleCI customers:

|Resource class name|Specs|Pricing|Plans on which the resource is available|
|---|---|---|---|
|`arm.medium`|2 vCPUs, 8GB RAM |10 credits/min| Free, Performance, Scale, Custom|
|`arm.large` |4 vCPUs, 16GB RAM|20 credits/min| Performance, Scale              |

## How to provide feedback

Please [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues) with any feedback. The specific areas where we would appreciate feedback:

* Software pre-installed in the image. Do you find everything you need in the image? Are there any bugs that you’ve noticed in the pre-installed software?
* Naming of the resource class. Is the `arm.medium` and `arm.large` naming intuitive? Would you expect different names for these resources?
