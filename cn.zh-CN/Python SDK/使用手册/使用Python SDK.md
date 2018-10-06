# 使用Python SDK {#concept_mkk_vpj_zdb .concept}

本文档介绍如何安装及调用阿里云Python SDK。

## 安装 {#section_kjb_1qj_zdb .section}

阿里云Python SDK支持 Python 2.6.x, 2.7.x 和 3.x及以上环境，并提供pip和GitHub两种安装方式。

-   使用pip安装\(推荐\)

    执行以下命令，通过pip安装SDK。

    ```bash
    pip install aliyun-python-sdk-core # 安装阿里云SDK核心库
    pip install aliyun-python-sdk-ecs # 安装管理ECS的库
    pip install aliyun-python-sdk-rds # 安装管理RDS的库
    ```

    **说明：** 如果您使用的是 python3.x，请将`pip install aliyun-python-sdk-core`修改为`pip install aliyun-python-sdk-core-v3`。

-   下载GithHub源码

    无执行以下命令，通过GitHub安装Python SDK。

    ```bash
    git clone https://github.com/aliyun/aliyun-openapi-python-sdk.git
    # 安装阿里云 SDK 核心库
    cd aliyun-python-sdk-core
    python setup.py install
    # 安装阿里云 ECS SDK
    cd aliyun-python-sdk-ecs
    python setup.py install
    ```

## 设置身份验证凭据 {#section_vjb_1qj_zdb .section}

当使用阿里云SDK访问阿里云服务时，您需要提供阿里云账号进行身份验证。

目前，Python SDK支持以下几种身份验证方式：

|验证方式|说明|
|:---|:-|
|AccessKey|使用AccessKey ID和AccessKey Secret访问|
|StsToken|使用STS Token访问|
|RamRoleArn|使用RAM子账号的AssumeRole方式访问|
|EcsRamRole|在ECS实例上通过EcsRamRole实现免密验证|
|RsaKeyPair|使用RSA公私钥方式（仅日本站支持）|

本文以AccessKey为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用RAM账号来访问阿里云服务。阿里云账号的AccessKey对拥有的资源有完全的权限。RAM账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。参考[创建AccessKey](https://help.aliyun.com/document_detail/66453.html)创建RAM账号的AccessKey。

使用AccessKey作为访问凭据，需要在初始化Client时设置。

**说明：** 确保包含AccessKey的代码不会泄漏（例如提交到外部公开的GitHub项目），否则将会危害您的阿里云账号的信息安全。

```py
client = AcsClient(
   "<access-key-id>",
   "<access-key-secret>",
   "<region-id>"
);
```

## 发起调用 {#section_bkb_1qj_zdb .section}

本文以ECS为例，介绍如何使用阿里云Python SDK发起请求。

1.  导入相关产品的SDK。

    ```py
    from aliyunsdkcore.client import AcsClient
    from aliyunsdkcore.acs_exception.exceptions import ClientException
    from aliyunsdkcore.acs_exception.exceptions import ServerException
    from aliyunsdkecs.request.v20140526 import DescribeInstancesRequest
    from aliyunsdkecs.request.v20140526 import StopInstanceRequest
    ```

2.  新建一个AcsClient。

    ```py
    client = AcsClient(
       "<your-access-key-id>",
       "<your-access-key-secret>",
       "<your-region-id>"
    );
    ```

3.  创建Request对象。

    ```py
    request = DescribeInstancesRequest.DescribeInstancesRequest()
    request.set_PageSize(10)
    ```

4.  发起调用并处理返回。

    ```py
    DescribeInstancesResponse response;

    try:
        response = client.do_action_with_exception(request)
        print response
    except ServerException as e:
        print e
    except ClientException as e:
        print e
    ```
