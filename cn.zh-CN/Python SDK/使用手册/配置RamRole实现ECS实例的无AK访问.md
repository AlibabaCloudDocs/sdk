# 配置 RamRole 实现 ECS 实例的无 AK 访问 {#concept_ewz_h3k_zdb .concept}

为了提高应用部署的安全性的同时提升便利性，阿里云 SDK 支持通过[实例元数据](../cn.zh-CN/用户指南/实例/实例自定义/元数据/实例元数据.md#)服务来获取 ECS RAM 角色的授权信息来访问阿里云资源和服务。使用这种方式，您部署在 ECS 上的应用程序，无需在SDK上配置授权信息即可访问阿里云 API（即不需要配置 AccessKey ）。通过这种方式授权的 SDK，可以拥有这个 ECS RAM 角色的权限。

**说明：** 确保 ECS 实例已经配置了 RAM 角色。

## 示例 {#section_th2_n3k_zdb .section}

```python
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.auth.credentials import EcsRamRoleCredential

ecs_ram_role_credential = EcsRamRoleCredential("role_name")
acs_client = AcsClient(region_id='region-id', credential=ecs_ram_role_credential)
```

其中：

- role-name 是与 ECS 实例关联的 RAM 角色名称。

- region-id 是您正在使用的地域的 ID ，你可以通过调用 DescribeRegions 接口查看地域 ID 。

**说明：** 示例中的 region-id 是目标服务\(RAM 角色有访问权限\)的 API 所在地域，不一定等于这个 ECS 实例的地域。
