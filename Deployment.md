
# **Serenus Backend Deployment Documentation**

### **1. Install Gunicorn**

First, ensure that Gunicorn is installed in your virtual environment:

```bash

pip install gunicorn
```

### **2. Create a Systemd Service File**

Create a new systemd service file to manage the Gunicorn instance. This file defines how the Gunicorn server will be started, stopped, and restarted.

1. **Open a new file:**

   ```bash
   sudo nano /etc/systemd/system/serenus.service
   ```

2. **Add the following content to the file:**

   ```ini
   [Unit]
   Description=Gunicorn instance to serve Serenus
   After=network.target

   [Service]
   User=sorwar
   Group=www-data
   WorkingDirectory=/home/sorwar/Documents/Serenus_pne_project/backend/serenus-ai-backend
   ExecStart=/home/sorwar/Documents/Serenus_pne_project/backend/serenus-ai-backend/.venv/bin/gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app

   [Install]
   WantedBy=multi-user.target
   ```

   - **User**: Replace `sorwar` with the correct user who will run the service.
   - **Group**: `www-data` is generally the web server group; you may change it based on your setup.
   - **WorkingDirectory**: This should be the directory where your project resides.
   - **ExecStart**: This specifies the command to start the Gunicorn server, including the number of workers (`-w 1`), the worker class (`-k uvicorn.workers.UvicornWorker`), and the module to serve (`main:app`).

### **3. Start and Enable the Service**

1. **Reload the systemd daemon to recognize the new service:**

   ```bash
   sudo systemctl daemon-reload
   ```

2. **Start the Gunicorn service:**

   ```bash
   sudo systemctl start serenus.service
   ```

3. **Enable the service to start on boot:**

   ```bash
   sudo systemctl enable serenus.service
   ```

### **4. Verify the Service**

1. **Check the status of the service to ensure it's running:**

   ```bash
   sudo systemctl status serenus.service
   ```

   You should see an output indicating that the service is active and running.

2. **If necessary, view logs to debug any issues:**

   ```bash
   sudo journalctl -u serenus.service
   ```

### **5. Restarting and Stopping the Service**

- **To restart the service:**

  ```bash
  sudo systemctl restart serenus.service
  ```

- **To stop the service:**

  ```bash
  sudo systemctl stop serenus.service
  ```

---
