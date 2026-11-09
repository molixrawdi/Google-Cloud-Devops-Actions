## Securing instances:</p>
The major point that has evolved in the last couple of years was how to secure the instances from being exposed and hacked. </p>
Google Identity Aware Proxy was one of the solutions. Previously we had the bastion, the latter worked for some time, now the matters,</p>
are different.

Instances come with external ephemeral IP address attached, one needs to remove that after applying google private connection.</p>

```
# Delete the external access configuration from the primary network interface
gcloud compute instances delete-access-config INSTANCE_NAME \
    --zone=YOUR_ZONE \
    --access-config-name="External NAT"
```
