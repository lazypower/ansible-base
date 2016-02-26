# Ansible Base Layer for Charms

This is an early prototype of the Ansible charm base layer, which includes the
batteries to write ansible based charms using layers. This ships with
`charms.ansible` which exposes both the bootstrapping of ansible on the host,
and exposes a natural language to wrap ansible execution.

This natrually integrates with `charms.reactive` states, by piping the hosts
states into the ansible playbook. If no matching states declared in the playbook
are found, the hook will error and the user/charm author will need to correct
the inconsistency due to how Ansible handles tags.

To schedule a play to run during hook execution, leverage a charm state.

@when('ansible.available')
def run_install_and_configure():
    set_state('cowsay')
    charms.ansible.apply_playbook('playbook.yaml')

Then in the corresponding playbook:

    - name: do something when ansible.available
      command: cowsay ansible is available
      tags:
        - cowsay

Anytime the state of ansible.available occurs, the playbook will be executed.

At this time, there are no corresponding state mechanism's within ansible and
they will need to be handled by the associated layer's calling hook code.

### Credit

This layer and library are heavilly influenced by the work of Michael Nelson.
Special thanks to mnelson and contributors to make this layer a reality. Which
serves as the basis for charms.ansible
