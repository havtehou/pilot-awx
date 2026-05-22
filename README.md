# Azure Ansible Project

This project manages Azure resources using Ansible and the `azure.azcollection`.

## Prerequisites

1.  **Ansible & Collections:**
    ```bash
    cd ansible
    source .venv/bin/activate
    pip install -r requirements.txt
    ansible-galaxy collection install -r collections/requirements.yml
    ```

2.  **Authentication:**
    We use a Service Principal for authentication. You can store these in a `.env` file.

    ```bash
    cp .env.example .env
    # Edit .env with your actual credentials
    source .env
    ```

    **Note:** Ensure `.env` is added to your `.gitignore` to prevent committing secrets.

## Usage

### 1. Verify Inventory
```bash
ansible-inventory -i inventories/dev/ --graph
```

### 2. Deploy Infrastructure
```bash
# Deploy to Dev
ansible-playbook -i inventories/dev/ playbooks/site.yml

# Deploy to Staging
ansible-playbook -i inventories/staging/ playbooks/site.yml

# Deploy to Production
ansible-playbook -i inventories/prod/ playbooks/site.yml
```

### 3. Run from AWX/Tower
Use the AWX inventory when launching this playbook from AWX.
- Create or choose an AWX inventory.
- Create a Job Template using this project.
- Set the playbook to `playbooks/awx_inventory_test.yml`.
- Select the AWX inventory in the Job Template.
- Optionally add credentials for SSH or Azure service principal injection.

This playbook confirms AWX inventory hosts are reachable and prints the inventory host/group context.

## Project Structure
- `inventories/`: Environment-specific configs (Dev, Staging, Prod).
- `roles/`: Reusable logic for Resource Groups, VNets, VMs, and AKS.
- `playbooks/`: Orchestration entry points.
