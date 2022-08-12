# Getting started

* [Setup client security](#setup-client-security)


# Setup client security

## Register ClientAuthService

In order to provide security to all of our frameworks, you need to configure **ClientAuthorizationService**, that handles token authentication and renewal. 

You can configure this in the **SceneDelegate** like so:

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?
    
    private lazy var navigationController = UINavigationController(rootViewController: FlowsViewController())

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
                
        self.window = UIWindow(windowScene: windowScene)
        window?.rootViewController = navigationController
        window?.makeKeyAndVisible()
        PomeloNetworkConfigurator.shared.configure(authorizationService: ClientAuthorizationService())
    }
}
```

## Implementing ClientAuthService

```swift
protocol AccessTokenResolverProtocol {
    func resolve(completion: @escaping (String?) -> Void)
}

class AccessTokenResolver: AccessTokenResolverProtocol {

    func resolve(completion: @escaping (String?) -> Void) {
        guard let baseURL = Bundle.main.infoDictionary?["CLIENT_BASE_URL"] as? String,
              let url = URL(string: "\(baseURL)/oauth/token") else {
            completion(nil)
            return
        }
        
        let session = URLSession.shared
        
        var urlRequest = URLRequest(url: url)
        urlRequest.httpMethod = "GET"
                
        let task = session.dataTask(with: urlRequest) { data, response, error in
            if let error = error {
                completion(nil)
            }
            
            let decoder = JSONDecoder()
            
            do {
                let dto = try decoder.decode(PomeloAccessTokenDTO.self, from: data!)
                completion(dto.accessToken)
            } catch {
                completion(nil)
            }
        }
        task.resume()
    }
}
```

