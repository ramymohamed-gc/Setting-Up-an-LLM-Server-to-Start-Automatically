# Setting Up an LLM Server to Start Automatically on AWS VM

This guide will help you configure a systemd service to automatically start your LLM server whenever your AWS VM boots.

## Prerequisites

- An AWS VM with SSH access.
- Anaconda installed on the VM.
- The `openllm` command available within a Conda environment.

## Step 1: Create a Systemd Service File

1. SSH into your VM.
2. Create a new systemd service file:

   ```bash
   sudo nano /etc/systemd/system/llm-server.service
   ```
3. Add the following content to the llm-server.service file:
    ```
    [Unit]
    Description=LLM Server
    After=network.target

    [Service]
    ExecStart=/home/ubuntu/anaconda3/bin/openllm serve llama3.1:8b-4bit
    WorkingDirectory=/home/ubuntu
    User=ubuntu
    Restart=always

    [Install]
    WantedBy=multi-user.target

    ```

## Step 2: Reload the Systemd Daemon 

- After creating the service file, reload the systemd daemon to apply the changes:
    ```
    sudo systemctl daemon-reload
    ```

## Step 3: Enable and Start the Service
- To ensure the service starts on boot, enable it:
    ```
    sudo systemctl enable llm-server.service
    ```

- Start the service immediately without needing to reboot:
    ```
    sudo systemctl start llm-server.service
    ```

## Step 4: Verify the Service Status
- Check the status of the service to confirm that it is running as expected:
    ```
    sudo systemctl status llm-server.service
    ```
    You should see output indicating that the service is active and running.

## Conclusion
With these steps, your LLM server will automatically start whenever your AWS VM boots, eliminating the need to manually SSH into the machine to start it.

