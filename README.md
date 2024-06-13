# Project-Nautobot-Ansible
1. Project-Nautobot-Ansible is aiming to integrate the IP device management with sw compliance enforcement based on open source tool nautobot and ansible.
2. This nautobot official git hub proejct is avaialabe at https://github.com/nautobot/nautobot-lab. This project is using Virtualbox to bring up the VM with Ubuntu as shown in the Vagrantfile.

Once docker is installed and follow the installation CLI to bring up the nautobot in docker container:
'docker run -itd --name nautobot -p 8000:8000 networktocode/nautobot-lab'

use 'docker ps' to examine if the status is in 'healthy' state. The application boot up time is about 30 seconds to reach healthy state for operation. Troubleshooting with general docker container might be needed for your environment which will need at least 2 cpu, 2G storage and 4G of RAM. 


Use 'docker exec -it nautobot nautobot-server createsuperuser' to setup user credentials

3. The Ansible part is based on control server to access web01, web02 and db nodes as shown in the inventory file.
4. The screenshots are collected under images subdirectory for reference.
5. Here is the Python API verification sample for using Juypter Notebook with pynautobot package.
from pynautobot.core.api import Api as api
url="http://192.168.10.2:8000"
token="be3bec4cc2ea15b0a04f939e0d5f076047724937"
nautobot = api(url=url, token=token)
nautobot.version
devices = nautobot.dcim.devices
print(devices.all())
Output:
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

