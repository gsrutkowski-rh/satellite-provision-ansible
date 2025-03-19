# Provisoning Hosts via Satellite with Ansible

The following is something I've put together based upon instructions from our [Documentation](https://docs.redhat.com/en/documentation/red_hat_satellite/6.16/html/provisioning_hosts/using_pxe_to_provision_hosts_provisioning#Creating_Hosts_with_UEFI_HTTP_Boot_Provisioning_provisioning) and [here](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite/content/module/host/)

    - name: "create a bare metal host to be provisioned by Grub2 UEFI HTTP"
      redhat.satellite.host
        username: someuser
        password: "{{ password_variable }}"
        server_url: "https://satellite.example.com"
        name: "new_host"
        hostgroup: "my_hostgroup"
        location: "my_location"
        organization: "my_organization"
        pxe-loader: "Grub2 UEFI HTTP"
        operatingsystem: "Red Hat Laptop"
        managed: true
        build: true
        state: present
        
the important parts are the **pxe-loader**, **managed**, **build**, and **operatingsystem** lines.  the **managed** line explicitly states that Satellite is responsible for managing the host.  the **pxe-loader** line along with the **build** line indicate whether or not we're expecting Satellite to provision the host, and how we expect Satellite to provision the host.  The **operatingsystem** determines which provisioning templates will be used as part of this process.  For more information regarding provisioning templates, you can read through Red Hat's [documentation](https://docs.redhat.com/en/documentation/red_hat_satellite/6.16/html/provisioning_hosts/configuring_provisioning_resources_provisioning#creating-operating-systems_provisioning).
During the creation of an "Operating System", provisioning templates are selected based .  Specifically, a Partition Table template (default is "Kickstart default") a configuration template (default is named "Kickstart default PXELinux"), and a provisioning template (default is "Kickstart Default").  All of these templates will get inserted together to create the Kickstart file which will be utilized when provisioning a new Host with the Operating System being created.  The pxe-loader line determines whether to use TFTP or UEFI HTTP(S) to push the kickstart and image the host we're building.  For more information about what and how provisioning templates are used to create a Kickstart file, you can read more bout templates and scriplets as well as how to create or modify the templates for better control: </a href=https://docs.redhat.com/en/documentation/red_hat_satellite/6.16/html/provisioning_hosts/configuring_provisioning_resources_provisioning#provisioning-templates_provisioning>.
