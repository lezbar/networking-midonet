Rally Neutron Extras Plugin
===========================

This module adds create only tasks in Neutron as the defaults in the Rally
code base benchmarks allows create_delete/list/update operations only.

Copy this directory in "~/.rally" to be automatically discovered by Rally.

Set the following variables in the create_*.yaml jobs to override the defaults.

    {% set rps_scalability = 0 %}
    {% set times_scalability = 0 %}
    {% set sla_scalability = 0 %}

Note: create_neutron_routers.yaml requires "add_router=False" in setup() on
rally/plugins/openstack/context/network/networks.py
