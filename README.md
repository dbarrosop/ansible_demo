# Usage

To run the playbook without committing changes:

    ansible-playbook -i network.hosts configure_network.yml -e "commit_changes=0"

To run the playbook committing the changes:

    ansible-playbook -i network.hosts configure_network.yml -e "commit_changes=1"
    
# Disclaimer

This is just a demo. The final configuration is not meant to be production ready. The playbook and the code displayed here is distributed as it is just for informational purposes. Use it at your own risk.

# Author

David Barroso <dbarroso@spotify.com>
