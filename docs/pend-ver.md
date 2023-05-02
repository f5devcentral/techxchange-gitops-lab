# PendingVerification Error

If you receive a *PendingVerification* error from AWS in your Terraform output, then proceed with the following steps:

1. Destroy the **aws-appstack-site**:

    ```bash
    cd aws-appstack-site-1
    terragrunt destroy
    ```

1. Ensure your site has been removed by checking the *Multi-Cloud App Connect -> App Site List* to ensure there are no sites with your XC username.

1. Run the deployment again

    ```.bash
    cd ~/terraform-modular-demo-framework
    ./gitops-lab/setup-lab-environment.sh
    ```

1. [Continue with the lab](introduction.md#configure-git-in-visual-studio-code).
