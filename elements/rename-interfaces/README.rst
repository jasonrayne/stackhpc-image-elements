================
rename-interfaces
================

This element creates udev rules for network interface renaming based on PCI device
addresses, vendor IDs, and device IDs.

Configuration
=============

The element accepts configuration via the ``DIB_INTERFACE_MAPPINGS`` environment
variable, which should contain a JSON array of interface mapping objects.

Each mapping object should contain:

* ``kernels``: PCI device address (e.g., "0000:85:00.0")
* ``vendor``: Vendor ID in hex format (e.g., "0x15b3")
* ``device``: Device ID in hex format (e.g., "0x101f")  
* ``name``: Desired interface name (e.g., "mlxB0")

Example Configuration
=====================

Set the environment variable before running DIB::

    export DIB_INTERFACE_MAPPINGS='[
      {"kernels": "0000:85:00.0", "vendor": "0x15b3", "device": "0x101f", "name": "mlxB0"},
      {"kernels": "0000:85:00.1", "vendor": "0x15b3", "device": "0x101f", "name": "mlxB1"},
      {"kernels": "0000:81:00.0", "vendor": "0x15b3", "device": "0x101f", "name": "mlxA0"},
      {"kernels": "0000:81:00.1", "vendor": "0x15b3", "device": "0x101f", "name": "mlxA1"}
    ]'

From Ansible Playbook
======================

When using this element with Kayobe, set the variable in your playbook::

    - name: Build overcloud image
      command: disk-image-create ...
      environment:
        DIB_INTERFACE_MAPPINGS: |
          [
            {"kernels": "0000:85:00.0", "vendor": "0x15b3", "device": "0x101f", "name": "mlxB0"},
            {"kernels": "0000:85:00.1", "vendor": "0x15b3", "device": "0x101f", "name": "mlxB1"}
          ]

Default Behavior
================

If ``DIB_INTERFACE_MAPPINGS`` is not set, the element will generate placeholder
udev rules with example values that should be customized for your environment.
