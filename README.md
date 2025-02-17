# Project-Nautobot-Ansible
1. Project-Nautobot-Ansible is aiming to integrate the IP device management with sw compliance enforcement based on open source tool nautobot and ansible.
2. This nautobot official git hub proejct is avaialabe at https://github.com/nautobot/nautobot-lab. This project is using Virtualbox to bring up the VM with Ubuntu as shown in the Vagrantfile.

Once docker is installed and follow the installation CLI to bring up the nautobot in docker container:
'docker run -itd --name nautobot -p 8000:8000 networktocode/nautobot-lab'

use 'docker ps' to examine if the status is in 'healthy' state. The application boot up time is about 30 seconds to reach healthy state for operation. Troubleshooting with general docker container might be needed for your environment which will need at least 2 cpu, 2G storage and 4G of RAM. 


Use 'docker exec -it nautobot nautobot-server createsuperuser' to setup user credentials

3. The Ansible part is based on control server to access web01, web02 and db nodes as shown in the inventory file.
4. The screenshots are collected under images subdirectory for reference.
5. The README.GraphQL is used to outline the built-in data query supported in Nautobot package. You could use it the examine the related data schema while working on the the automation scripting. 
6. In this README, we will examine the devices list by both REST api and Python scripting as well. 
Here is the Python API verification sample for using Juypter Notebook with pynautobot package.

*** sample python script
from pynautobot.core.api import Api as api
url="http://192.168.10.2:8000"
token="be3bec4cc2ea15b0a04f939e0d5f076047724937"
nautobot = api(url=url, token=token)
nautobot.version
devices = nautobot.dcim.devices
print(devices.all())
*** python console Output:
[<pynautobot.models.dcim.Devices ('AVMsysSW1') at 0x21e7bc560d0>, <pynautobot.models.dcim.Devices ('cat9400a') at 0x21e7b76fc10>, <pynautobot.models.dcim.Devices ('sjc1sw1') at 0x21e7c6c47d0>, <pynautobot.models.dcim.Devices ('web01') at 0x21e7c6c4e90>, <pynautobot.models.dcim.Devices ('web01') at 0x21e7c6c4690>]


here is the sample of CLI query for first device:AVMsysSW1
root@control:~/nautobot# curl -sH "Authorization: Token be3bec4cc2ea15b0a04f939e0d5f076047724937" -H "Accept: application/json; indent=4" http://192.168.10.2:8000/api/dcim/devices/ | jq .results[0]
{
  "id": "c3415159-020c-485c-a375-bfa76cada1f9",
  "object_type": "dcim.device",
  "display": "AVMsysSW1",
  "url": "http://192.168.10.2:8000/api/dcim/devices/c3415159-020c-485c-a375-bfa76cada1f9/",
  "natural_slug": "avmsyssw1__avmsysloc1_sjc1_c341",
  "face": null,
  "local_config_context_data": null,
  "local_config_context_data_owner_object_id": null,
  "name": "AVMsysSW1",
  "serial": "",
  "asset_tag": null,
  "position": null,
  "device_redundancy_group_priority": null,
  "vc_position": null,
  "vc_priority": null,
  "comments": "",
  "local_config_context_schema": null,
  "local_config_context_data_owner_content_type": null,
  "device_type": {
    "id": "3c30bd48-19e0-4cd5-bb2a-ff6632e8fdfe",
    "object_type": "dcim.devicetype",
    "url": "http://192.168.10.2:8000/api/dcim/device-types/3c30bd48-19e0-4cd5-bb2a-ff6632e8fdfe/"
  },
  "status": {
    "id": "290108e4-cff7-4672-84fa-36bd09541c02",
    "object_type": "extras.status",
    "url": "http://192.168.10.2:8000/api/extras/statuses/290108e4-cff7-4672-84fa-36bd09541c02/"
  },
  "role": {
    "id": "ce041ee5-d017-4683-bc69-ac83e9ebd41b",
    "object_type": "extras.role",
    "url": "http://192.168.10.2:8000/api/extras/roles/ce041ee5-d017-4683-bc69-ac83e9ebd41b/"
  },
  "tenant": null,
  "platform": null,
  "location": {
    "id": "0c56ae92-7378-44c5-8470-d46395af1d93",
    "object_type": "dcim.location",
    "url": "http://192.168.10.2:8000/api/dcim/locations/0c56ae92-7378-44c5-8470-d46395af1d93/"
  },
  "rack": null,
  "primary_ip4": null,
  "primary_ip6": null,
  "cluster": null,
  "virtual_chassis": null,
  "device_redundancy_group": null,
  "secrets_group": null,
  "created": "2024-06-13T02:33:57.985816Z",
  "last_updated": "2024-06-13T02:33:57.985854Z",
  "tags": [],
  "notes_url": "http://192.168.10.2:8000/api/dcim/devices/c3415159-020c-485c-a375-bfa76cada1f9/notes/",
  "custom_fields": {},
  "parent_bay": null
}

