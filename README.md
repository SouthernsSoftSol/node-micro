# node-micro
Simple Node MicroService

Creating brach using checkout command
```git checkout -b practice_new```

Then commiting it
```git commit -am"Updating Readme file"```

Push it to repo
```git push```

if you have created new branch then you need to use below command

```git push --set-upstream origin practice_new```


import boto3
import datetime
from datetime import date


client = boto3.client('ec2',region_name = 'eu-west-2')
response = client.describe_volumes(
            Filters=[
                {
                    'Name': 'status',
                    'Values': [
                        'available',
                    ]
                },
            ]
        )
print(response)
for volume in response['Volumes']:
    today = datetime.datetime.now()
    volumeCreateDate=volume['CreateTime']
    volumeCreateDateWithoutZone = volumeCreateDate.replace(tzinfo=None)
    delta = today-volumeCreateDateWithoutZone
    print(delta.days)
    if delta.days > 30:
        print("deleting volume {} ".format(volume))
        volId=volume['VolumeId']

        try:
            client.delete_volume(
                VolumeId = volId
            )
        except Exception as e:
            print(e._doc_)
            print(e.message)
