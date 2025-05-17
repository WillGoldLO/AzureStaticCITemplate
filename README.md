# AzureStaticCITemplate
Azure static website CI/CD deployment template. (.NET 9.0)

Automatically deploys all pushed commits to a development site using the token associated with the development Azure static website resource.
Expects a PR in order to merge to main, and upon PR completion, deploys the code to the production branch which also uses the token associated with the production Azure static website resource.

Assumes:
1. Blazor WebAssembly is the project type.
2. Your .sln file is in a folder with the same name as the project and the .csproj file is in a subfolder of that with the same name.
3. There is no API associated and this is just a simple static website.

This works with .NET 9.0 and resolves some build issues with the standard template and the Oryx compiler in the early days after .NET 9.0 went GA.
It uses a custom build_and_deploy stage and does not rely on Oryx to do this because output paths and release folders were problematic for some site architectures when building a Blazor WebAssembly app.

Must update these fields to match your project (in the env: section):
1. APP_NAME - set this to the name of your csproj file.
2. REPO_TOKEN - set this to the secrets variable in GitHub containing the token for your repo containing your source code.
3. WEB_API_TOKEN - set this to the secrets variable in GitHub containing the deployment token for the site that is associated with the matching custom domain name and set the value in GitHub to the value Azure gave you.
   
Must name the deployment files properly as Azure static websites require your GitHub action files to be the same as the custom domain that Azure automatically reserves for your resource upon creation with a yml fiel extension.  This file cannot be named in any other way or Azure will not run the action to deploy your code in the respective environment.
