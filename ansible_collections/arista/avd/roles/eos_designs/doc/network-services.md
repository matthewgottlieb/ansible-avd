# Network Services

- The network services variables provide an abstracted model to create L2 and L3 network services across the fabric.
- The network services are grouped by tenants. The definition of a tenant may vary between organizations.
  - e.g. Tenants can be organizations or departments.
- The tenant shares a common vni range for mac vrf assignment.
- The filtering model allows for granular deployment of network service to the fabric leveraging the tenant name and tags applied to the service definition.
  - This allows for the re-use of SVIs and VLANs across the fabric.

## Variables and Options

### mlag_ibgp_peering_vrfs

On mlag leafs, an SVI interface is defined per vrf, to establish iBGP peering.
The SVI id will be derived from the base vlan defined: ` mlag_ibgp_peering_vrfs.base_vlan + vrf_vni - 1`.
The SVI ip address derived from `mlag_l3_peer_ipv4_pool` is re-used across all iBGP peerings.

```yaml
# mlag_ibgp_peering_vrfs | Optional
mlag_ibgp_peering_vrfs: 
  # Base VLAN < 1-4000 > | Optional
  base_vlan: <int, default: 3000>
```

<details>
<summary>Schema</summary>

```yaml
mlag_ibgp_peering_vrfs:
  description: 'On mlag leafs, an SVI interface is defined per vrf, to establish iBGP
    peering.

    The SVI id will be derived from the base vlan defined: ` mlag_ibgp_peering_vrfs.base_vlan
    + vrf_vni - 1`.

    The SVI ip address derived from `mlag_l3_peer_ipv4_pool` is re-used across all iBGP
    peerings.'
  options:
    base_vlan:
      default: 3000
      description: Base VLAN < 1-4000 >
      type: int
  type: dict
```

</details>

### evpn_rd_type

Route Distinguisher (RD) for L2 / L3 services is set to <overlay_loopback>:<vni> per default.

By configuring evpn_rd_type the Administrator subfield (first part of RD) can be set to other values.

Note:

RD is a 48-bit value which is split into <16-bit>:<32-bit> or <32-bit>:<16-bit>.

For loopback or 32-bit ASN/number the VNI can only be a 16-bit number.

For 16-bit ASN/number the VNI can be a 32-bit number.

```yaml
# evpn_rd_type | Optional
evpn_rd_type: 
  # admin_subfield | Optional
  admin_subfield: <str, options: [overlay_loopback | vtep_loopback | bgp_as | < IPv4 Address > | <0-65535> | <0-4294967295>], default: overlay_loopback>
```

<details>
<summary>Schema</summary>

```yaml
evpn_rd_type:
  description: 'Route Distinguisher (RD) for L2 / L3 services is set to <overlay_loopback>:<vni>
    per default.


    By configuring evpn_rd_type the Administrator subfield (first part of RD) can be
    set to other values.


    Note:


    RD is a 48-bit value which is split into <16-bit>:<32-bit> or <32-bit>:<16-bit>.


    For loopback or 32-bit ASN/number the VNI can only be a 16-bit number.


    For 16-bit ASN/number the VNI can be a 32-bit number.'
  options:
    admin_subfield:
      choices:
      - overlay_loopback
      - vtep_loopback
      - bgp_as
      - < IPv4 Address >
      - <0-65535>
      - <0-4294967295>
      default: overlay_loopback
      type: str
  type: dict
```

</details>

### evpn_rt_type

Route Target (RT) for L2 / L3 services is set to <vni>:<vni> per default

By configuring evpn_rt_type the Administrator subfield (first part of RT) can be set to other values.

Note:

RT is a 48-bit value which is split into <16-bit>:<32-bit> or <32-bit>:<16-bit>.

For 32-bit ASN/number the VNI can only be a 16-bit number.

For 16-bit ASN/number the VNI can be a 32-bit number.

```yaml
# evpn_rt_type | Optional
evpn_rt_type: 
  # admin_subfield | Optional
  admin_subfield: <str, options: [bgp_as | vni | <0-65535> | <0-4294967295>], default: vni>
```

<details>
<summary>Schema</summary>

