========================
DevStack external plugin
========================


To Run DevStack with Full OpenStack Environment
-----------------------------------------------

1. Download DevStack
2. Prepare ``local.conf``
3. Run ``stack.sh``

There are more detailed info on the wiki.
https://github.com/midonet/midonet/wiki/Devstack


To Run DevStack with monolithic midonet plugin
-----------------------------------------------

1. Download DevStack
2. Copy the sample ``midonet/local.conf.sample`` file over to the devstack
   directory as ``local.conf``.
3. Run ``stack.sh``


To Run DevStack with ML2 and midonet mechanism driver
-----------------------------------------------------

1. Download DevStack
2. Copy the sample ``ml2/local.conf.sample`` file over to the devstack directory
   as ``local.conf``.
3. Run ``stack.sh``

Note that with these configurations, only the following services are started::

    rabbit
    mysql
    keystone
    nova
    glance
    neutron (with DHCP and metadata agents)
    lbaas
    tempest
    horizon


Plugin
------

There are two versions of MidoNet plugin.  Set MIDONET_PLUGIN local.conf
variable to the plugin that you want to load.

MidoNet plugin v1, which is compatible with MidoNet v2015.03 and v2015.06::

    MIDONET_PLUGIN=midonet

MidoNet plugin v2, which is compatible with MidoNet v5.0 and beyond::

    MIDONET_PLUGIN=midonet_v2


MidoNet Data Service
--------------------

On the master branch of MidoNet, there are two types of Zookeeper data store
engines available:

1. DataClient (legacy)
2. ZOOM (default)

By default, the ZOOM backend is enabled.  If you want to use the legacy
DataClient data store, set the following::

    MIDONET_USE_ZOOM=False

Also, MidoNet exposes two ways to communicate to its service:

1. REST (synchronous)
2. Tasks DB (asynchronous - experimental)

By default, the plugin is configured to use the REST API service.  The REST API
client is specified as::

    MIDONET_CLIENT=midonet.neutron.client.api.MidonetApiClient

If you want to use the experimental Tasks based API, set the following::

    MIDONET_CLIENT=midonet.neutron.client.cluster.MidonetClusterClient

There are three ways in which the Neutron plugin could access MidoNet:

1. MidoNet REST API with DataClient (legacy version)::

    MIDONET_PLUGIN=midonet
    MIDONET_CLIENT=midonet.neutron.client.api.MidonetApiClient
    MIDONET_USE_ZOOM=False

2. MidoNet REST API with ZOOM (current version)::

    MIDONET_PLUGIN=midonet_v2
    MIDONET_CLIENT=midonet.neutron.client.api.MidonetApiClient
    MIDONET_USE_ZOOM=True

3. MidoNet Tasks API with ZOOM (experimental version)::

    MIDONET_PLUGIN=midonet_v2
    MIDONET_CLIENT=midonet.neutron.client.cluster.MidonetClusterClient
    MIDONET_USE_ZOOM=True


LBaaS
-----

MidoNet plugin implements LBaaS v1 following the advanced service driver model.
To configure MidoNet as the LBaaS driver when running devstack, make sure the
following is defined in ``local.conf``::

    NEUTRON_LBAAS_SERVICE_PROVIDERV1="LOADBALANCER:Midonet:midonet.neutron.services.loadbalancer.driver.MidonetLoadbalancerDriver:default"


VPNaaS
------

Starting v5.1, MidoNet implements Neutron VPNaaS extension API.
This devstack plugin sets Midonet as the VPNaaS driver when the q-vpn service is
enabled in ``local.conf``.

NOTE: Currently, this devstack plugin doesn't install ipsec package "libreswan".
Please install it manually.


Gateway Device Management Service
---------------------------------

Starting v5.1, MidoNet implements
Neutron Gateway Device Management Service extension API.
To configure MidoNet including Gateway Device Management Service
when running devstack, make sure the following is defined in ``local.conf``::

    Q_SERVICE_PLUGIN_CLASSES=midonet.neutron.services.gw_device.plugin.MidonetGwDeviceServicePlugin


L2 Gateway Management Service
---------------------------------

Starting v5.1, MidoNet implements
Neutron L2 Gateway Management Service extension API.
To configure MidoNet including L2 Gateway Management Service
when running devstack, make sure the following is defined in ``local.conf``::

    enable_plugin networking-l2gw https://github.com/openstack/networking-l2gw
    enable_service l2gw-plugin
    Q_PLUGIN_EXTRA_CONF_PATH=/etc/neutron
    Q_PLUGIN_EXTRA_CONF_FILES=(l2gw_plugin.ini)
    L2GW_PLUGIN="midonet.neutron.services.l2gateway.plugin.MidonetL2GatewayPlugin"