from pynautobot.core.api import Api as api
#import pynautobot

nautobot = api(
    url="http://192.168.10.2:8000",
    token="be3bec4cc2ea15b0a04f939e0d5f076047724937"
)
nautobot.status()

{'django-version': '3.2.24',
 'installed-apps': {'constance': '2.9.1',
  'constance.backends.database': None,
  'corsheaders': None,
  'db_file_storage': None,
  'django.contrib.admin': None,
  'django.contrib.auth': None,
  'django.contrib.contenttypes': None,
  'django.contrib.humanize': None,
  'django.contrib.messages': None,
  'django.contrib.sessions': None,
  'django.contrib.staticfiles': None,
  'django_ajax_tables': None,
  'django_celery_beat': '2.5.0..',
  'django_celery_results': '2.4.0..',
  'django_extensions': '3.2.3',
  'django_filters': '23.1',
  'django_jinja': '3.2.24.final.0',
  'django_prometheus': '2.3.1',
  'django_tables2': '2.6.0',
  'drf_spectacular': '0.26.3',
  'drf_spectacular_sidecar': '2024.2.1',
  'graphene_django': '2.16.0',
  'health_check': None,
  'health_check.storage': None,
  'nautobot.circuits': None,
  'nautobot.core': None,
  'nautobot.dcim': None,
  'nautobot.extras': None,
  'nautobot.ipam': None,
  'nautobot.tenancy': None,
  'nautobot.users': None,
  'nautobot.virtualization': None,
  'nautobot_bgp_models': '0.20.1',
  'nautobot_capacity_metrics': '3.0.1',
  'nautobot_circuit_maintenance': '2.0.0',
  'nautobot_data_validation_engine': '3.1.0',
  'nautobot_design_builder': '1.0.0',
  'nautobot_device_lifecycle_mgmt': '2.1.0',
  'nautobot_device_onboarding': '3.0.1',
  'nautobot_firewall_models': '2.0.3',
  'nautobot_floor_plan': '2.0.0',
  'nautobot_golden_config': '2.0.1',
  'nautobot_plugin_nornir': '2.0.0',
  'nautobot_ssot': '2.2.0',
  'rest_framework': '3.14.0',
  'silk': '5.1.0',
  'social_django': '5.2.0',
  'taggit': '4.0.0',
  'timezone_field': '5.1',
  'welcome_wizard': '2.0.0'},
 'nautobot-version': '2.1.4',
 'plugins': {'nautobot_bgp_models': '0.20.1',
  'nautobot_capacity_metrics': '3.0.1',
  'nautobot_circuit_maintenance': '2.0.0',
  'nautobot_data_validation_engine': '3.1.0',
  'nautobot_design_builder': '1.0.0',
  'nautobot_device_lifecycle_mgmt': '2.1.0',
  'nautobot_device_onboarding': '3.0.1',
  'nautobot_firewall_models': '2.0.3',
  'nautobot_floor_plan': '2.0.0',
  'nautobot_golden_config': '2.0.1',
  'nautobot_plugin_nornir': '2.0.0',
  'nautobot_ssot': '2.2.0',
  'welcome_wizard': '2.0.0'},
 'python-version': '3.10.12',
 'celery-workers-running': 1}