```yaml
evpn_rt_type:
  description: 'Route Target (RT) for L2 / L3 services is set to <vni>:<vni> per default


    By configuring evpn_rt_type the Administrator subfield (first part of RT) can be
    set to other values.


    Note:


    RT is a 48-bit value which is split into <16-bit>:<32-bit> or <32-bit>:<16-bit>.


    For 32-bit ASN/number the VNI can only be a 16-bit number.


    For 16-bit ASN/number the VNI can be a 32-bit number.'
  options:
    admin_subfield:
      choices:
      - bgp_as
      - vni
      - <0-65535>
      - <0-4294967295>
      default: vni
      type: str
  type: dict
```

</details>

### internal_vlan_order

Internal vlan allocation order and range

```yaml
# internal_vlan_order | Required
internal_vlan_order: 
  # allocation | Optional
  allocation: <str, options: [ascending | descending], default: ascending>

  # range | Optional
  range: 
    # First VLAN ID | Optional
    beginning: <int, default: 1006>

    # Last VLAN ID | Optional
    ending: <int, default: 1199>
```

<details>
<summary>Schema</summary>

```yaml
internal_vlan_order:
  description: Internal vlan allocation order and range
  options:
    allocation:
      choices:
      - ascending
      - descending
      default: ascending
      type: str
    range:
      options:
        beginning:
          default: 1006
          description: First VLAN ID
          type: int
        ending:
          default: 1199
          description: Last VLAN ID
          type: int
      type: dict
  required: true
  type: dict
```

</details>

### mac_address_table

MAC address-table aging time. Use to change the EOS default of 300

```yaml
# mac_address_table | Optional
mac_address_table: 
  # Aging time in seconds | Optional
  aging_time: <int>
```

<details>
<summary>Schema</summary>

```yaml
mac_address_table:
  description: MAC address-table aging time. Use to change the EOS default of 300
  options:
    aging_time:
      description: Aging time in seconds
      type: int
  type: dict
```

</details>

### svi_profiles

Optional profiles to apply on SVI interfaces

Each profile can support all or some of the following keys according to your own needs.

Keys are the same used under SVI.

```yaml
# svi_profiles | Optional
svi_profiles: 
    # Profile name | Required
  - name: <str (unique)>

    # Interface MTU | Optional
    mtu: <int>

    # Enable or disable SVI interface | Optional
    enabled: <bool, default: False>

    # ip virtual-router address
    note, also requires an IP address to be configured on the SVI where it is applied. | Optional
    ip_virtual_router_addresses: 
      - <string, options: [< IPv4_address > | < IPv4_address/Mask >]>

    # ip address virtual to configure VXLAN Anycast IP address
    Conserves IP addresses in VXLAN deployments as it doesn't require unique IP addresses on each node. | Optional
    ip_address_virtual: <str, options: [< IPv4_address/Mask >]>

    # ip_address_virtual_secondaries | Optional
    ip_address_virtual_secondaries: 
      - <string, options: [< IPv4_address/Mask >]>

    # Enable IGMP Snooping ("on" by default in EOS) | Optional
    igmp_snooping_enabled: <bool>

    # IP Helpers for DHCP relay | Optional
    ip_helpers: 
        # DNS Name or IP address | Required
      - name: <str (unique)>

        # Source interface name | Optional
        source_interface: <str>

        # VRF to originate DHCP relay packets to DHCP server | Optional
        source_vrf: <str>
```

<details>
<summary>Schema</summary>

```yaml
svi_profiles:
  description: 'Optional profiles to apply on SVI interfaces


    Each profile can support all or some of the following keys according to your own
    needs.


    Keys are the same used under SVI.'
  elements: dict
  options:
    enabled:
      default: false
      description: Enable or disable SVI interface
      type: bool
    igmp_snooping_enabled:
      description: Enable IGMP Snooping ("on" by default in EOS)
      type: bool
    ip_address_virtual:
      choices:
      - < IPv4_address/Mask >
      description: 'ip address virtual to configure VXLAN Anycast IP address

        Conserves IP addresses in VXLAN deployments as it doesn''t require unique IP
        addresses on each node.'
      type: str
    ip_address_virtual_secondaries:
      choices:
      - < IPv4_address/Mask >
      elements: string
      type: string
    ip_helpers:
      description: IP Helpers for DHCP relay
      elements: dict
      options:
        name:
          description: DNS Name or IP address
          required: true
          type: str
          unique: true
        source_interface:
          description: Source interface name
          type: str
        source_vrf:
          description: VRF to originate DHCP relay packets to DHCP server
          type: str
      type: list
    ip_virtual_router_addresses:
      choices:
      - < IPv4_address >
      - < IPv4_address/Mask >
      description: 'ip virtual-router address

        note, also requires an IP address to be configured on the SVI where it is applied.'
      elements: string
      type: string
    mtu:
      description: Interface MTU
      type: int
    name:
      description: Profile name
      required: true
      type: str
      unique: true
  type: list
```

