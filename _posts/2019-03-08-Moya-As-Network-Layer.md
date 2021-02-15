---
title: Moya As Network Layer With Codable
author: Onur Genes
date: 2019-03-18 16:16:00 +0300
tags: [ios, moya, network]
categories: [iOS, Network]
math: false
toc: true
---


## What is [Moya](https://github.com/Moya/Moya/)? And Why?

> Moya is that we want some network abstraction layer that sufficiently encapsulates actually calling Alamofire directly. It should be simple enough that common things are easy, but comprehensive enough that complicated things are also easy.


### Moya's awesome features:

* Compile-time checking for correct API endpoint accesses.
* Lets you define a clear usage of different endpoints with associated enum values.
* Treats test stubs as first-class citizens so unit testing is super-easy.

## Installation

Installation part is described really well on [Moya's Github Page](https://github.com/Moya/Moya/).

## The API

We will use [JSONPlaceholder](https://jsonplaceholder.typicode.com/) as our API.

## Let's Start With Models

We will use `typealias Codable = Encodable & Decodable` for out model.

```swift
typealias Codable = Encodable & Decodable

struct Post: Codable {
    
    var userId: Int
    var id: Int
    var title: String
    var body: String
    
    enum CodingKeys: String, CodingKey {
        case userId, id, title, body
    }
}
```

## Create API Enum with `TargetType`

For keeping the post as easy as possible we will just use 1 POST and 2 GET route.

Let's create API Enum:

```swift
enum JSONPlaceHolderAPI {
    case getPost()
    case getPostWith(id: Int)
    case createPost(post: Post)
}
```

And add Moya's necessary extensions on top of it:

First, `baseURL`. This is used for our API's base URL:

```swift
var baseURL: URL {
    return URL(string: "https://jsonplaceholder.typicode.com")!
}
```

Next, `path`. We are using this for defining different routes (e.g. `/posts`, `/comments` etc.):

```swift
var path: String {
    return "/posts"
}
```

Next, `method`. We can define different methods for different `func`s:

```swift
var method: Moya.Method {
    switch self {
    case .getPost:
        return .get
    case .getPostWith:
        return .get
    case .createPost:
        return .post
    }
}
```
Next, we need sample `Data` for testing. We will not cover this on that post. So, let's give it a simple `Data()`:

```swift
var sampleData: Data {
    return Data()
}
```

Next, we need to define our `task`s.
* We need `.requestPlain` for getting all `Post`s.
* `.requestParameters` for getting specific `Post`.
* Lastly, `.requestJSONEncodable` for creating a `Post` with our `Codable` object.

```swift
var task: Task {
    switch self {
    case .getPost:
        return .requestPlain
    case .getPostWith(let id):
        return .requestParameters(parameters: ["id": id], encoding: JSONEncoding.default)
    case .createPost(let post):
        return .requestJSONEncodable(post)
    }
}
```

Last, we need `headers`. This will be sent with all of our requests:

```swift
var headers: [String : String]? {
    return ["Content-type": "application/json; charset=UTF-8"]
}
```

## Create our `Networkable` Delegate

Our `NetworkManager` will need to conform to `Networkable`. We will create our protocol like this:

```swift
protocol Networkable {
    var provider: MoyaProvider<JSONPlaceHolderAPI> { get }
    
    func getPosts(completion: @escaping ([Post]?, Error?) -> ())
    func getPostWith(id: Int, completion: @escaping (Post?, Error?) -> ())
    func createPosth(post: Post, completion: @escaping (Post?, Error?) -> ())
}
```

## Now, `NetworkManager`

We will create a `NetworkManager` object for [Dependency Injection](https://www.swiftbysundell.comdifferent-flavors-of-dependency-injection-in-swift). Let's create:

```swift
class NetworkManager: Networkable {
    
    var provider = MoyaProvider<JSONPlaceHolderAPI>(plugins: [NetworkLoggerPlugin(verbose: true)])
    
    func getPosts(completion: @escaping ([Post]?, Error?) -> ()) {
        provider.request(.getPost()) { (response) in
            switch response.result {
            case .failure(let error):
                completion(nil, error)
            case .success(let value):
                let decoder = JSONDecoder()
                do {
                    let posts = try decoder.decode([Post].self, from: value.data)
                    completion(posts, nil)
                } catch let error {
                    completion(nil, error)
                }
            }
        }
    }
    
    func getPostWith(id: Int, completion: @escaping (Post?, Error?) -> ()) {
        provider.request(.getPostWith(id: id)) { (response) in
            switch response.result {
            case .failure(let error):
                completion(nil, error)
            case .success(let value):
                let decoder = JSONDecoder()
                do {
                    let post = try decoder.decode(Post.self, from: value.data)
                    completion(post, nil)
                } catch let error {
                    completion(nil, error)
                }
            }
        }
    }
    
    func createPosth(post: Post, completion: @escaping (Post?, Error?) -> ()) {
        provider.request(.createPost(post: post)) { (response) in
            switch response.result {
            case .failure(let error):
                completion(nil, error)
            case .success(let value):
                let decoder = JSONDecoder()
                do {
                    let post = try decoder.decode(Post.self, from: value.data)
                    completion(post, nil)
                } catch let error {
                    completion(nil, error)
                }
            }
        }
    }
}
```

TL;DR: We have created a NetworkManager with necessary request `func`s and now we can create an object of that class and use it for requesting.

### Note:

I am using a `Moya plugin` for showing all requests and responses on console in that line:
```swift
var provider = MoyaProvider<JSONPlaceHolderAPI>(plugins: [NetworkLoggerPlugin(verbose: true)])
```

This is not necessary on production.

## Let's Test Our Code

We can use our `NetworkManager` like this, (without `Dependency Injection`):

```swift
class ViewController: UIViewController {

    let networkManager = NetworkManager()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        networkManager.getPosts { (posts, error) in
            if let error = error {
                print(error)
                return
            }
            dump(posts)
        }
    }
}
```
Note: With `dump` you can get more stylized version of `print`.

## That's It!

Now you can use `Moya`.