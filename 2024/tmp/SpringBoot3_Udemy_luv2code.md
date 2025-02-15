## 1章
- コースの時間→2008分（３３時間ほど）
- 一日30分ずつ消化で６６日。つまり2ヶ月ほどかかる。

- springBootではすぐ始められるようにHTサーバーは組み込まれている。なので別途Tomcatやundertowなどのサーバを別途インストールしなくていい。
- Mavenに買い物リストを渡しておくだけで、MavenはこれらのプロジェクトのJARファイルをダウンロードしてくれる｡
- 「依存関係A､ B､ C､ Dが必要だ｡ そしてMavenは､ これらのJARファイルを取得してクラスパスに追加し､コンパイルと実行時に利用できるようにする｡
- springイニシャライザーでスナップショット・バージョンはアルファ/ベータ版なので避けること｡
```txt
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/

 :: Spring Boot ::                (v3.4.1)

2025-01-20T23:05:14.746+09:00  INFO 48177 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : Starting MycoolappApplication using Java 22.0.2 with PID 48177 (/Users/k_tanaka/other_learn/spring/dev-spring-boot/mycoolapp/target/classes started by k_tanaka in /Users/k_tanaka/other_learn/spring/dev-spring-boot/mycoolapp)
2025-01-20T23:05:14.747+09:00  INFO 48177 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : No active profile set, falling back to 1 default profile: "default"
2025-01-20T23:05:15.066+09:00  INFO 48177 --- [mycoolapp] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 8080 (http)
2025-01-20T23:05:15.071+09:00  INFO 48177 --- [mycoolapp] [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2025-01-20T23:05:15.072+09:00  INFO 48177 --- [mycoolapp] [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.34]
2025-01-20T23:05:15.091+09:00  INFO 48177 --- [mycoolapp] [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2025-01-20T23:05:15.092+09:00  INFO 48177 --- [mycoolapp] [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 324 ms
2025-01-20T23:05:15.220+09:00  INFO 48177 --- [mycoolapp] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
2025-01-20T23:05:15.223+09:00  INFO 48177 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : Started MycoolappApplication in 0.777 seconds (process running for 1.007)
```
    - 上記がSPRINGアプリケーションを起動した時のログ。
    - `Tomcat initialized with port 8080 (http)`は、これはspringではtomcatなどのサーバが埋め込まれていてアプリ実行時に勝手に立ち上がってくれる。

- いかのように@RestControllerのついたクラスと、その中にgetmappingを("/")で書くことで、http://localhost:8080にアクセス時にhello!が表示されるようになった。
```java
package com.luv2code.springboot.demo.mycoolapp.rest;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class FunRestController {
    @GetMapping("/")
    public String sayHello(){
        return "hello!";
    }
}
```

- Maven 
    - Mavenはプロジェクトmanagementツール。一番はビルド管理と依存関係の管理につかわれる
    - Mavenに依存関係を教えてあげるだけで勝手にインターネットからJARファイルをダウンロードしてくれル。
    - そしてMAvenはコンパイル時やランタイム時にそれらのJARファイルを利用できるようにしてくれル。
    - 買い物リスト(Spring, JSON, CommonLogging, Hibernate)を渡すだけで買ってきてくれる感じ。
    - pom.xmlがその買い物リスト
    - Project Object Model ファイルの略→つまりはプロジェクトの設定ファイル
    - POMファイル内は(project meta data, dependencies, plug ins)がある
    - POMファイルにはSpringinitializerで入寮したものが含まれている
        - meta dataは基本的にプロジェクトに関する情報。
            ```xml
            <groupId>4.0.0</groupId>
            <artifactId>mycoolapp</artifactId>
            <version>1.0.FINAL</version>
            <packaging>jar</packaging>

            <name>mycoolapp</name>
            ```
            - プロジェクト座標はプロジェクトを一位に識別する。緯度と軽度緯度と軽度のようなGPSみたいなもの(自分の家を見つける正確な情報、住所みたいなものだと思えばいい。)
            - groupId=市町村(あなたの会社名、グループ名、組織名。逆ドメインを書く)
            - artifactIDはstreet(プロジェクトの名前)
            - versionは部屋番号的な感じ
        - dependenceisは依存しているもののリスト
            ```xml
            <project ...>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>spring-context</artifactId>
                        <version>6.0.0</version>
                    </dependency>

                    <dependency>
                        <groupId>org.hibernate.orm</groupId>
                        <artifactId>hibernate-core</artifactId>
                        <version>6.0.4.FINAL</version>
                    </dependency>

                    ...
                </dependencies>
            </project>
            ```
            - 依存関係を追加する際は依存セクションにそれらの依存関係を追加するだけ。
            - 依存関係を追加する際はGroupId,artifactIdが必要。バージョンは実際には任意ですが含めておくのがベスト
            - 上記３要素をGAVという。（このプロジェクトのGAVはなに？とか聞いたりするｓ）
            - じゃあ上記のGAVはどこで確認できるのかというと、SpringのwebサイトやHibernateのサイトを見れば、Mavenを使って依存関係を追加する際の必要な詳細が書いてある。
            - またはMaven central repositoryに行ってそれらの依存関係を検索する方法もある（こっちのが簡単）
        - plug ins は追加のカスタムタスク的な。

        - View　→ Tool Window → Maven → dependenciesから spring initializerで追加した依存関係が見れる

