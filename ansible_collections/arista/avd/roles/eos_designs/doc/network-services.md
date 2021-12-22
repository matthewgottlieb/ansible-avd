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
    # mtu | Optional
    mtu: <int>
    # enabled | Optional
    enabled: <bool>
    # ip_virtual_router_addresses | Optional
    ip_virtual_router_addresses: 
      - <string, options: [< IPv4_address > | < IPv4_address/Mask >]>
    # ip_address_virtual | Optional
    ip_address_virtual: <str, options: [< IPv4_address/Mask >]>
    # ip_address_virtual_secondaries | Optional
    ip_address_virtual_secondaries: 
      - <string, options: [< IPv4_address/Mask >]>
    # Enable IGMP Snooping ("on" by default in EOS) | Optional
    igmp_snooping_enabled: <bool>
    # ip_helpers | Optional
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
      type: bool
    igmp_snooping_enabled:
      description: Enable IGMP Snooping ("on" by default in EOS)
      type: bool
    ip_address_virtual:
      choices:
      - < IPv4_address/Mask >
      type: str
    ip_address_virtual_secondaries:
      choices:
      - < IPv4_address/Mask >
      description: null
      elements: string
      type: string
    ip_helpers:
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
      description: null
      elements: string
      type: string
    mtu:
      type: int
    name:
      description: Profile name
      required: true
      type: str
      unique: true
  type: list
```

</details>
