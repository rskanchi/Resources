# Box CLI setup and usage on HPC Clusters

To upload folders directly from the HPC cluster to your Box account. 

Resources: 
[Box CLI quick start on Box Dev page](https://developer.box.com/guides/cli/quick-start/)  
[Box CLI on GitHub](https://github.com/box/boxcli)   


## 1. Installation

The installation process might vary depending on your HPC cluster's environment and your user privileges. 

**Option 1: Using a pre-installed package**

* Check if the Box CLI is already installed. Open a terminal on the HPC cluster and run:
    ```bash
    box --version
    ```
* If you see a version number, skip to the **3. Authentication** section.

**Option 2: Manual installation**

I had issues using curl and wget. So, manually downloaded the `box-v4.0.1-x64.exe` from 
[latest box CLI download page](https://github.com/box/boxcli/releases/). And transferred it to the `~/../tools/box` folder
where I wanted it. From the local terminal:  

    ```bash
    scp box-v4.0.1-x64.exe username@cluster:/path/to/tools/box/box
    ```

**Make the binary executable:**
On the cluster:  

    ```bash
    cd /path/to/tools/box
    chmod +x box-v4.0.1-x64
    ```
**Add directory to your PATH (optional):** 

To run `box` without specifying the full path. I created a sub-folder named `box` in my `/tools` folder. and added the path to my `.bashrc`.

* **Verify installation:**
    ```bash
    box --version
    ```

The above told me that the command was not found. 


## 2. Authentication

You need to authenticate the Box CLI with your Box account to grant it access. The recommended method is using the `configure:oauth` command.

1.  **Initiate OAuth 2.0 authentication:**
    ```bash
    box configure:oauth
    ```
2.  **Follow the instructions:** The CLI will provide a URL that you need to open in a web browser (usually on your local computer).
3.  **Authorize the Box CLI application:** Log in to your Box account in the browser and grant the "Box CLI" application the necessary permissions.
4.  **Copy the authorization code:** After successful authorization, Box will provide you with an authorization code. Copy this code.
5.  **Paste the authorization code:** Go back to your terminal on the HPC cluster and paste the authorization code when prompted.
6.  **Verification:** If the authentication is successful, you should see a confirmation message.

## 3. Usage: Uploading Folders

Once the Box CLI is set up and authenticated, you can upload folders using the `box files upload` command with the `--recursive` option.

```bash
box files upload --recursive /path/to/your/local/folder <BOX_FOLDER_ID>