- spirng boot dev tools
    - pom.xmlに以下を追加
    ```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
    ```
    - 変更を保存時に自動でビルド、起動までやってくれる設定
        - settings→build,Execution,deployment→compiler → build project automaticallyをチェック
        - settings → advanced settings → Compiler → allow auto-make to...をチェック
        - https://support.samuraism.com/jetbrains/ide/idea-auto-compilation
        - https://qiita.com/IsaoTakahashi/items/f99d5f761d1d4190860d
    - pom.xmlを修正時には、ｐom.xmlファイル内の右上のリロードボタンを押すと更新される

- spring boot actuator
    - 実際にアプリケーションを管理監視するためのエンドポイントを公開するものです
    - だから簡単にdevopsの機能を手に入れられる
    - https://qiita.com/HiroyaEnd/items/f640a6cd2657c42c69a2

    - edit pom.xml and add spring-boot-starter-actuator
    - view actuator endpoints for: /health
    - edit applications.properties to customize /info

    - pom.xmlに以下を追加
    ```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
    ```
    - エンドポイントを追加するにはapplications.propertiesファイルに以下を設定
        ```properties
            management.endpoints.web.exposure.include=health, info, bean
            management.info.env,enabled=true
        ```
        - https://spring.pleiades.io/spring-boot/reference/actuator/endpoints.html

    - あとは「http://localhost:8080/actuator/health」などにアクセスするだけ
        - healthはアプリケーションがUpかDownしてるかを見れる

    - 以下のようにinfo以下のパスに値を入れておくと「http://localhost:8080/actuator/info」アクセス時に画面にJSON形式で表示される
    ```properties
        info.app.name = mt super cool app
        info.app.description = yeahhhhhhhhhhhhhhhhh,nice toomeetyou
        info.app.version = 1.0.0
    ```
    - 以下のchromeプラグインで画面のJSONが綺麗に整形されて表示される
        「https://chromewebstore.google.com/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=ja&pli=1」
    
    - beansエンドポイントはアプリケーションに登録されているすべてのbeanが表示される
    - endpointsエンドピントはアクセス可能なエンドポイントが一覧で表示される

    - securityに関して。（上記のアクチュエータはエンドユーザからは見えないようにして管理者だけ見えるようにしないとまずい。）
        - pom.xmlに以下の依存関係を追加することで自動でRESTエンドポイントにセキュリティを持たせることができる
        ```xml

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
        ```

        - 上記を追加してアプリを起動するだけで、以下のように起動時のログに「パスワード」が生成される
        ```txt
        2025-02-15T18:13:05.080+09:00  WARN 73504 --- [mycoolapp] [  restartedMain] .s.s.UserDetailsServiceAutoConfiguration : 

        Using generated security password: a40bdcae-f742-4f09-97ee-3ec934aebb33

        This generated password is for development use only. Your security configuration must be updated before running your application in production.
        ```

        - ↑のパスワードをactuatorにアクセス時に求められるようになるので入力する

        - 以下のように書くことで、actuatorのパスを一部無効化できる
            - 以下の設定で「http://localhost:8080/actuator/health」「http://localhost:8080/actuator/info」
            のアクセス時に404が表示される。
            ```properties
            management.endpoints.web.exposure.exclude=health,info
            ```

