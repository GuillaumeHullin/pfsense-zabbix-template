# Futur-Tech pfSense Zabbix Template

This is template is just an override of [Rbicelli Zabbix Template](https://github.com/rbicelli/pfsense-zabbix-template) for Futur-Tech purposes

## Specific Changes not exportable

- Disable duplicate triggers concerning updates and upgrades available

### Services Discovery

- Disable `Create enabled` and `Discover` for trigger `Service {#DESCRIPTION} is not running`

- Add the following 2 triggers
```
# Trigger Name:
Service "{#DESCRIPTION}" is running

# Expression:
{Template Futur-Tech App pfSense Active:pfsense.value[service_value,{#SERVICE},status].last()}=1 and {$PFSENSE_SRVC_MONITORING:"{#SERVICE}"}=2 and (({Template Futur-Tech App pfSense Active:pfsense.value[service_value,{#SERVICE},run_on_carp_slave].last()}=1 and {Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=2) or {Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=1 or {Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=0)

# Description:
Service is running

If you want to skip the trigger for this service, remove the macro $PFSENSE_SRVC_MONITORING:"{#SERVICE}"=2
Alternatively you can also set the macro to 1 or 0.

0 = Service monitoring disabled
1 = Service monitoring check if running
2 = Service monitoring check if not running
```

```
# Trigger Name:
Service "{#DESCRIPTION}" is not running

# Expression:
{Template Futur-Tech App pfSense Active:pfsense.value[service_value,{#SERVICE},status].last()}=0 
and {$PFSENSE_SRVC_MONITORING:"{#SERVICE}"}=1 and (
({Template Futur-Tech App pfSense Active:pfsense.value[service_value,{#SERVICE},run_on_carp_slave].last()}=1 and 
{Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=2)
or
( {Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=1)
or
({Template Futur-Tech App pfSense Active:pfsense.value[carp_status].last()}=0)
)

# Description:
Service is not running

If you want to skip the trigger for this service, add the macro $PFSENSE_SRVC_MONITORING:"{#SERVICE}"=0

0 = Service monitoring disabled
1 = Service monitoring check if running
2 = Service monitoring check if not running
```

## Credits
- [Rbicelli Zabbix Template](https://github.com/rbicelli/pfsense-zabbix-template) for Zabbix Template and PHP.
- [Keenton Zabbix Template](https://github.com/keentonsas/zabbix-template-pfsense) for Zabbix Agent freeBSD part.