</details>

### tenants

List of tenants, to define network services; L3 VRFs and L2 VLANS.

```yaml
# tenants | Optional
tenants: 
    # Specify a tenant name.
    Tenant provide a construct to group L3 VRFs and L2 VLANs.
    Networks services can be filtered by tenant name. | Required
  - name: <str (unique)>

    # VXLAN Network Identifier for MAC VRF
    VXLAN VNI is derived from the base number with simple addition.
    e.g. mac_vrf_vni_base = 10000, svi 100 = VNI 10100, svi 300 = VNI 10300. | Required
    mac_vrf_vni_base: <int, options: [< 10000-16770000 >]>

    # Base number for vlan_aware_bundle
    The "Assigned Number" part of RD/RT is derived from vrf_vni + vlan_aware_bundle_number_base. | Optional
    vlan_aware_bundle_number_base: <int, default: 0>

    # MLAG IBGP peering per VRF
    By default an IBGP peering is configured per VRF between MLAG peers on separate VLANs.
    Setting enable_mlag_ibgp_peering_vrfs: false under tenant will change this default to prevent configuration of these peerings and VLANs for all VRFs in the tenant.
    This setting can be overridden per VRF. | Optional
    enable_mlag_ibgp_peering_vrfs: <bool, default: True>

    # Define L3 network services organized by vrf. | Optional
    vrfs: 
        # VRF name | Required
      - name: <str (unique)>

        # VRF VNI - required if vrf_id is not set
        The VRF VNI range is not limited, but it is recommended to keep vrf_vni <= 1024
        It is necessary to keep [ vrf_vni + MLAG IBGP base_vlan ] < 4094 to support MLAG IBGP peering in VRF.
        If vrf_vni > 1094 make sure to change mlag_ibgp_peering_vrfs: { base_vlan: < > } to a lower value (default 3000).
        If vrf_vni > 10000 make sure to adjust mac_vrf_vni_base accordingly to avoid overlap. | Optional
        vrf_vni: <int, default: < vrf_id >>

        # IP Helpers for DHCP relay | Optional
        ip_helpers: 
            # DNS Name or IP address | Required
          - name: <str (unique)>

            # Source interface name | Optional
            source_interface: <str>

            # VRF to originate DHCP relay packets to DHCP server | Optional
            source_vrf: <str>

        # MLAG IBGP peering per VRF
        By default an IBGP peering is configured per VRF between MLAG peers on separate VLANs.
        Setting enable_mlag_ibgp_peering_vrfs: false under vrf will change this default and/or override the tenant-wide setting | Optional
        enable_mlag_ibgp_peering_vrfs: <bool, default: True>

        # Manually define the VLAN used on the MLAG pair for the iBGP session.
        By default this parameter is calculated using the following formula: <base_vlan> + <vrf_vni> - 1 | Optional
        mlag_ibgp_peering_vlan: <int, options: [< 1-4094 >]>

        # Enable VTEP Network diagnostics
        This will create a loopback with virtual source-nat enable to perform diagnostics from the switch. | Optional
        vtep_diagnostic: 
          # Loopback interface number | Required
          loopback: <int, options: [< 2-2100 >]>

          # Loopback ip range, a unique ip is derived from this ranged and assigned
          to each l3 leaf based on it's unique id. Loopback is not created unless either loopback_ip_range or loopback_ip_pools are set. | Optional
          loopback_ip_range: <str, options: [< IPv4_address/Mask >]>

          # For inventories with multiple PODs a loopback range can be set per POD to avoid overlaps.
          This only takes effect when loopback_ip_range is not defined. Loopback is not created unless either loopback_ip_range or loopback_ip_pools are set. | Optional
          loopback_ip_pools: 
              # Name matching "pod_name" | Required
            - pod: <str>

              # ipv4_pool | Required
              ipv4_pool: <str, options: [< IPv4_address/Mask >]>

        # Dictionary of SVIs
        This will create both the L3 SVI and L2 VLAN based on filters applied to l3leaf and l2leaf. | Required
        svis: 
            # SVI interface id and VLAN id | Required
          - id: <int (unique), options: [< 1-4096 >]>

            # By default the vni will be derived from "mac_vrf_vni_base"
            The vni_override allows us to override this value and statically define it. | Optional
            vni_override: <int, options: [< 1-16777215 >]>

            # Name of SVI profile to apply If variables are configured in profile AND SVI, SVI information will overwrite profile. | Optional
            profile: <str>

            # Vlan name | Required
            name: <str>

            # description | Optional
            description: <str, default: < vlan_name >>

            # Tags leveraged for networks services filtering. | Required
            tags: 
              - <str>

            # Enable or disable SVI interface | Optional
            enabled: <bool, default: False>

            # Enable IGMP Snooping ("on" by default in EOS) | Optional
            igmp_snooping_enabled: <bool>

            # ip address virtual to configure VXLAN Anycast IP address
            Conserves IP addresses in VXLAN deployments as it doesn't require unique IP addresses on each node. | Optional
            ip_address_virtual: <str, options: [< IPv4_address/Mask >]>

            # ip_address_virtual_secondaries | Optional
            ip_address_virtual_secondaries: 
              - <string, options: [< IPv4_address/Mask >]>

            # ip virtual-router address
            note, also requires an IP address to be configured on the SVI where it is applied. | Optional
            ip_virtual_router_addresses: 
              - <string, options: [< IPv4_address > | < IPv4_address/Mask >]>

            # IP Helpers for DHCP relay | Optional
            ip_helpers: 
                # DNS Name or IP address | Required
              - name: <str (unique)>

                # Source interface name | Optional
                source_interface: <str>

                # VRF to originate DHCP relay packets to DHCP server. | Optional
                source_vrf: <str>

            # Extend this SVI over VXLAN | Optional
            vxlan: <bool, default: True>

            # Define node specific configuration, such as unique IP addresses. | Optional
            nodes: 
                # Hostname of node | Required
              - name: <str>

                # device unique IP address for node. | Optional
                ip_address: <str, options: [< |   | I | P | v | 4 | _ | a | d | d | r | e | s | s | / | M | a | s | k |   | >]>

                # EOS CLI rendered directly on the VLAN interface in the final EOS configuration Overrides the setting on SVI level. Supports Multiline strings | Optional
                raw_eos_cli: <str>

                # Custom structured config added under vlan_interfaces.<interface> for eos_cli_config_gen
                Overrides the setting on SVI level. | Optional
                structured_config: <any>

            # Interface MTU | Optional
            mtu: <int>

            # EOS CLI rendered directly on the VLAN interface in the final EOS configuration Supports Multiline strings | Optional
            raw_eos_cli: <str>

            # Custom structured config added under vlan_interfaces.<interface> for eos_cli_config_gen | Optional
            structured_config: <any>

        # List of L3 interfaces
        This will create IP routed interface inside VRF. Length of interfaces, nodes and ip_addresses must match. | Optional
        l3_interfaces: 
            # Interface Name | Optional
          - interfaces: 
              - <str>

            # ip_addresses | Optional
            ip_addresses: 
              - <str, options: [<IPv4_address/Mask>]>

            # Hostname of Node | Optional
            nodes: 
              - <str>

            # Interface Description | Optional
            description: <str>

            # Enable or disable L3 interface | Optional
            enabled: <bool, default: False>

            # Interface MTU | Optional
            mtu: <int>

            # EOS CLI rendered directly on the Ethernet interface in the final EOS configuration Supports Multiline strings | Optional
            raw_eos_cli: <str>

            # Custom structured config added under ethernet_interfaces.<interface> for eos_cli_config_gen | Optional
            structured_config: <any>

            # For sub-interfaces the dot1q vlan is derived from the interface name by default, but can also be specified. | Optional
            encapsulation_dot1q_vlan: 
              - <int>

        # This will create static routes inside the tenant VRF.
        If nodes are not specified, all l3leafs that carry the VRF will also be applied the static routes.
        If a node has a static route in the VRF, redistribute static will be automatically enabled in that VRF.
        This automatic behavior can be overridden non-selectively with the redistribute_static knob for the VRF. | Optional
        static_routes: 
            # destination_address_prefix | Required
          - destination_address_prefix: <str, options: [< IPv4_address/Mask >]>

            # gateway | Optional
            gateway: <str, options: [< IPv4_address >]>

            # distance | Optional
            distance: <int, options: [< 1-255 >]>

            # tag | Optional
            tag: <int, options: [< 0-4294967295 >]>

            # Name/description for static route | Optional
            name: <str>

            # metric | Optional
            metric: <int, options: [< 0-4294967295 >]>

            # interface | Optional
            interface: <str>

            # Hostname of Node | Optional
            nodes: 
              - <str>

        # Non-selectively enabling or disabling redistribute static inside the VRF | Optional
        redistribute_static: <bool>

        # This will configure BGP neighbors inside the tenant VRF for peering with external devices. The configured peer will automatically be activated for ipv4 or ipv6 address family based on the ip address. Note, only ipv4 and ipv6 address families are currently supported in eos_designs. For other address families, use custom_structured configuration with eos_cli_config_gen. | Optional
        bgp_peers: 
            # ip_address | Required
          - ip_address: <str (unique), options: [< IPv4_address > | < IPv6_address >]>

            # Remote ASN | Optional
            remote_as: <int>

            # description | Optional
            description: <str>

            # Encrypted password | Optional
            password: <str>

            # send_community | Optional
            send_community: <str, options: [standard | extended | large | all]>

            # next_hop_self | Optional
            next_hop_self: <bool>

            # maximum_routes | Optional
            maximum_routes: <int>

            # default_originate | Optional
            default_originate: 
              # always | Optional
              always: <bool>

            # Interface name | Optional
            update_source: <str>

            # ebgp_multihop | Optional
            ebgp_multihop: <int, options: [< 1-255 >]>

            # Nodes are required to restrict configuration of BGP neighbors to certain nodes in the network. | Optional
            nodes: 
              - <str>

            # Next hop settings can be either ipv4 or ipv6 for one neighbor, this will be applied by a uniquely generated route-map per neighbor. Next hop takes precedence over route_map_out. | Optional
            set_ipv4_next_hop: <str, options: [< IPv4_address >]>

            # Next hop settings can be either ipv4 or ipv6 for one neighbor, this will be applied by a uniquely generated route-map per neighbor. Next hop takes precedence over route_map_out. | Optional
            set_ipv6_next_hop: <str, options: [< IPv6_address >]>

            # Route-map name | Optional
            route_map_out: <str>

            # Route-map name | Optional
            route_map_in: <st>

            # local_as | Optional
            local_as: <int>

            # weight | Optional
            weight: <int, options: [< 0-65535>]>

        # bgp | Optional
        bgp: 
          # EOS CLI rendered directly on the Router BGP, VRF definition in the final EOS configuration Supports Multiline strings | Optional
          raw_eos_cli: <str>

          # Custom structured config added under router_bgp.vrfs.<vrf> for eos_cli_config_gen | Optional
          structured_config: <any>

        # Optional configuration of extra route-targets for this VRF. Useful for route-leaking or gateway between address families. | Optional
        additional_route_targets: 
            # type | Optional
          - type: <str, options: [import | export]>

            # address_family | Optional
            address_family: <str>

            # route_target | Optional
            route_target: <str>

            # Nodes key is optional. Default is all nodes where the VRF is defined. | Optional
            nodes: 
              - <str>

        # EOS CLI rendered directly on the root level of the final EOS configuration Supports Multiline strings | Optional
        raw_eos_cli: <str>

        # Custom structured config for eos_cli_config_gen | Optional
        structured_config: <any>

    # Define L2 network services organized by vlan id. | Optional
    l2vlans: 
        # VLAN id | Required
      - id: <int (unique), options: [< 1-4096 >]>

        # By default the vni will be derived from "mac_vrf_vni_base"
        The vni_override allows us to override this value and statically define it. | Optional
        vni_override: <int, options: [< 1-16777215 >]>

        # Vlan name | Required
        name: <str>

        # Tags leveraged for networks services filtering. | Required
        tags: 
          - <str>

        # Extend this L2VLAN over VXLAN | Optional
        vxlan: <bool, default: True>

        # Enable IGMP Snooping ("on" by default in EOS) | Optional
        igmp_snooping_enabled: <bool>
```

