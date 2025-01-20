# Ansible Group Tasks Example

This project demonstrates a custom solution for managing group-specific tasks in Ansible. It extends the familiar `group_vars` concept by introducing **group-specific tasks**, allowing tasks to be scoped directly to inventory groups without adding complex conditional logic to playbooks.

## Features

- **Group-Specific Tasks**: Define tasks that apply only to specific groups, just like variables in `group_vars`.
- **Dynamic Task Inclusion**: Automatically include tasks for groups present in the inventory.
- **Improved Readability**: Simplifies playbooks by offloading group-specific logic to dedicated files.

---

## File Structure

### Main Playbook

- **`group_tasks_example.yml`**: The main playbook that dynamically includes tasks for groups.

### Group Tasks Directory

- **`group_tasks/FORIS_TEST/pre_tasks.yml`**: Pre-tasks specific to the `FORIS_TEST` group.
- **`group_tasks/FORIS_TEST/post_tasks.yml`**: Post-tasks specific to the `FORIS_TEST` group.

---

## Usage

1. **Setup Inventory**:

   - Ensure your Ansible inventory includes the `FORIS_TEST` group or similar.

2. **Run the Playbook**:
   Execute the main playbook:

   ```bash
   ansible-playbook group_tasks_example.yml
   ```

3. **Dynamic Task Execution**:

   - Pre-tasks (`pre_tasks.yml`) and post-tasks (`post_tasks.yml`) in the `group_tasks/FORIS_TEST` directory will automatically run for hosts in the `FORIS_TEST` group.

---

## How It Works

- The playbook dynamically includes task files from the `group_tasks/` directory based on group names in the inventory.
- Example logic from `group_tasks_example.yml`:
  ```yaml
  - name: Include pre-tasks for group
    include_tasks: "group_tasks/{{ group_name }}/pre_tasks.yml"
    when: group_name in group_names

  - name: Include post-tasks for group
    include_tasks: "group_tasks/{{ group_name }}/post_tasks.yml"
    when: group_name in group_names
  ```

---

## Example Output

When running the playbook, you’ll see group-specific tasks executed based on the inventory:

```bash
PLAY [Run group-specific tasks] *************************************************

TASK [Include pre-tasks for group FORIS_TEST] ***********************************
ok: [host1]

TASK [FORIS_TEST pre-task example] **********************************************
ok: [host1]

TASK [FORIS_TEST post-task example] *********************************************
ok: [host1]

PLAY RECAP **********************************************************************
host1: ok=3    changed=0    unreachable=0    failed=0
```

---

## Notes

- Ensure the `group_tasks/` directory contains subdirectories matching group names.
- Tasks can be categorized as `pre_tasks`, `post_tasks`, or any other logical division.
- This implementation is not native to Ansible but provides a simple, extensible solution for managing group-specific tasks.

---


