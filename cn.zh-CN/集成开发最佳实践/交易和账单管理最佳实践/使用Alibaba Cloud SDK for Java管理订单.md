# 使用Alibaba Cloud SDK for Java管理订单 {#concept_222272 .concept}

阿里云用户购买产品或者开通阿里云业务后，在阿里云平台会生成订单。您可以通过使用Alibaba Cloud SDK for Java调用相关接口查询用户订单列表及详细信息和取消未支付订单。

## 使用场景 {#section_w0u_mpq_lei .section}

阿里云用户在购买或开通产品后会在阿里云生成订单记录， 订单是瞬时行为，也就是一次业务行为就会产生一笔订单。如下图所示，构成订单的主要因素包括订单基本信息、产品信息、支付信息和业务动作这四部分。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/189693/155756196346979_zh-CN.png)

您可以通过阿里云开放的费用中心接口，实现以下需求：

-   通过查询订单了解自己下单/开通某产品和时间信息。
-   通过查询订单了解自己下单/开通某产品费用及支付信息。
-   通过查询订单详情了解自己下单/开通某产品对应详细信息，比如：产品配置信息，数量和实例ID。
-   通过订单的operator参数查看/审计订单开通RAM账号及对应信息。
-   如果存在已下单但尚未支付的订单，可以通过取消订单接口释放订单。

## 产品架构 {#section_u22_gea_01s .section}

一个自然人或组织在阿里云可以申请多个阿里云账号，每个阿里云账号可以根据需要创建多个订单，但每一个订单都会对应到一个具体的云产品，具体架构如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/189693/155756196346036_zh-CN.png)

## 相关API {#section_i91_tth_ca5 .section}

您可以通过使用Alibaba Cloud SDK for Java分别调用以下API，完成订单管理操作。

|API名称|说明|
|-----|--|
|[QueryProductList](https://help.aliyun.com/document_detail/95984.html?spm=a2c4g.11186623.6.553.54d26b94Fg6uAh)|查询阿里云产品：包含产品名称以及对应信息。|
|[QueryOrders](https://help.aliyun.com/document_detail/89079.html?spm=a2c4g.11174283.6.609.2b515467TptaeA)|查询用户所有订单的列表：包含订单号、产品和创建时间等。|
|[GetOrderDetail](https://help.aliyun.com/document_detail/89084.html?spm=a2c4g.11186623.6.610.7ec3254cePhErA)|查询用户某个订单详情信息：包含订单号、产品类型和创建时间等。|
|[CancelOrder](https://help.aliyun.com/document_detail/113020.html?spm=a2c4g.11186623.6.611.45d555a6iZ9FxO)|取消用户未支付订单。|

## 前提条件 {#section_oki_oml_zab .section}

在调用API查询订单数据之前，您需要获取访问密钥（AccessKey）。请登录[AccessKey](https://usercenter.console.aliyun.com/#/manage/ak)管理控制台，创建AccessKey，或者联系系统管理员获取授权账号。

## 典型案例 {#section_254_67o_rto .section}

-   查询用户订单列表：

    以下示例代码展示了通过调用QueryOrders接口查询订单，获取产品下单/开通的时间信息。

    ``` {#codeblock_ojq_cdw_cov}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import com.aliyuncs.bssopenapi.model.v20171214.*;
    
    public class BssOpenApiDemo {
    
         public static void main(String[] args) {
            try {
              // 给BssOpenApi产品添加默认region路由信息
              DefaultProfile.addEndpoint("cn-hangzhou", "cn-hangzhou", "BssOpenApi", "business.aliyuncs.com");
              // 中国站产品的调用，统一调用cn-hangzhou地域
              DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "", "");
              IAcsClient client = new DefaultAcsClient(profile);
              QueryOrdersRequest request = new QueryOrdersRequest();
              QueryOrdersResponse response = client.getAcsResponse(request);
              System.out.println(new Gson().toJson(response));
            } catch (ServerException e) {
                 e.printStackTrace();
            } catch (ClientException e) {
                 System.out.println("ErrCode:" + e.getErrCode());
                 System.out.println("ErrMsg:" + e.getErrMsg());
                 System.out.println("RequestId:" + e.getRequestId());
            }
       }
    }
    ```

-   查询用户某个订单详情信息：

    以下示例代码展示了通过调用GetOrderDetail接口查询订单，获取某产品下单/开通的费用及支付信息。也可以通过查询订单详情，获取已下单/开通某产品对应的详细信息，比如：产品配置信息，数量和实例ID。

    ``` {#codeblock_hj3_x5v_hcr}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import com.aliyuncs.bssopenapi.model.v20171214.*;
    
    public class BssOpenApiDemo {
    
        public static void main(String[] args) {
           try {
               // 给BssOpenApi产品添加默认region路由信息
               DefaultProfile.addEndpoint("cn-hangzhou", "cn-hangzhou", "BssOpenApi", "business.aliyuncs.com");
               // 中国站产品的调用，统一调用cn-hangzhou地域
               DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "", "");
               IAcsClient client = new DefaultAcsClient(profile);
               GetOrderDetailRequest request = new GetOrderDetailRequest();
               GetOrderDetailResponse response = client.getAcsResponse(request);
               System.out.println(new Gson().toJson(response));
               }catch (ServerException e){
                   e.printStackTrace();
               }catch (ClientException e) {
                   System.out.println("ErrCode:" + e.getErrCode());
                   System.out.println("ErrMsg:" + e.getErrMsg());
                   System.out.println("RequestId:" + e.getRequestId());
               }
        }
    }                    
    ```

-   取消未支付订单：

    以下示例代码展示了通过调用CancelOrder接口释放未支付订单。

    ``` {#codeblock_aew_qv7_z70}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import com.aliyuncs.bssopenapi.model.v20171214.*;
    
    public class BssOpenApiDemo {
    
        public static void main(String[] args) {
    
            try {
                // 给BssOpenApi产品添加默认region路由信息
                DefaultProfile.addEndpoint("cn-hangzhou", "cn-hangzhou", "BssOpenApi", "business.aliyuncs.com");
                // 中国站产品的调用，统一调用cn-hangzhou地域
                DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "", "");
                IAcsClient client = new DefaultAcsClient(profile);
                CancelOrderRequest request = new CancelOrderRequest();
                CancelOrderResponse response = client.getAcsResponse(request);
                System.out.println(new Gson().toJson(response));
                }catch (ServerException e){
                    e.printStackTrace();
                }catch (ClientException e){
                    System.out.println("ErrCode:" + e.getErrCode());
                    System.out.println("ErrMsg:" + e.getErrMsg());
                    System.out.println("RequestId:" + e.getRequestId());
                }
        }
    }
    ```