<details>
<summary>Schema</summary>

```yaml
tenants:
  description: List of tenants, to define network services; L3 VRFs and L2 VLANS.
  elements: dict
  options:
    enable_mlag_ibgp_peering_vrfs:
      default: true
      description: 'MLAG IBGP peering per VRF

        By default an IBGP peering is configured per VRF between MLAG peers on separate
        VLANs.

        Setting enable_mlag_ibgp_peering_vrfs: false under tenant will change this default
        to prevent configuration of these peerings and VLANs for all VRFs in the tenant.

        This setting can be overridden per VRF.'
      type: bool
    l2vlans:
      description: Define L2 network services organized by vlan id.
      elements: dict
      options:
        id:
          choices:
          - < 1-4096 >
          description: VLAN id
          required: true
          type: int
          unique: true
        igmp_snooping_enabled:
          description: Enable IGMP Snooping ("on" by default in EOS)
          type: bool
        name:
          description: Vlan name
          required: true
          type: str
        tags:
          description: Tags leveraged for networks services filtering.
          elements: str
          required: true
          type: str
        vni_override:
          choices:
          - < 1-16777215 >
          description: 'By default the vni will be derived from "mac_vrf_vni_base"

            The vni_override allows us to override this value and statically define
            it.'
          type: int
        vxlan:
          default: true
          description: Extend this L2VLAN over VXLAN
          type: bool
      type: list
    mac_vrf_vni_base:
      choices:
      - < 10000-16770000 >
      description: 'VXLAN Network Identifier for MAC VRF

        VXLAN VNI is derived from the base number with simple addition.

        e.g. mac_vrf_vni_base = 10000, svi 100 = VNI 10100, svi 300 = VNI 10300.'
      required: true
      type: int
    name:
      description: 'Specify a tenant name.

        Tenant provide a construct to group L3 VRFs and L2 VLANs.

        Networks services can be filtered by tenant name.'
      required: true
      type: str
      unique: true
    vlan_aware_bundle_number_base:
      default: 0
      description: 'Base number for vlan_aware_bundle

        The "Assigned Number" part of RD/RT is derived from vrf_vni + vlan_aware_bundle_number_base.'
      type: int
    vrfs:
      description: Define L3 network services organized by vrf.
      elements: dict
      options:
        additional_route_targets:
          description: Optional configuration of extra route-targets for this VRF. Useful
            for route-leaking or gateway between address families.
          elements: dict
          options:
            address_family:
              type: str
            nodes:
              description: Nodes key is optional. Default is all nodes where the VRF
                is defined.
              elements: str
              type: str
            route_target:
              type: str
            type:
              choices:
              - import
              - export
              type: str
          type: list
        bgp:
          options:
            raw_eos_cli:
              description: EOS CLI rendered directly on the Router BGP, VRF definition
                in the final EOS configuration Supports Multiline strings
              type: str
            structured_config:
              description: Custom structured config added under router_bgp.vrfs.<vrf>
                for eos_cli_config_gen
              type: any
          type: dict
        bgp_peers:
          description: This will configure BGP neighbors inside the tenant VRF for peering
            with external devices. The configured peer will automatically be activated
            for ipv4 or ipv6 address family based on the ip address. Note, only ipv4
            and ipv6 address families are currently supported in eos_designs. For other
            address families, use custom_structured configuration with eos_cli_config_gen.
          elements: dict
          options:
            default_originate:
              options:
                always:
                  type: bool
              type: dict
            description:
              type: str
            ebgp_multihop:
              choices:
              - < 1-255 >
              type: int
            ip_address:
              choices:
              - < IPv4_address >
              - < IPv6_address >
              required: true
              type: str
              unique: true
            local_as:
              type: int
            maximum_routes:
              type: int
            next_hop_self:
              type: bool
            nodes:
              description: Nodes are required to restrict configuration of BGP neighbors
                to certain nodes in the network.
              elements: str
              type: str
            password:
              description: Encrypted password
              type: str
            remote_as:
              description: Remote ASN
              type: int
            route_map_in:
              description: Route-map name
              type: st
            route_map_out:
              description: Route-map name
              type: str
            send_community:
              choices:
              - standard
              - extended
              - large
              - all
              type: str
            set_ipv4_next_hop:
              choices:
              - < IPv4_address >
              description: Next hop settings can be either ipv4 or ipv6 for one neighbor,
                this will be applied by a uniquely generated route-map per neighbor.
                Next hop takes precedence over route_map_out.
              type: str
            set_ipv6_next_hop:
              choices:
              - < IPv6_address >
              description: Next hop settings can be either ipv4 or ipv6 for one neighbor,
                this will be applied by a uniquely generated route-map per neighbor.
                Next hop takes precedence over route_map_out.
              type: str
            update_source:
              description: Interface name
              type: str
            weight:
              choices:
              - < 0-65535>
              type: int
          type: list
        enable_mlag_ibgp_peering_vrfs:
          default: true
          description: 'MLAG IBGP peering per VRF

            By default an IBGP peering is configured per VRF between MLAG peers on separate
            VLANs.

            Setting enable_mlag_ibgp_peering_vrfs: false under vrf will change this
            default and/or override the tenant-wide setting'
          type: bool
        ip_helpers:
          description: IP Helpers for DHCP relay
          elements: dict
          options:
            name:
              description: DNS Name or IP address
              required: true
              type: str
              unique: true
            source_interface:
              description: Source interface name
              type: str
            source_vrf:
              description: VRF to originate DHCP relay packets to DHCP server
              type: str
          type: list
        l3_interfaces:
          description: 'List of L3 interfaces

            This will create IP routed interface inside VRF. Length of interfaces, nodes
            and ip_addresses must match.'
          elements: dict
          options:
            description:
              description: Interface Description
              type: str
            enabled:
              default: false
              description: Enable or disable L3 interface
              type: bool
            encapsulation_dot1q_vlan:
              description: For sub-interfaces the dot1q vlan is derived from the interface
                name by default, but can also be specified.
              elements: int
              type: int
            interfaces:
              description: Interface Name
              elements: str
              type: str
            ip_addresses:
              choices:
              - <IPv4_address/Mask>
              elements: str
              type: str
            mtu:
              description: Interface MTU
              type: int
            nodes:
              description: Hostname of Node
              elements: str
              type: str
            raw_eos_cli:
              description: EOS CLI rendered directly on the Ethernet interface in the
                final EOS configuration Supports Multiline strings
              type: str
            structured_config:
              description: Custom structured config added under ethernet_interfaces.<interface>
                for eos_cli_config_gen
              type: any
          type: list
        mlag_ibgp_peering_vlan:
          choices:
          - < 1-4094 >
          description: 'Manually define the VLAN used on the MLAG pair for the iBGP
            session.

            By default this parameter is calculated using the following formula: <base_vlan>
            + <vrf_vni> - 1'
          type: int
        name:
          description: VRF name
          required: true
          type: str
          unique: true
        raw_eos_cli:
          description: EOS CLI rendered directly on the root level of the final EOS
            configuration Supports Multiline strings
          type: str
        redistribute_static:
          description: Non-selectively enabling or disabling redistribute static inside
            the VRF
          type: bool
        static_routes:
          description: 'This will create static routes inside the tenant VRF.

            If nodes are not specified, all l3leafs that carry the VRF will also be
            applied the static routes.

            If a node has a static route in the VRF, redistribute static will be automatically
            enabled in that VRF.

            This automatic behavior can be overridden non-selectively with the redistribute_static
            knob for the VRF.'
          elements: dict
          options:
            destination_address_prefix:
              choices:
              - < IPv4_address/Mask >
              required: true
              type: str
            distance:
              choices:
              - < 1-255 >
              type: int
            gateway:
              choices:
              - < IPv4_address >
              type: str
            interface:
              type: str
            metric:
              choices:
              - < 0-4294967295 >
              type: int
            name:
              description: Name/description for static route
              type: str
            nodes:
              description: Hostname of Node
              elements: str
              type: str
            tag:
              choices:
              - < 0-4294967295 >
              type: int
          type: list
        structured_config:
          description: Custom structured config for eos_cli_config_gen
          type: any
        svis:
          description: 'Dictionary of SVIs

            This will create both the L3 SVI and L2 VLAN based on filters applied to
            l3leaf and l2leaf.'
          elements: dict
          options:
            description:
              default: < vlan_name >
              type: str
            enabled:
              default: false
              description: Enable or disable SVI interface
              type: bool
            id:
              choices:
              - < 1-4096 >
              description: SVI interface id and VLAN id
              required: true
              type: int
              unique: true
            igmp_snooping_enabled:
              description: Enable IGMP Snooping ("on" by default in EOS)
              type: bool
            ip_address_virtual:
              choices:
              - < IPv4_address/Mask >
              description: 'ip address virtual to configure VXLAN Anycast IP address

                Conserves IP addresses in VXLAN deployments as it doesn''t require unique
                IP addresses on each node.'
              type: str
            ip_address_virtual_secondaries:
              choices:
              - < IPv4_address/Mask >
              elements: string
              type: string
            ip_helpers:
              description: IP Helpers for DHCP relay
              elements: dict
              options:
                name:
                  description: DNS Name or IP address
                  required: true
                  type: str
                  unique: true
                source_interface:
                  description: Source interface name
                  type: str
                source_vrf:
                  description: VRF to originate DHCP relay packets to DHCP server.
                  type: str
              type: list
            ip_virtual_router_addresses:
              choices:
              - < IPv4_address >
              - < IPv4_address/Mask >
              description: 'ip virtual-router address

                note, also requires an IP address to be configured on the SVI where
                it is applied.'
              elements: string
              type: string
            mtu:
              description: Interface MTU
              type: int
            name:
              description: Vlan name
              required: true
              type: str
            nodes:
              description: Define node specific configuration, such as unique IP addresses.
              elements: dict
              options:
                ip_address:
                  choices: < IPv4_address/Mask >
                  description: device unique IP address for node.
                  type: str
                name:
                  description: Hostname of node
                  required: true
                  type: str
                raw_eos_cli:
                  description: EOS CLI rendered directly on the VLAN interface in the
                    final EOS configuration Overrides the setting on SVI level. Supports
                    Multiline strings
                  type: str
                structured_config:
                  description: 'Custom structured config added under vlan_interfaces.<interface>
                    for eos_cli_config_gen

                    Overrides the setting on SVI level.'
                  type: any
              type: list
            profile:
              description: Name of SVI profile to apply If variables are configured
                in profile AND SVI, SVI information will overwrite profile.
              type: str
            raw_eos_cli:
              description: EOS CLI rendered directly on the VLAN interface in the final
                EOS configuration Supports Multiline strings
              type: str
            structured_config:
              description: Custom structured config added under vlan_interfaces.<interface>
                for eos_cli_config_gen
              type: any
            tags:
              description: Tags leveraged for networks services filtering.
              elements: str
              required: true
              type: str
            vni_override:
              choices:
              - < 1-16777215 >
              description: 'By default the vni will be derived from "mac_vrf_vni_base"

                The vni_override allows us to override this value and statically define
                it.'
              type: int
            vxlan:
              default: true
              description: Extend this SVI over VXLAN
              type: bool
          required: true
          type: list
        vrf_vni:
          default: < vrf_id >
          description: 'VRF VNI - required if vrf_id is not set

            The VRF VNI range is not limited, but it is recommended to keep vrf_vni
            <= 1024

            It is necessary to keep [ vrf_vni + MLAG IBGP base_vlan ] < 4094 to support
            MLAG IBGP peering in VRF.

            If vrf_vni > 1094 make sure to change mlag_ibgp_peering_vrfs: { base_vlan:
            < > } to a lower value (default 3000).

            If vrf_vni > 10000 make sure to adjust mac_vrf_vni_base accordingly to avoid
            overlap.'
          type: int
        vtep_diagnostic:
          description: 'Enable VTEP Network diagnostics

            This will create a loopback with virtual source-nat enable to perform diagnostics
            from the switch.'
          options:
            loopback:
              choices:
              - < 2-2100 >
              description: Loopback interface number
              required: true
              type: int
            loopback_ip_pools:
              description: 'For inventories with multiple PODs a loopback range can
                be set per POD to avoid overlaps.

                This only takes effect when loopback_ip_range is not defined. Loopback
                is not created unless either loopback_ip_range or loopback_ip_pools
                are set.'
              elements: dict
              options:
                ipv4_pool:
                  choices:
                  - < IPv4_address/Mask >
                  required: true
                  type: str
                pod:
                  description: Name matching "pod_name"
                  required: true
                  type: str
              type: list
            loopback_ip_range:
              choices:
              - < IPv4_address/Mask >
              description: 'Loopback ip range, a unique ip is derived from this ranged
                and assigned

                to each l3 leaf based on it''s unique id. Loopback is not created unless
                either loopback_ip_range or loopback_ip_pools are set.'
              type: str
          type: dict
      type: list
  type: list
```

</details>
