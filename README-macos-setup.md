# macOS Update Playbook Setup

## SSH Key Setup

Since your Ansible control node is on **10.0.200.102** (not your local Mac), you need to copy the SSH key from that machine to your MacBook.

### Steps:

1. **SSH into your Ansible control node:**
   ```bash
   ssh root@10.0.200.102
   ```

2. **Generate SSH key on the Ansible host (if not already done):**
   ```bash
   ssh-keygen -t ed25519 -C "ansible-macos"
   ```
   Press Enter to accept defaults.

3. **Copy the SSH key to your MacBook:**
   ```bash
   ssh-copy-id -i ~/.ssh/id_ed25519.pub luiscruzeiro@10.0.200.24
   ```
   You'll be prompted for your MacBook password.

4. **Test the connection:**
   ```bash
   ssh luiscruzeiro@10.0.200.24 "echo 'SSH works!'"
   ```
   This should work without asking for a password.

## Running the Playbook

Once SSH is set up, run the playbook from your Ansible control node (10.0.200.102):

```bash
ansible-playbook -i inventory-macos.yml playbook-macos-update.yml --ask-become-pass
```

The `--ask-become-pass` flag will prompt for your MacBook's sudo password to install system updates.

## What the Playbook Does

1. **Lists available macOS system updates**
2. **Installs all recommended system updates** (requires sudo)
3. **Updates Homebrew** (if installed)
4. **Upgrades all Homebrew packages** (if installed)
5. **Cleans up old Homebrew files**

## Notes

- The playbook automatically detects if Homebrew is installed (works with both Intel and Apple Silicon Macs)
- System updates may require a restart - the playbook will notify you
- You can run this playbook regularly to keep your Mac up to date
