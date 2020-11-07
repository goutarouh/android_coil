# `ImageLoader`

- 以下など簡単に管理できる

  -  `caching`
  - `data fetching`
  - `image decoding`
  - `request management`
  - `bitmap pooling`
  - `memory management`  など

- `builder`を使用して以下のように簡単に設定可能

  ```kotlin
  val imageLoader = ImageLoader.Builder(context)
      .availableMemoryPercentage(0.25)
      .crossfade(true)
      .build()
  ```

  

- 一つのアプリ内でシングルオブジェクトで使用するべき

  - `ImageLoader`のそれぞれが`network observer`や`memory cache`の情報を持っているため。
  - もし複数使用した場合は`shutdown`メソッドを使用後に使用してね。
  - シングルトン的に使用した場合は`shutdown`する必要はない。アプリプロセスが殺されたときに同時に`shutdown`されるため。



# Caching

- `ImageLoader`はdecodeされたbitmapのメモリキャッシュを持ってる。

- キャッシュの設定は`OkHttpClient`に依存

  - デフォルトでは10-250Mbのキャッシュスペースがある。

- 以下のようにカスタムでキャッシュを設定することも可能(らしい)

  ```kotlin
  val imageLoader = ImageLoader.Builder(context)
      .okHttpClient {
          OkHttpClient.Builder()
              .cache(CoilUtils.createDefaultCache(context))
              .build()
      }
      .build()
  ```



# Image Requests

- リクエストに必要な情報

  ```kotlin
  val request = ImageRequest.Builder(this)
  	.data("url")
  	.target(imageView)
  	.build
  ```



# target

- リクエストの結果を受けととる

  ```kotlin
  val request = ImageRequest.Builder(this)
  	.data("url")
  	.target(
          onStart = { result -> process1}
          onSuccess = {result -> process2}
          onError = { error -> process3}
      )
  	.build()
  ```



# `TransFormations`

- デフォルトで以下の4つ
  - blue
  - circle crop
  - grayscale
  - rounded corners



# Transitions

- アニメーション可能