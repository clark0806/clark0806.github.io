***With private API-Gateway service, we need create `endpoint` in VPC service, when create `endpoint`, there has one option for "Private DNS" enable/disable, which chould impact for api invoke url***  

[document](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-private-api-test-invoke-url.html)
### Enable the "Private DNS Name"
- **Public api-gateway:**  
  | Url_Format | Url_Example | Result |  
  | :--- | :--- | :--- |  
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://6vto8ihm33.execute-api.eu-west-1.amazonaws.com/public | Forbidden |  
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://6vto8ihm33-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/public | Forbidden |  
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/public -H 'x-apigw-api-id: 6vto8ihm33'| Forbidden |  

- **Private api-gateway: < associate with `endpoint` >**
  | Url_Format | Url_Example | Result | 
  | :--- | :--- | :--- |
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://12ud25gpc4.execute-api.eu-west-1.amazonaws.com/private | Access Pass |
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://12ud25gpc4-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/private | Access Pass |
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/private -H 'x-apigw-api-id: 12ud25gpc4'| Access Pass |

- **Private api-gateway: < dis-associate with `endpoint` >**
  | Url_Format | Url_Example | Result | 
  | :--- | :--- | :--- |
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://zbjvkwsg01.execute-api.eu-west-1.amazonaws.com/without_endporint_associate | `anonymous is not authorized` |
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://zbjvkwsg01-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/without_endporint_associate | `anonymous is not authorized` |
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/without_endporint_associate -H 'x-apigw-api-id: zbjvkwsg01'| `anonymous is not authorized` |
  
### Disable the "Private DNS Name"
- **Public api-gateway:**
  | Url_Format | Url_Example | Result | 
  | :--- | :--- | :--- |
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://6vto8ihm33.execute-api.eu-west-1.amazonaws.com/public | Access Pass |
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://6vto8ihm33-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/public | DNS Not Resolvable |
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/public -H 'x-apigw-api-id: 6vto8ihm33'| Forbidden |
- **Private api-gateway: < associate with `endpoint` >**
  | Url_Format | Url_Example | Result | 
  | :--- | :--- | :--- |
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://12ud25gpc4.execute-api.eu-west-1.amazonaws.com/private | DNS Not Resolvable |
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://12ud25gpc4-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/private | Access Pass |
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/private -H 'x-apigw-api-id: 12ud25gpc4'| Access Pass |

- **Private api-gateway: < dis-associate with `endpoint` >**
  | Url_Format | Url_Example | Result | 
  | :--- | :--- | :--- |
  | https://{{api-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://zbjvkwsg01.execute-api.eu-west-1.amazonaws.com/without_endporint_associate |  DNS Not Resolvable |
  | https://{{api-id}}-{{endpoint-id}}.execute-api.{{region}}.amazonaws.com/{{stage}} | https://zbjvkwsg01-vpce-016b813476559bb0f.execute-api.eu-west-1.amazonaws.com/without_endporint_associate | DNS Not Resolvable |
  | https://{{endpoint-public-dns-name}}/{{stage}} -H "x-apigw-api-id: {{api-id}}" | https://vpce-016b813476559bb0f-4w2769cp.execute-api.eu-west-1.vpce.amazonaws.com/without_endporint_associate -H 'x-apigw-api-id: zbjvkwsg01'| `anonymous is not authorized` |
* TOC
{:toc}
