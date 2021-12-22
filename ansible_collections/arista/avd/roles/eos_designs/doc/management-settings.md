# Management Settings

## Variables and Options

### local_users

List of local users

```yaml
# List of local users | Required
local_users: 
    # Username | Required
  - name: <str (unique)>

    # (1-15) Initial privilege level with local EXEC authorization | Optional
    privilege: <int>

    # Specify a role for the user | Optional
    role: <str>

    # do not configure a password for given username. sha512_password MUST not be defined for this user. | Optional
    no_password: <bool>

    # SHA512 ENCRYPTED password | Optional
    sha512_password: <str>
```

<details>
<summary>Example</summary>

```yaml
local_users:
  - name: admin
    no_password: true
    privilege: 15
    role: network-admin
```

</details>

<details>
<summary>Schema</summary>

```yaml
local_users:
  description: List of local users
  elements: dict
  example:
  - name: admin
    no_password: true
    privilege: 15
    role: network-admin
  options:
    name:
      description: Username
      required: true
      type: str
      unique: true
    no_password:
      description: do not configure a password for given username. sha512_password MUST
        not be defined for this user.
      type: bool
    privilege:
      description: (1-15) Initial privilege level with local EXEC authorization
      type: int
    role:
      description: Specify a role for the user
      type: str
    sha512_password:
      description: SHA512 ENCRYPTED password
      type: str
  required: true
  type: list
```

</details>

### mgmt_vrf_routing

Enable IP Routing on OOB Management VRF

```yaml
# Enable IP Routing on OOB Management VRF | Optional
mgmt_vrf_routing: <bool, default: False>
```

<details>
<summary>Schema</summary>

```yaml
mgmt_vrf_routing:
  default: false
  description: Enable IP Routing on OOB Management VRF
  type: bool
```

</details>

### mgmt_interface

OOB Management Interface

```yaml
# OOB Management Interface | Optional
mgmt_interface: <str, default: Management1>
```

<details>
<summary>Schema</summary>

```yaml
mgmt_interface:
  default: Management1
  description: OOB Management Interface
  type: str
```

</details>

### mgmt_interface_vrf

OOB Management VRF

```yaml
# OOB Management VRF | Optional
mgmt_interface_vrf: <str, default: MGMT>
```

<details>
<summary>Schema</summary>

```yaml
mgmt_interface_vrf:
  default: MGMT
  description: OOB Management VRF
  type: str
```

</details>

### mgmt_gateway

OOB Management Default Gateway, IPv4 address

```yaml
# OOB Management Default Gateway, IPv4 address | Required
mgmt_gateway: <str>
```

<details>
<summary>Schema</summary>

```yaml
mgmt_gateway:
  description: OOB Management Default Gateway, IPv4 address
  required: true
  type: str
```

</details>

### mgmt_destination_networks

OOB mgmt interface destination networks - override default route. IPv4_network/Mask

```yaml
# OOB mgmt interface destination networks - override default route. IPv4_network/Mask | Optional
mgmt_destination_networks: 
  - <str>
```

<details>
<summary>Schema</summary>

```yaml
mgmt_destination_networks:
  description: OOB mgmt interface destination networks - override default route. IPv4_network/Mask
  elements: str
  type: str
```

</details>

### management_eapi

Management eAPI. Default is https management eAPI enabled. The vrf is set to < mgmt_interface_vrf >

```yaml
# Management eAPI. Default is https management eAPI enabled. The vrf is set to < mgmt_interface_vrf > | Optional
management_eapi: 
  # enable_http | Optional
  enable_http: <bool, default: False>

  # enable_https | Optional
  enable_https: <bool, default: True>
```

<details>
<summary>Schema</summary>

```yaml
management_eapi:
  description: Management eAPI. Default is https management eAPI enabled. The vrf is
    set to < mgmt_interface_vrf >
  options:
    enable_http:
      default: false
      type: bool
    enable_https:
      default: true
      type: bool
  type: dict
```

</details>

### cvp_instance_ip

IPv4 address

```yaml
# IPv4 address | Optional
cvp_instance_ip: <str>
```

<details>
<summary>Schema</summary>

```yaml
cvp_instance_ip:
  description: IPv4 address
  type: str
```

</details>

### cvp_instance_ips

List of IPv4 Addresses or CV as a Service hostname

```yaml
# List of IPv4 Addresses or CV as a Service hostname | Optional
cvp_instance_ips: 
  - <str>
```

<details>
<summary>Schema</summary>

```yaml
cvp_instance_ips:
  description: List of IPv4 Addresses or CV as a Service hostname
  elements: str
  type: str
```

</details>

### cvp_ingestauth_key

CloudVision Ingest Authentication key

```yaml
# CloudVision Ingest Authentication key | Optional
cvp_ingestauth_key: <str>
```

<details>
<summary>Schema</summary>

```yaml
cvp_ingestauth_key:
  description: CloudVision Ingest Authentication key
  type: str
```

</details>

### terminattr_ingestgrpcurl_port

CloudVision Telemetry Port Number

