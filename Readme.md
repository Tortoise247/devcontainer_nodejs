## プロジェクト概要
このプロジェクトは、Electron アプリケーションの開発環境を Docker を使用して構築することを目的としています。
以下は、docker-compose.yml ファイルの内容に基づいたプロジェクトの詳細な説明です。

Windows（WSL2）で、コンテナ上にNode.jsの開発環境を構築します。
画面出力のため、WSL2のX11ソケットおよび、Waylandをマウントして利用します。

### 使用技術
- Docker
    - コンテナ化技術を使用して、開発環境を一貫性のあるものにします。
- Electron
    - クロスプラットフォームのデスクトップアプリケーションを構築するためのフレームワーク。

### docker-compose.yml の構成
- バージョン
  - version: '3.8'
- service
    - electron_app
      - build
        - context
          - ./electron_dev ディレクトリをビルドコンテキストとして使用します。
        - args
          -  環境変数 ROOT_PASSWORD をビルド時に渡します。
     -  image: electron_dev:0.1 という名前のイメージを使用します。
     -  container_name: コンテナの名前を electron_dev に設定します。
     -  volumes:
        -  ./app:/app
           -  ローカルの ./app ディレクトリをコンテナ内の /app にマウントします。
        -  \\wsl.localhost\${DISTRO}\tmp\.X11-unix
           -  /tmp/.X11-unix: WSL の X11 ソケットをマウントします。
        -  \\wsl.localhost\${DISTRO}\mnt\wslg\runtime-dir
           -  /mnt/wslg/runtime-dir/: WSLg のランタイムディレクトリをマウントします。
        -  privileged: 特権モードを有効にします。
        -  working_dir: 作業ディレクトリを /app に設定します。
        -  networks: electron_dev_network ネットワークに接続します。
        -  tty: TTY を有効にします。
        -  ports:
           -  9223:9223: ホストのポート 9223 をコンテナのポート 9223 にマッピングします。
     -  networks:
        -  electron_dev_network:
           -  driver: bridge
