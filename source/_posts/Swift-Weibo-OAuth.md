---
title: Swift 的 新浪微博 OAuth 认证实现
date: 2015-11-29 16:31:00
categories:
- Swift
tags:
- Swift
- OAuth
- 微博
---


## 网络请求

### Alamofire + SwiftyJSON
<!-- more -->
[Alamofire](https://github.com/Alamofire/Alamofire)
[SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON)

**封装网络请求**

```swift
import UIKit
import Alamofire
import SwiftyJSON

class HTTPTool{

    static let shared = HTTPTool()
    
}


extension HTTPTool {
    
    /// 网络请求方法
    ///
    /// - Parameters:
    ///   - url: url
    ///   - method: method
    ///   - params: params
    ///   - success: 请求成功: 返回SwiftyJSON对象(可直接取子对象)
    ///   - errors: errors
    func requestWithParameters(url: String, method: HTTPMethod, parameters params: Parameters, success: @escaping (_ json: JSON)->Void, errors: @escaping ()->Void) {
        
        Alamofire.request(url, method: method, parameters: params).responseJSON { (response) in
            switch response.result {
                
            case .success:
                if let value = response.result.value{
                    let json = JSON(value)
                    
                    guard json["_error"].string == nil else{
                        errors()
                        return
                    }
                    success(json)
                }
                break
                
            case .failure(let error):
                NSLog("error,failure: \(error)")
                errors()
                break
            }
        }
    }
}
```


## OAuth认证

### WebView加载授权网页

#### OAuth2的authorize接口

<img src="/images/14804085876901.jpg" width="728"/>

#### 获取URL

定义 client_id 和 redirect_uri 为常量


```swift
    fileprivate let client_id = ""
    fileprivate let redirect_uri = ""
```

拼接URL

```swift
    var oAuthURL:String {
        return "https://api.weibo.com/oauth2/authorize?client_id=\(client_id)&redirect_uri=\(redirect_uri)&display=mobile&forcelogin=true"
    }
```

#### 加载授权网页


```swift
    webView.loadRequest(request)
```

### 截取code

#### WebView代理

遵守协议并实现 shouldStartLoad 方法

```swift
extension WBOAuthViewController: UIWebViewDelegate{
    
    func webView(_ webView: UIWebView, shouldStartLoadWith request: URLRequest, navigationType: UIWebViewNavigationType) -> Bool {
    
    }
}
```

#### 截取code字段

code是跟随返回地址的参数

<img src="/images/14804086149366.jpg" width="713"/>

实现 shouldStartLoad 方法后可以得到所有网络请求 , 若请求包含字段 error_uri , 则授权失败 , 若请求字段包含 code , 则授权成功 , 得到正确的 code

是否包含字段

```swift
    let urlString = request.url?.query, urlString.hasPrefix("error_uri or code")
```

截取code字段

```swift
    let code = urlString.substring(from: "code=".endIndex)
```

### 处理AccessToken

#### 请求token

5个必选参数
<img src="/images/14804086315231.jpg" width="726"/>

获取code之后立即发起token的网络请求 , 请求成功后保存到模型中 , 由于网络工具类直接返回 SwiftyJSON 对象 , 则可以通过 ObjectMapper ([ObjectMapper](https://github.com/Hearst-DD/ObjectMapper)) 转为模型对象

<img src="/images/14804086422144.jpg" width="712"/>

返回数据为字典,调用 json.dictionaryObject 方法

```swift
    /// 请求Token信息
    ///
    /// - Parameters:
    ///   - code: code description
    ///   - callBack: callBack description
    func requestAccessToken(code: String, callBack: @escaping (WBUserInfo?) -> ()){
        
        let urlString = "https://api.weibo.com/oauth2/access_token"

        let paramenters = [
            "client_id": client_id,
            "client_secret": client_secret,
            "grant_type": "authorization_code",
            "code": code,
            "redirect_uri": redirect_uri
        ]
        
        HTTPTool.shared.requestWithParameters(url: urlString, method: .post, parameters: paramenters, success: { (json) in
            let userInfo = Mapper<WBUserInfo>().map(JSONObject: json.dictionaryObject)
            callBack(userInfo)
        }) { 
            
        }
        
    }
```

#### token本地化

模型转字典

```swift
    var userData = userInfo.toJSON()
```

存储

```swift
    func saveUserDefault(dict:[String: Any]) {
        userDict = dict
        UserDefaults.standard.set(dict, forKey: userInfoKey)
    }
```

读取

```swift
    func loadUserDefault() -> [String: Any]? {
        if let data = UserDefaults.standard.dictionary(forKey: userInfoKey){
            return data
        }
        return nil
    }
```

根据token存储状态判断登录

```swift
    func isLogin() -> Bool {
        if let userDict = loadUserDefault() {
            let isToken = userDict["access_token"] != nil
            return isToken
        }
        return false
    }
```



