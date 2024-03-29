# Wcf Self Hosting

## Create service

'Service' is a Wcf Service Application containing the service information and the Web.config file.

```csharp
var service = new ServiceHost(typeof(Service), new Uri("10.1.1.1:80/Service"));
```

## Setup Service

```csharp
ServiceDebugBehavior sdb = new ServiceDebugBehavior()
{
    IncludeExceptionDetailInFaults = true
};
ServiceMetadataBehavior smb = new ServiceMetadataBehavior
{
    HttpGetEnabled = true
};
smb.MetadataExporter.PolicyVersion = PolicyVersion.Policy15;
// Check if service already has ServiceMetadataBehavior set and manually modify it if so
if (service.Description.Behaviors.Any(b => b.GetType() == typeof(ServiceMetadataBehavior)))
{
    service.Description.Behaviors.Find<ServiceMetadataBehavior>().HttpGetEnabled = true;
    service.Description.Behaviors.Find<ServiceMetadataBehavior>().MetadataExporter.PolicyVersion = PolicyVersion.Policy15;
}
// Otherwise, add it
else
{
    service.Description.Behaviors.Add(smb);
}
// Check if service already has ServiceDebugBehavior set and manually modify it if so
if (service.Description.Behaviors.Any(b => b.GetType() == typeof(ServiceDebugBehavior)))
{
    service.Description.Behaviors.Find<ServiceDebugBehavior>().IncludeExceptionDetailInFaults = true;
}
// Otherwise, add it
else
{
    service.Description.Behaviors.Add(sdb);
}
// Timeouts
service.OpenTimeout = new TimeSpan(0, 0, 5));
service.CloseTimeout = new TimeSpan(0, 0, 5));
```

## Open Service

```csharp
service.Open());
```

## Close Service

```csharp
service.Close());
```
