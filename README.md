# Governance with Terraform Sentinel Policies

Sentinel gives operations teams the governance capabilities they need to ensure that all infrastructure provisioned with Terraform Enterprise complies with their organization's provisioning rules. The files under this directory provide some sample Sentinel policies for several clouds including AWS, Microsoft Azure, Google Cloud Platform (GCP), and VMware. The external directory also includes an example of an external data source and a Sentinel policy which checks the result of that data source.

In addition, Sentinel [mock files](https://www.terraform.io/docs/enterprise/sentinel/mock.html) and [test files](https://docs.hashicorp.com/sentinel/commands/config#test-cases) have been provided under the test directory of each cloud so that the policies can be tested with the [Sentinel Simulator](https://docs.hashicorp.com/sentinel/commands). The mocks were generated from actual Terraform plans run against Terraform code that provisioned resources in these clouds. The mock-tfplan-pass.sentinel and mock-tfplan-fail.sentinel files were edited to respectively pass and fail the associated Sentinel policies. For policies that have multiple rules, there are more than one failing mock file with names that indicate which condition or conditions they fail.

To test the policies of any of the clouds, please do the following:
1. Download the Sentinel Simulator from the [downloads](https://docs.hashicorp.com/sentinel/downloads) page.
1. Unzip the zip file and place the sentinel binary in your path.
1. Clone this repository to your local machine.
1. Navigate to any of the cloud directories (aws, azure, gcp, or vmware).
1. Run `sentinel test -verbose` to test all policies for that cloud.

Using the -verbose flag will show you the output that you would see if running the policies in TFE itself. You can drop it if you don't care about that output.

The Sentinel Simulator expects test cases to be in a test/\<policy\> directory under the directory containing the policy being tested where \<policy\> is the name of the policy not including the ".sentinel" extension. So, if you add new policies for any of the clouds, please be sure to create a new directory with the same name of the policy under that cloud's directory and then add pass.json, fail.json, mock-tfplan-pass.sentinel, and mock-tfplan-fail.sentinel files to that directory. Ensure that the pass and fail mocks cause the policy to pass and fail respectively. If you add a policy with multiple conditions, add mock files that fail each condition and one that fails all of them.

Policies that use the tfconfig import will require the addition of mock-tfconfig-pass.sentinel and  mock-tfconfig-fail.sentinel files that mock the configuration of relevant resources. Policies that use the tfstate import will require the addition of mock-tfstate-pass.sentinel and mock-tfstate-fail.sentinel files that mock the state of relevant resources. The pass.json and fail.json files would have to be modified to refer to these additional mock files. We have included some mocks of all 3 types for each cloud under the \<cloud\>/mocks driectory.
