# 配置STS Token {#concept_ixf_gbk_zdb .concept}

直接使用阿里云账号的主账号的AccessKey ID和AccessKey Secret进行应用开发会有一定的安全风险，为了提升安全性，除了通过RAM角色控制权限范围外，您还可以使用为RAM角色签发的STS Token来访问阿里云服务。

## STS Token介绍 {#section_npt_pbk_zdb .section}

阿里云STS \(Security Token Service\) 是为阿里云账号（或RAM用户）提供短期访问权限管理的云服务。通过STS，您可以为联盟用户（您的本地账号系统所管理的用户）颁发一个自定义时效和访问权限的访问凭证。联盟用户可以使用STS短期访问凭证直接调用阿里云服务API，或登录阿里云管理控制台操作被授权访问的资源。

使用STS Token调用SDK有以下优势：

-   减少了主账号AccessKey ID和AccessKey Secret泄露的风险，特别是移动设备等场景。

-   能使用灵活的权限控制，STS Token有一定的时间限制，并且根据RAM角色的灵活设置对ECS、SLB等资源的精细授权。


**说明：** 在使用STS Token前，确保该产品支持STS Token验证。

## 设置STS Token {#section_dh2_pck_zdb .section}

您可以通过以下两种方式设置STS Token：

-   方式一：直接使用STS Token

    直接使用STS Token时，您需要自行维护STS Token的周期性更新。

    -   不使用自定义配置：

        ```
        // 如果您不需要自定义config，我们提供了一个快捷入口来构建client
        client, err = ecs.NewClientWithStsToken("region-id", "sts-access-key-id", 
                    "sts-access-key-secret", "sts-session-token")
        ```

    -   使用自定义配置：

        ```
        // 如果您需要自定义config，您可以使用下列代码来构建client
        config := sdk.NewConfig().WithEnableAsync(true)
        // 创建credential对象
        credential := credentials.NewEcsRamRoleCredential("role-name")
        // 初始化client
        client, err := ecs.NewClientWithOptions("region-id", config, credential)
        ```

    其中：

    -   region-id是您正在使用的地域ID。你可以通过调用DescribeRegions接口查看地域ID。
    -   sts-access-key-id、 sts-access-key-secret和sts-session-token是调用AssumeRole接口返回的授权信息。
-   方式二：使用SDK自动管理STS Token

    您可以通过指定RAM的角色信息，让SDK帮您自动申请并维护STS Token。

    -   不使用自定义配置：

        ```
        // 如果您不需要自定义config，我们提供了一个快捷入口来构建client
        client, err = ecs.NewClientWithRamRoleArn("region-id", "sts-access-key-id", 
                "sts-access-key-secret", "role-arn", "session-role-name")
        ```

    -   使用自定义配置：

        ```
        // 如果您需要自定义config，您可以使用下列代码来构建client
          config := sdk.NewConfig()
         // 创建credential对象
          credential := credentials.NewRamRoleArnCredential("sts-access-key-id", 
                          "sts-access-key-secret", "role-arn", "role-session-name")
         // 初始化client
         client, err := ecs.NewClientWithOptions("region-id", config, credential)
        ```

    其中：

    -   role-arn是角色全局资源描述符，您可以通过访问[RAM控制台](https://ram.console.aliyun.com/role/list?spm=a2c4g.11186623.2.7.IjY04Z#/role/list)，单击角色名，进入详情页后查询角色名对应的role-arn。

    -   role-session-name是临时角色名称，您可以通过调用AssumeRole接口来获取一个扮演该角色的临时身份，创建成功后，便可以使用创建时的RoleSessionName参数作为本方式的role-session-name参数。


