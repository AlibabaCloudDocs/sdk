# 使用 Python SDK {#concept_mkk_vpj_zdb .concept}

本文档介绍如何安装及调用阿里云 Python SDK。

## 安装 {#section_kjb_1qj_zdb .section}

阿里云 Python SDK 支持 Python 2.6.x, 2.7.x 和 3.x 及以上环境，并提供 pip 和 GitHub 源码两种安装方式。

- 使用pip安装\(推荐\)

  执行以下命令，通过pip安装SDK。

  ```shell
  pip install aliyun-python-sdk-core # 安装阿里云SDK核心库
  pip install aliyun-python-sdk-ecs # 安装管理ECS的库
  pip install aliyun-python-sdk-rds # 安装管理RDS的库
  ```

  **说明：** 如果您使用的是 python3.x，请将`pip install aliyun-python-sdk-core`修改为`pip install aliyun-python-sdk-core-v3`。

- 下载 GithHub 源码

  执行以下命令，通过 GitHub 上的源码安装 Python SDK 。

  ```shell
  git clone https://github.com/aliyun/aliyun-openapi-python-sdk.git
  # 安装阿里云 SDK 核心库
  cd aliyun-python-sdk-core
  python setup.py install
  # 安装阿里云 ECS SDK
  cd aliyun-python-sdk-ecs
  python setup.py install
  ```


## 设置身份验证凭据 {#section_vjb_1qj_zdb .section}

当使用阿里云 SDK 访问阿里云服务时，您需要提供阿里云账号进行身份验证。

目前，Python SDK 支持以下几种身份验证方式：

|验证方式|说明|
|:---|:-|
|AccessKey | 使用 AccessKey ID 和 AccessKey Secret 访问 |
|StsToken  | 使用 STS Token 访问 |
|RamRoleArn| 使用 RAM 子账号的 AssumeRole 方式访问 |
|EcsRamRole| 在 ECS 实例上通过 EcsRamRole 实现免密验证 |
|RsaKeyPair| 使用 RSA 公私钥方式（仅日本站支持）|

本文以 AccessKey 为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用 RAM 账号来访问阿里云服务。阿里云账号的 AccessKey 对拥有的资源有完全的权限。RAM 账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。参考[创建 AccessKey](https://help.aliyun.com/document_detail/66453.html)创建 RAM 账号的 AccessKey。

使用 AccessKey 作为访问凭据，需要在初始化 Client 时设置。

**说明：** 确保包含 AccessKey 的代码不会泄漏（例如提交到外部公开的 GitHub 项目），否则将会危害您的阿里云账号的信息安全。

```python
client = AcsClient(
   "<access-key-id>", 
   "<access-key-secret>",
   "<region-id>"
);
```

## 发起调用 {#section_bkb_1qj_zdb .section}

本文以 ECS 为例，介绍如何使用阿里云 Python SDK 发起请求。

1.  导入相关产品的 SDK。

  ```python
  from aliyunsdkcore.client import AcsClient
  from aliyunsdkcore.acs_exception.exceptions import ClientException
  from aliyunsdkcore.acs_exception.exceptions import ServerException
  from aliyunsdkecs.request.v20140526 import DescribeInstancesRequest
  from aliyunsdkecs.request.v20140526 import StopInstanceRequest
  ```

2.  新建一个 AcsClient。

  ```python
  client = AcsClient(
     "<your-access-key-id>", 
     "<your-access-key-secret>",
     "<your-region-id>"
  );
  ```

3.  创建 Request 对象。

  ```python
  request = DescribeInstancesRequest.DescribeInstancesRequest()
  request.set_PageSize(10)
  ```

4.  发起调用并处理返回。

  ```python
  DescribeInstancesResponse response;
  try {
      response = client.getAcsResponse(request);
      for (DescribeInstancesResponse.Instance instance:response.getInstances()) {
          System.out.println(instance.getPublicIpAddress());
      }
  } catch (ServerException e) {
      e.printStackTrace();
  } catch (ClientException e) {
      e.printStackTrace();
  }
  ```