```yaml
# CloudVision Telemetry Port Number | Optional
terminattr_ingestgrpcurl_port: <int, default: 9910>
```

<details>
<summary>Schema</summary>

```yaml
terminattr_ingestgrpcurl_port:
  default: 9910
  description: CloudVision Telemetry Port Number
  type: int
```

</details>

### terminattr_smashexcludes

smash excludes

```yaml
# smash excludes | Optional
terminattr_smashexcludes: <str, default: ale,flexCounter,hardware,kni,pulse,strata>
```

<details>
<summary>Schema</summary>

```yaml
terminattr_smashexcludes:
  default: ale,flexCounter,hardware,kni,pulse,strata
  description: smash excludes
  type: str
```

</details>

### terminattr_ingestexclude

ingest excludes

```yaml
# ingest excludes | Optional
terminattr_ingestexclude: <str, default: /Sysdb/cell/1/agent,/Sysdb/cell/2/agent >>
```

<details>
<summary>Schema</summary>

```yaml
terminattr_ingestexclude:
  default: /Sysdb/cell/1/agent,/Sysdb/cell/2/agent >
  description: ingest excludes
  type: str
```

</details>

### terminattr_disable_aaa

Bypass AAA on switch for commands sent via TerminAttr connection

```yaml
# Bypass AAA on switch for commands sent via TerminAttr connection | Optional
terminattr_disable_aaa: <bool, default: False>
```

<details>
<summary>Schema</summary>

```yaml
terminattr_disable_aaa:
  default: false
  description: Bypass AAA on switch for commands sent via TerminAttr connection
  type: bool
```

</details>

### name_servers

List of DNS servers, IPv4 address

```yaml
# List of DNS servers, IPv4 address | Optional
name_servers: 
  - <str>
```

<details>
<summary>Schema</summary>

```yaml
name_servers:
  description: List of DNS servers, IPv4 address
  elements: str
  type: str
```

</details>

### snmp_settings

SNMP Settings

```yaml
# SNMP Settings | Optional
snmp_settings: 
  # contact_info | Optional
  contact: <str>

  # SNMP Location, Formatted as <fabric_name> <dc_name> <pod_name> <switch_rack> <inventory_hostname> | Optional
  location: <bool, default: False>
```

<details>
<summary>Schema</summary>

```yaml
snmp_settings:
  description: SNMP Settings
  options:
    contact:
      description: contact_info
      type: str
    location:
      default: false
      description: SNMP Location, Formatted as <fabric_name> <dc_name> <pod_name> <switch_rack>
        <inventory_hostname>
      type: bool
  type: dict
```

</details>

### timezone

Clock timezone

```yaml
# Clock timezone | Optional
timezone: <str>
```

<details>
<summary>Schema</summary>

```yaml
timezone:
  description: Clock timezone
  type: str
```

</details>

### event_handlers

Gives ability to monitor and react to Syslog messages provides a powerful
and flexible tool that can be used to apply self-healing actions,
customize the system behavior, and implement workarounds to problems
discovered in the field.


```yaml
# Gives ability to monitor and react to Syslog messages provides a powerful
# and flexible tool that can be used to apply self-healing actions,
# customize the system behavior, and implement workarounds to problems
# discovered in the field.
#  | Optional
event_handlers: 
    # Name of the event-handler | Required
  - name: <str>

    # Action type ex. bash, increment | Required
    action_type: <str>

    # Command to run when handler is triggered | Required
    action: <str>

    # Delay in sec between 2 triggers | Required
    delay: <int>

    # Trigger, ex. on-logging | Optional
    trigger: <str>

    # String to trigger handler | Optional
    regex: <str>

    # asynchronous | Optional
    asynchronous: <bool>
```

<details>
<summary>Example</summary>

```yaml
event_handlers:
  - action: FastCli -p 15 -c "clear bgp evpn host-flap"
    action_type: bash
    asynchronous: true
    delay: 300
    name: evpn-blacklist-recovery
    regex: EVPN-3-BLACKLISTED_DUPLICATE_MAC
    trigger: on-logging
```

</details>

<details>
<summary>Schema</summary>

```yaml
event_handlers:
  description: 'Gives ability to monitor and react to Syslog messages provides a powerful

    and flexible tool that can be used to apply self-healing actions,

    customize the system behavior, and implement workarounds to problems

    discovered in the field.

    '
  elements: dict
  example:
  - action: FastCli -p 15 -c "clear bgp evpn host-flap"
    action_type: bash
    asynchronous: true
    delay: 300
    name: evpn-blacklist-recovery
    regex: EVPN-3-BLACKLISTED_DUPLICATE_MAC
    trigger: on-logging
  options:
    action:
      description: Command to run when handler is triggered
      required: true
      type: str
    action_type:
      description: Action type ex. bash, increment
      required: true
      type: str
    asynchronous:
      type: bool
    delay:
      description: Delay in sec between 2 triggers
      required: true
      type: int
    name:
      description: Name of the event-handler
      required: true
      type: str
    regex:
      description: String to trigger handler
      type: str
    trigger:
      description: Trigger, ex. on-logging
      type: str
  type: list
```

</details>