- sprinbootアプリをコマンドラインで起動する
    - mvnwはMavenを手動でインストールすることなく、インターネットから正しいバージョンのMavenを自動的にダウンロードするための特別なラッパーコマンドである。
    - springbootプロジェクトの直下（mvnwとかpom.xmlとかあるディレクトリ）で以下コマンド打つとJarファイルを生成してくれる
        - 「./mvnw package」
        - Jarファイルはtargetのサブディレクトリに生成される
    
    - で、projectディレクトリ配下のtargetディレクトリ下にjarができたのでtagetに移り以下コマンドを打つ
    - 「java -jar mycoolapp-0.0.1-SNAPSHOT.jar」
    - これで「http://localhost:8080」niakusesudekiru

    ```txt
        .   ____          _            __ _ _
        /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
        ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
        \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
        '  |____| .__|_| |_|_| |_\__, | / / / /
        =========|_|==============|___/=/_/_/_/

        :: Spring Boot ::                (v3.4.1)

        2025-02-15T18:37:01.752+09:00  INFO 81031 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : Starting MycoolappApplication v0.0.1-SNAPSHOT using Java 22.0.2 with PID 81031 (/Users/k_tanaka/other_learn/spring/dev-spring-boot/01-spring-boot-overview/05-command-line-demo/target/mycoolapp-0.0.1-SNAPSHOT.jar started by k_tanaka in /Users/k_tanaka/other_learn/spring/dev-spring-boot/01-spring-boot-overview/05-command-line-demo/target)
        2025-02-15T18:37:01.753+09:00  INFO 81031 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : No active profile set, falling back to 1 default profile: "default"
        2025-02-15T18:37:02.105+09:00  INFO 81031 --- [mycoolapp] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 8080 (http)
        2025-02-15T18:37:02.113+09:00  INFO 81031 --- [mycoolapp] [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
        2025-02-15T18:37:02.113+09:00  INFO 81031 --- [mycoolapp] [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.34]
        2025-02-15T18:37:02.127+09:00  INFO 81031 --- [mycoolapp] [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
        2025-02-15T18:37:02.127+09:00  INFO 81031 --- [mycoolapp] [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 350 ms
        2025-02-15T18:37:02.279+09:00  INFO 81031 --- [mycoolapp] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
        2025-02-15T18:37:02.288+09:00  INFO 81031 --- [mycoolapp] [           main] c.l.s.d.mycoolapp.MycoolappApplication   : Started MycoolappApplication in 0.735 seconds (process running for 1.022)
    ```

    - プロジェクトディレクトリ直下で以下コマンド打つだけでもアプリを起動できる
        「./mvnw spring-boot:run」

- custom applications.properties

    - application.propertiesファイルで以下のようにじぶんでｈ円数を設定した場合、
        ```properties
        coach.name=Mickeymouse
        team.name=my club
        ```

    - @RestCOntrollerのクラスから読み込みたい時は以下のように書くだけで使える。
        - @Valueアノテーションを利用してこれらのプロパティを注入したのである
    ```java
    package com.luv2code.springboot.demo.mycoolapp.rest;

    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class FunRestController {
        @Value("${coach.name}")  // ←これ！！！！！！
        private String coachName;

        @Value("${team.name}")  // ←これ！！！！！！
        private String teamName;


        @GetMapping("/")
        public String sayHello(){
            return "hello!";
        }

        @GetMapping("/workout")
        public String getDailyWorkOut(){
            return "run 5 km !!!!!!!!!!";
        }

        @GetMapping("/fortune")
        public String getFortune(){
            return "fortune``````````!!";
        }

        @GetMapping("/coach")
        public String getCoachName(){
            return this.coachName;
        }

        @GetMapping("/team")
        public String getTeamName(){
            return teamName;
        }
    }

    ```

- spring boot properties
    - server port, context path, actuator, secutiry ,etc...
    - 1000を超えるプロパティがある
    - 以下にプロパティーの一覧が載ってる
        - https://spring.pleiades.io/spring-boot/appendix/application-properties/
    - １０００を超えるが、以下4つに分類されているので安心
        - Core
        - Web
        - Security
        - Data
        - Actuator
        - Integration
        - DevTools
        - Testing
    
    - 例えば「server.port= 7070」をapplication.propertiesに設定すると、サーバーのポートが7070で起動する
    - 「http://localhost:7070/」でアクセスできる
        ```txt
        2025-02-15T19:32:11.549+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] c.l.s.d.mycoolapp.MycoolappApplication   : Starting MycoolappApplication using Java 22.0.2 with PID 85850 (/Users/k_tanaka/other_learn/spring/dev-spring-boot/01-spring-boot-overview/06-properties-demo/target/classes started by k_tanaka in /Users/k_tanaka/other_learn/spring/dev-spring-boot/01-spring-boot-overview/06-properties-demo)
        2025-02-15T19:32:11.549+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] c.l.s.d.mycoolapp.MycoolappApplication   : No active profile set, falling back to 1 default profile: "default"
        2025-02-15T19:32:11.606+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 7070 (http)
        2025-02-15T19:32:11.606+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
        2025-02-15T19:32:11.606+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.34]
        2025-02-15T19:32:11.609+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
        2025-02-15T19:32:11.609+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 59 ms
        2025-02-15T19:32:11.631+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
        2025-02-15T19:32:11.634+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 7070 (http) with context path '/' ←ここ！！！！！！！！！！！！！！！！！
        2025-02-15T19:32:11.635+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] c.l.s.d.mycoolapp.MycoolappApplication   : Started MycoolappApplication in 0.094 seconds (process running for 2401.394)
        2025-02-15T19:32:11.635+09:00  INFO 85850 --- [mycoolapp] [  restartedMain] .ConditionEvaluationDeltaLoggingListener : Condition evaluation unchanged
        ```
    - 以下を設定すると、すべてのリクエスとのパスにmycoolappがプレフィックすがつけられる
        - 「server.servlet.contextpath=/mycoolapp」
        - よって「http://localhost:7070/mycoolapp/coach」な感じですべてのパスにmycoolappをつけることになる