---
layout: post
title: "[iOS/Swift] WKWebView"
categories: iOS Swift
---

1) 네트워크 허용
[info.plist에 다음을 추가합니다.](https://garamm.github.io/ios/swift/network-permission/)

2) 코드 작성
```swift
import UIKit
import WebKit

class ViewController: UIViewController, WKUIDelegate, WKNavigationDelegate, WKScriptMessageHandler, UIScrollViewDelegate {
    
    var webView: WKWebView?
    @IBOutlet var containerView: UIView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let contentController = WKUserContentController()
        contentController.add(self,name: "callback")
        
        let config = WKWebViewConfiguration()
        config.userContentController = contentController
        
        webView = WKWebView(frame: containerView.bounds, configuration: config)
        webView?.uiDelegate = self
        webView?.navigationDelegate = self
        
        // 가로, 세로 회전시 뷰가 유동적으로 넓어지도록추가
        webView?.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        
        containerView.addSubview(webView!)
        
        // webview 자체가 스크롤 되지 않도록 다음 코드 추가
        webView!.scrollView.bounces = false
//        webView!.scrollView.isScrollEnabled = false
        
        let webUrl = "접속할 주소 입력"
        guard let url = URL(string: webUrl) else {return}
        let request = URLRequest(url: url)
        HTTPCookieStorage.shared.cookieAcceptPolicy = HTTPCookie.AcceptPolicy.always
        webView?.load(request)
    }
    
    
    func webView(_ webView: WKWebView,
                 didReceive challenge: URLAuthenticationChallenge,
                 completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void)
    {
        if(challenge.protectionSpace.authenticationMethod == NSURLAuthenticationMethodServerTrust) {
            let cred = URLCredential(trust: challenge.protectionSpace.serverTrust!)
            completionHandler(.useCredential, cred)
        } else {
            completionHandler(.performDefaultHandling, nil)
        }
    }
    
    func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
        guard let response = message.body as? String else {
            return
        }
        print(response)
    }
    
    // 웹뷰 alert 허용
    func webView(_ webView: WKWebView, runJavaScriptAlertPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping () -> Void) {
        
        let alertController = UIAlertController(title: message, message: nil, preferredStyle: .alert);
        let cancelAction = UIAlertAction(title: "확인", style: .cancel) { _ in completionHandler() }
        alertController.addAction(cancelAction)
        DispatchQueue.main.async { self.present(alertController, animated: true, completion: nil) }
        
    }
}
```