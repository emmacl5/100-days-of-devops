### 2. Grant execute permission to all users
```bash
sudo chmod 755 /tmp/xfusioncop.sh

- `chmod 755` sets the file permissions to `rwxr-xr-x`
- this ensures the owner has full access, while group and others can read and execute
- this approach guarantees consistent permissions, which is often required in automated checks
