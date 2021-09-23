***With private API-Gateway service, we need create `endpoint` in VPC service, when create `endpoint`, there has one option for "Private DNS" enable/disable, which chould impact for api invoke url***  

[document](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-private-api-test-invoke-url.html)
### Enable the "Private DNS Name"
**Public api-gateway:**  
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://6vto8ihm33.execute-api.eu-west-1.amazonaws.com/public  
    Result: **Forbidden**  
    
  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://6vto8ihm33-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/public  
    Result: **Forbidden**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/public -H 'x-apigw-api-id: 6vto8ihm33'  
    Result: **Forbidden**  

**Private api-gateway: < associate with `endpoint` >**
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example:  https://12ud25gpc4.execute-api.eu-west-1.amazonaws.com/private  
    Result: **Access Pass**  

  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://12ud25gpc4-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/private  
    Result: **Access Pass**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/private -H 'x-apigw-api-id: 12ud25gpc4'
    Result: **Access Pass**  

**Private api-gateway: < disassociate with `endpoint` >**
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://zbjvkwsg01.execute-api.eu-west-1.amazonaws.com/without_endporint_associate  
    Result: **anonymous is not authorized**  

  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://zbjvkwsg01-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/without_endporint_associate  
    Result: **anonymous is not authorized**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/without_endporint_associate -H 'x-apigw-api-id: zbjvkwsg01'  
    Result: **anonymous is not authorized**  
  
### Disable the "Private DNS Name"
**Public api-gateway:**
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://6vto8ihm33.execute-api.eu-west-1.amazonaws.com/public  
    Result: **Access Pass**  

  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://6vto8ihm33-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/public  
    Result: **DNS Not Resolvable**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/public -H 'x-apigw-api-id: 6vto8ihm33'  
    Result: **Forbidden**  

**Private api-gateway: < associate with `endpoint` >**
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://12ud25gpc4.execute-api.eu-west-1.amazonaws.com/private  
    Result: **DNS Not Resolvable**  

  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://12ud25gpc4-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/private  
    Result: **Access Pass**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/private -H 'x-apigw-api-id: 12ud25gpc4'  
    Result: **Access Pass**  

**Private api-gateway: < dis-associate with `endpoint` >**
  Example 01:
    Url_Format: https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://zbjvkwsg01.execute-api.eu-west-1.amazonaws.com/without_endporint_associate  
    Result: **DNS Not Resolvable**  

  Example 02:
    Url_Format: https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}}  
    Url_Example: https://zbjvkwsg01-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/without_endporint_associate  
    Result: **DNS Not Resolvable**  

  Example 03:
    Url_Format: https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}"  
    Url_Example: https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/without_endporint_associate -H 'x-apigw-api-id: zbjvkwsg01'  
    Result: **anonymous is not authorized**  
