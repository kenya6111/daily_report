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


## 2章（Spring Core）
- Inversion of control（DI-依存性の注入）
    - 俺たちがオブジェクトを生成はせず、アプリに作ってもらうこと。
    - インスタンスの生成と依存関係の注入をDIコンテナが提供する
    - DIコンテナの主な機能
        - インスタンスの生成と管理などライフサイクルを生業できる
        - 依存関係の注入ができる
    
    - injectionには2種類ある
        - Constructor Injection
        - Setter Injection
    
    - Auto Wiringとは
        - @Autowired
            - Springに依存関係を注入するように指示している。
    
    - @Componentアノテーション
        - これをクラスにつけるとSpring Beanとして認識される（依存性注入の候補になる）
        - つまり依存性注入に利用できるようにしてくれるアノテーション。
    
    - @Autowiredアノテーション
        - Autowiredは依存関係を注入するようにspringに命令する

- サンプル
    - 以下のように３ファイル用意した。
    - DemoController.java
    ```java
    package com.luv2code.springcoredemo;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.autoconfigure.AutoConfiguration;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class DemoController {
        private Coach myCoach;

        @Autowired
        public DemoController(Coach theCoach){
            myCoach = theCoach;
        }
        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }
    }
    ```

    - CricketCoach.java
    ```java
    package com.luv2code.springcoredemo;

    import org.springframework.stereotype.Component;

    @Component
    public class CricketCoach implements Coach{
        @Override
        public String getDailyWorkout() {
            return "practice fast bowling  for 1256 minutes";
        }
    }
    ```

    - Coach.java
    ```java
    package com.luv2code.springcoredemo;
    public interface Coach {
        String getDailyWorkout();
    }
    ```

    - 上記で画面にはpractice fast bowling  for 1256 minutesが表示される
    - 以下、自分で調べた裏側の動き
    ```txt
    public class DemoController {
        private Coach myCoach;

        @Autowired
        public DemoController(Coach theCoach){
            myCoach = theCoach;
        }
        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }
    }

    Q.なんでDemoControllerでインジェクトするのがCoachなのか（最初からCricketCoachとかBasebollCoachとかをAutowiredすればいいやないか）
    A.ダメじゃない。でも 将来の変更に弱いコード になっちゃう。
    小さいアプリなら CricketCoach 直指定でも問題ないけど、拡張性を考えると インターフェースを使う方がベストプラクティス なんだ。
    「後で変えたくなったら修正しなくちゃいけない」ってのを避けるために、最初から Coach を指定して、Spring に実装の選択を任せるのがスマートな方法ってこと！

    具体的な説明は以下⇩
    Spring の DI（依存性注入） の考え方は、「具体的な実装（CricketCoach）ではなく、抽象（Coach）に依存する」 という設計原則に基づいている。
    もし以下のように書くと
    @Autowired
    public DemoController(CricketCoach theCoach){
        myCoach = theCoach;
    }
    DemoController は CricketCoach にガチガチに依存する ことになる。

    これの何が問題なのか？？

    例えば、後で「やっぱ BaseballCoach に変えたいな」と思ったときに、この DemoController を 毎回変更しなきゃいけない。

    @Autowired
    public DemoController(BaseballCoach theCoach){
        myCoach = theCoach;
    }

    プロジェクト全体で 100個のクラスが BaseballCoach に依存していたら？
    → 100箇所の修正が必要 になるよね

    🟢 Coach を指定する場合
    @Autowired
    public DemoController(@Coach theCoach){
        myCoach = theCoach;
    }

    - @Primaryで、DemoController のコードは 一切変更しなくても、Spring が BaseballCoach を自動で選んでくれる。
    @Component
    @Primary  // これをつけた Coach がデフォルトで選ばれる
    public class BaseballCoach implements Coach {
        @Override
        public String getDailyWorkout() {
            return "Practice hitting for 30 minutes";
        }
    }

    その中で部分的に他のコートをdemoControllerで使いたいとなれば「@Qualifier」を使えばいい・
    @Autowired
    public DemoController(@Qualifier("baseballCoach") Coach theCoach){
        myCoach = theCoach;
    }
    ```

- コンポーネントScan
    - Springはjavaクラスや@Component裏酢などの特別なアノテーションをスキャンし自動でSpringコンテナにビーンズを登録します
    - コンポーネント Sキャンはメインのspringbootapplicationだけに行われる(要はアプリ起動時に実行する@springbootapplicationアノテーショんを持つクラスのこと)
    ![alt text](../../image/image9.png)

    - デフォルトはプロジェクトの名前のディレクトリは以下だけが Sキャンされる。
    - ので以下のようにcom.luv2code配下にspringcoredemo(最初に作ったプロジェクト名)の他にディレクトリ追加してそこにインジェクトされていたクラスを写すとスキャンされないので、そのクラスをつ買っていたdemocontrolerでは見つけられませんとエラーが起こり機動に失敗する。
    - なので、@springBootApplicationアノテーションに以下の引数を追加してスキャンできるディレクトリを追加してあげる
    ```java
    @SpringBootApplication(
		scanBasePackages = {"com.luv2code.springcoredemo",
							"com.luv2code.util"}
    )
    ```
    ![alt text](../../image/image10.png)

- setter Injection
    - クラスのセッターメソッドを呼び出すことで依存関係を注入すること。
    - @Autowiredのところで、さっきはインスタンスメソッドだったが、セッターメソッドでも良い。その場合、名前はなんでもいい。勝手にインジェクションしてくれる
    ```java
    package com.luv2code.springcoredemo.rest;

    import com.luv2code.springcoredemo.common.Coach;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class DemoController {
        private Coach myCoach;

    //    @Autowired
    //    public DemoController(Coach theCoach){
    //        myCoach = theCoach;
    //
    //    }

        @Autowired
        public  void setCoach(Coach theCoach){
            myCoach = theCoach;
        }


        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }


    }
    ```

- Field Injection
    - 最近は非推奨のインジェクション方法
    - コードのユニットテストが難しくなるから

- Qualifierアノテーション
    - Qualifierアノテーションでどのクラスをインジェクトするか指定できる
    - basebollCoach, tennisCOachなど、同じCoachk裏巣をimplementsし@component付きのCoachのクラスがたくさんいる場合は
    - 基本どれかに＠Primaryをつけて、例外的に他のCoachを使いたい場合に「Qualifier」を使う

    ```java
    package com.luv2code.springcoredemo.rest;

    import com.luv2code.springcoredemo.common.Coach;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class DemoController {
        private Coach myCoach;

        @Autowired
        public DemoController(@Qualifier("trackCoach") Coach theCoach){
            myCoach = theCoach;
        }

        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }
    }
    ```

- Primaryアノテーション
    - 以下のようにコントローラでインタフェースのコーチをインジェクションしている場合、そのインタフェースをimplementsしているCoachが複数あってそれぞれにcomponentがついている場合、そのどれかに@Primaryを追加で付与すればそれがデフォルトでインジェクションされる。
    ```java
    package com.luv2code.springcoredemo.rest;

    import com.luv2code.springcoredemo.common.Coach;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class DemoController {
        private Coach myCoach;

        @Autowired
        public DemoController(Coach theCoach){
            myCoach = theCoach;
        }

        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }
    }
    ```
    - TrackCoach.java
    ```java
    package com.luv2code.springcoredemo.common;
    import org.springframework.context.annotation.Primary;
    import org.springframework.stereotype.Component;

    @Component
    @Primary
    public class TrackCoach implements Coach{
        @Override
        public String getDailyWorkout() {
            return "RUn a hard 5 k";
        }
    }
    ```

- LazyInitialization(遅延初期化)
    - デフォルトではすべてのコンポーネントをスキャンし初期化する
    - 遅延初期化とは、そのフィールドが必要となるまで初期化を遅らせる行為のことを指します。遅延初期化を行う理由としては、初期化に多大なコストがかかるフィールドが存在した場合、遅延初期化を実装することによってパフォーマンスを向上させることができる点が挙げられます。
    - 「必要となるまで」というのは「依存性の注入に使われるとき」や「明示的に使われる時」である
    - @Lazyをつけるだけで遅延初期化が適用される
    - 「実際に必要とされない限り、私を作るな」ということになる

    - ただ全部のコンポーネントクラスに1つ1つ@Lazyをつけていくのはクソだるい
        - なので。propertiesファイルで「spring.main.lazy-initialization=true」を設定しておけばすべてのビーンズを遅延型にできる

    - DemoCOntroller.java
    ```java
    package com.luv2code.springcoredemo.rest;

    import com.luv2code.springcoredemo.common.Coach;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class DemoController {
        private Coach myCoach;

        @Autowired
        public DemoController(@Qualifier("cricketCoach") Coach theCoach){
            System.out.println("In constructor :" + getClass().getSimpleName());
            myCoach = theCoach;
        }

        @GetMapping("dailyworkout")
        public String getDailyWorkout(){
            return myCoach.getDailyWorkout();
        }
    }
    ```

    - CricketCoach.java
    ```java
    package com.luv2code.springcoredemo.common;
    import org.springframework.context.annotation.Primary;
    import org.springframework.stereotype.Component;

    @Component
    public class CricketCoach implements Coach{
        public CricketCoach(){
            System.out.println("In constructor :" + getClass().getSimpleName());
        }
        @Override
        public String getDailyWorkout() {
            return "practice fast bowling  for 1256 minutes!!!!!!!!!!";
        }
    }
    ```

    - BasebollCoach.java
    ```java
    package com.luv2code.springcoredemo.common;
    import org.springframework.stereotype.Component;

    @Component
    public class BasebollCoach  implements Coach{

        public BasebollCoach(){
            System.out.println("In constructor :" + getClass().getSimpleName());
        }


        @Override
        public String getDailyWorkout() {
            return "Spring 30 minutes in batting practice ";
        }
    }
    ```

    - TennisCoach.java
    ```java
    package com.luv2code.springcoredemo.common;

    import org.springframework.stereotype.Component;

    @Component
    public class TennisCoach implements Coach{
        public TennisCoach(){
            System.out.println("In constructor :" + getClass().getSimpleName());
        }

        @Override
        public String getDailyWorkout() {
            return "Practice your backend volley";
        }
    }
    ```

    - TrackCoach.java
    ```java
    package com.luv2code.springcoredemo.common;

    import org.springframework.context.annotation.Lazy;
    import org.springframework.context.annotation.Primary;
    import org.springframework.stereotype.Component;

    @Component
    public class TrackCoach implements Coach{
        public TrackCoach(){
            System.out.println("In constructor :" + getClass().getSimpleName());
        }
        @Override
        public String getDailyWorkout() {
            return "RUn a hard 5 k";
        }
    }
    ```

    - 上記でアプリ起動時は以下のログが出る。すべてのコンポおー年とが初期化されている
    ```txt
    2025-02-16T21:27:26.618+09:00  INFO 54711 --- [springcoredemo] [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 325 ms
    In constructor :BasebollCoach
    In constructor :CricketCoach
    In constructor :TennisCoach
    In constructor :TrackCoach
    In constructor :DemoController
    ```

    - TrackCoachだけに「＠Lazy」アノテーションを付与すると、アプリ起動時はログに出てこない、つまり初期化されてない
    - trackCoachは特にインジェクションなど必要とされていないので出番がないからだ。出番が来たら、その時に初期化される。
        ```java
        package com.luv2code.springcoredemo.common;

        import org.springframework.context.annotation.Lazy;
        import org.springframework.context.annotation.Primary;
        import org.springframework.stereotype.Component;

        @Component
        @Lazy
        public class TrackCoach implements Coach{
            public TrackCoach(){
                System.out.println("In constructor :" + getClass().getSimpleName());
            }
            @Override
            public String getDailyWorkout() {
                return "RUn a hard 5 k";
            }
        }
        ```
        ```txt
        2025-02-16T21:28:25.837+09:00  INFO 54711 --- [springcoredemo] [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 78 ms
        In constructor :BasebollCoach
        In constructor :CricketCoach
        In constructor :TennisCoach
        In constructor :DemoController
        ```

        - propertiesファイルに以下のように書くと、すべてのビーンが遅延型になる
            ```properties
            spring.main.lazy-initialization=true
            ```
            - これでアプリ起動してみると何も初期化されていない
            - http://localhost:8080/dailyworkoutにアクセスして初めて以下のログが出ている。2つだけコンポーネントが初期化されている
            ```txt
            In constructor :CricketCoach
            In constructor :DemoController
            ```

- Bean Scopes
    - 「Spring コンテナが Bean をどのように管理・生成・共有するかを決める設定」のこと
    - 「どのタイミングでオブジェクトを作るべきか？」 によって選択するのが重要！
    - デフォルトスコープはsingleton

    - sngleton
        - アプリケーション全体で 1つのインスタンス だけを作る
        - そのBeanに対するすべての依存性注入は同じBeanを参照することになる。
        - 以下の場合は、特にBeanのスコープを指定していないので、すべてのBeanがsingletonになる
        - なので、theCoachとtheAnotherCoachは同じインスタンスになる。
        - 「http://localhost:8080/check」にアクセス時には「comparing beans : myCoach == anotherCoach true」が表示される。
        ```java
        package com.luv2code.springcoredemo.rest;

        import com.luv2code.springcoredemo.common.Coach;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.beans.factory.annotation.Qualifier;
        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.RestController;

        @RestController
        public class DemoController {
            private Coach myCoach;
            private Coach anotherCoach;

            @Autowired
            public DemoController(@Qualifier("cricketCoach") Coach theCoach,
                                @Qualifier("cricketCoach") Coach theAnotherCoach){
                System.out.println("In constructor :" + getClass().getSimpleName());
                myCoach = theCoach;
                anotherCoach = theAnotherCoach;
            }

            @GetMapping("dailyworkout")
            public String getDailyWorkout(){
                return myCoach.getDailyWorkout();
            }

            @GetMapping("/check")
            public String check(){
                return "comparing beans : myCoach == anotherCoach"+(myCoach == anotherCoach);
            }
        }
        ```
    - prototype
        - @Autowiredするたびに新しいインスタンス を作る
        - 「@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)」でBeanのスコープを指定できる。
        ー今回はprototypeに設定し、「comparing beans : myCoach == anotherCoach false」が表示される。
        ```java
        package com.luv2code.springcoredemo.common;

        import org.springframework.beans.factory.config.ConfigurableBeanFactory;
        import org.springframework.context.annotation.Primary;
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;

        @Component
        @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
        public class CricketCoach implements Coach{
            public CricketCoach(){
                System.out.println("In constructor :" + getClass().getSimpleName());
            }
            @Override
            public String getDailyWorkout() {
                return "practice fast bowling  for 1256 minutes!!!!!!!!!!";
            }
        }

        ```
    - request
        - HTTPリクエストごとに新しいインスタンス
    - session
    - application
    - websocket

- Bean Lifecycle Methods
    - Spring Beanが生成されてから破棄されるまでの過程を指します
    - Beanのインスタンス化､ 依存関係の注入､ Spring内部の処理が行われ､
        その後､ 独自にカスタマイズした初期化メソッドを実行することができます｡
        そして､ その時点で､ 豆が使えるようになるのです｡
        そして､ コンテナがシャットダウンまたは停止されると､ 実際にカスタムのdestroyメソッドを呼び出すことになります｡

    - Beanの初期化時に独自のカスタムコードを追加することができます｡

    - @PostConstruct
        - Beanが初期化された直後に実行されるメソッドを定義するためのアノテーション」のことです。たとえば、Springアプリケーションが起動したときに「何か特別な処理を一度だけ行いたい！」という場面がありますよね。その場合に@PostConstructを使うと便利です。
        - 初期化処理を行うメソッドに付けるアノテーション
    - @PreDestroy
        - 終了処理を行うメソッドに付けるアノテーション

    - CricketCoahクラスを以下のようにした。
        ```java
        package com.luv2code.springcoredemo.common;
        import jakarta.annotation.PostConstruct;
        import jakarta.annotation.PreDestroy;
        import org.springframework.beans.factory.config.ConfigurableBeanFactory;
        import org.springframework.context.annotation.Primary;
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;

        @Component
        public class CricketCoach implements Coach{
            public CricketCoach(){
                System.out.println("In constructor :" + getClass().getSimpleName());
            }

            @PostConstruct
            public  void domyStartUpStuf(){
                System.out.println("In doMyStartUpStuff(): "+ getClass().getSimpleName());
            }

            @PreDestroy
            public void doMyCleanUpStuff(){
                System.out.println("In doMyCleanUpStuff(): " + getClass().getSimpleName());
            }

            @Override
            public String getDailyWorkout() {
                return "practice fast bowling  for 1256 minutes!!!!!!!!!!";
            }
        }

        ```

        - アプリ起動時にはすべてのBeanが初期化され、
        ```txt
        2025-02-17T21:14:41.613+09:00  INFO 72210 --- [springcoredemo] [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 353 ms
        In constructor :BasebollCoach
        In constructor :CricketCoach
        In doMyStartUpStuff(): CricketCoach ←　ここ！！　Bean初期化時に呼ばれている
        In constructor :TennisCoach
        In constructor :TrackCoach
        In constructor :DemoController
        ```

        - 逆にアプリを落とした時は@PreDestroyのメソッドが呼ばれているのがわかる
        ```txt
        2025-02-17T21:14:41.730+09:00  INFO 72210 --- [springcoredemo] [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
        2025-02-17T21:14:41.738+09:00  INFO 72210 --- [springcoredemo] [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
        2025-02-17T21:14:41.742+09:00  INFO 72210 --- [springcoredemo] [  restartedMain] c.l.s.SpringcoredemoApplication          : Started SpringcoredemoApplication in 0.693 seconds (process running for 0.9)
        2025-02-17T21:18:05.455+09:00  INFO 72210 --- [springcoredemo] [ionShutdownHook] o.s.b.w.e.tomcat.GracefulShutdown        : Commencing graceful shutdown. Waiting for active requests to complete
        2025-02-17T21:18:05.460+09:00  INFO 72210 --- [springcoredemo] [tomcat-shutdown] o.s.b.w.e.tomcat.GracefulShutdown        : Graceful shutdown complete
        In doMyCleanUpStuff(): CricketCoach　　←ここ！！！

        Process finished with exit code 130 (interrupted by signal 2:SIGINT)
        ```
- java Config Bean
    - @Componentや@Autowiredを使わずにBeanを生成、管理する方法
    - Spring では通常、@Component や @Service などのアノテーションを使って Bean を自動登録するけど@Configuration + @Bean を使うことで、手動で Bean を定義することもできる。
    - この場合、Spring は swimCoach() を実行し、その戻り値（SwimCoach のインスタンス）を Spring コンテナ内の Bean として登録 する。
    ```java
    @Configuration
    public class AppConfig {

        @Bean
        public Coach swimCoach() {
            return new SwimCoach();
        }
    }
    ```

    - 1️⃣ @Configuration クラスで Bean を登録
        まずは DIコンテナ（Spring コンテナ）に Coach の Bean を登録 する。
        ```java
        @Configuration
        public class SportConfig {

            @Bean
            public Coach swimCoach() {
                return new SwimCoach();
            }

            @Bean
            public Coach baseballCoach() {
                return new BaseballCoach();
            }
        }
        ```
    
    - 2️⃣ Controller で Bean を Inject
        次に、別の場所（Controller）で @Autowired を使って Inject する。
        ```java
        @RestController
        public class DemoController {
            private final Coach myCoach;

            @Autowired
            public DemoController(@Qualifier("swimCoach") Coach theCoach) {
                this.myCoach = theCoach;
            }

            @GetMapping("/dailyworkout")
            public String getDailyWorkout() {
                return myCoach.getDailyWorkout();
            }
        }
        ```
        - 💡 @Configuration クラスで @Bean を定義し、DIコンテナに登録する
        - 💡 別のクラス（Controller など）で @Autowired して Inject する
        - 💡 @Qualifier("beanName") で特定の @Bean を指定できる

        - つまり 「設定クラスで Bean を作って、Controller で Inject して使う」 という流れで 合ってる！ 🚀

## 3章（Hibernate/JPA CRUD）
- Hiberante
    - JavaオブジェクトをDBに永続化または保存するためのフレームワーク
    - Java でデータベース操作を簡単にするための ORM（オブジェクト・リレーショナル・マッピング）フレームワーク。
    - 🔹 通常の JDBC（SQL を直接書く）(SQL を直接書く必要がある（面倒 & ミスしやすい）)
        ```java
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "pass");
        PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
        stmt.setInt(1, userId);
        ResultSet rs = stmt.executeQuery();
        ```
   - 🛠 Spring + Hibernate（Spring Data JPA）の基本構成
        - 1️⃣ Entity クラスを作る（データベースのテーブルに対応するクラス）
        ```java
        @Entity
        @Table(name="users")
        public class User {
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;

            private String name;

            // Getter & Setter
        }
        ```
        - 2️⃣ Repository インターフェースを作る
        ```java
            @Repository
            public interface UserRepository extends JpaRepository<User, Long> {
            }
            👆 この UserRepository を使えば、SQL を書かなくてもデータベース操作ができる！
        ```

        - 3️⃣ Service で UserRepository を使う
        ```java
            @Service
            public class UserService {
                @Autowired
                private UserRepository userRepository;

                public User getUserById(Long id) {
                    return userRepository.findById(id).orElse(null);
                }
            }
        ```
        - 4️⃣ Controller でデータ取得
        ```java
            @RestController
            @RequestMapping("/users")
            public class UserController {
                @Autowired
                private UserService userService;

                @GetMapping("/{id}")
                public User getUser(@PathVariable Long id) {
                    return userService.getUserById(id);
                }
            }
        ```
        - ✅ これで /users/1 にアクセスすると、ID=1 のユーザー情報が JSON で取得できる！

    - 1️⃣ JPA（Java Persistence API）の由来
        - Java Persistence API（Java の永続化 API） の略
        - 「Persistence（永続化）」= データを長期間保存する仕組み（＝データベースに保存すること）
        - Java で オブジェクトをデータベースに保存・管理するための標準仕様 として設計された
        - 👉 JPA は「Java のデータ永続化のための API」って意味！
    
    - 2️⃣ Hibernate の由来
        - Hibernate（ハイバネート）は、英語で 「冬眠」や「休眠」 の意味
        - Hibernate は データベースとオブジェクトの間で、不要なデータのロードを避けてリソースを節約する仕組み を持っている
        - 「データを必要なときだけ呼び出す（休眠してるデータを必要なときに目覚めさせる）」 という考えから、この名前が付いた
        - 👉 Hibernate は「不要なデータを Hibernate（冬眠）させて、パフォーマンスを最適化する ORM」！
    
    - 📌 ORM（Object-Relational Mapping）とは？
        - 1️⃣ ORM の定義
        - ORM（オブジェクト・リレーショナル・マッピング） は、
        - 「データベースのテーブル」と「Java のオブジェクト」を対応させて、SQL を直接書かずにデータを操作できる仕組み」 のこと。

    - 2️⃣ ORM がない場合（JDBC で SQL を直接書く）
        ```java
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "pass");
        PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
        stmt.setInt(1, userId);
        ResultSet rs = stmt.executeQuery();
        ```
        👆 SQL を直書きしないとデータを取得できない！（めんどい & バグの原因になりやすい）

    - 3️⃣ ORM を使うと…（JPA / Hibernate の場合）
        ```java
            @Entity
            @Table(name="users")
            public class User {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private String name;
            }
        ```
        ```java
            @Autowired
            private UserRepository userRepository;
            User user = userRepository.findById(1L).orElse(null);
        ```
        👆 SQL を書かずに、オブジェクトのメソッドだけでデータを取得できる！
    - ✅ ORM を使うと SQL を書かずにデータを扱える！
    - ✅ JPA は ORM の標準仕様で、Hibernate はその実装の1つ！ 
        - JPAはただのインタフェースで、その実装としてHibernateが最もポピュラー

    - 開発者としてはJavaクラスやオブジェクトとデータベースのデータとの対応づけをHibernateに伝えるだけで良い
        - 実際にはjavaクラスを所定のデータベーステーブルにマッピングすることになる
    ![alt text](../../image/image11.png)

    - でも結局Hibernate/JPAは、実はすべてのデータベース通信にバックグラウンドでJDBCを使っている
        - つまりhibernate/jpaはJDBCの上にある抽象化の別レイヤーにすぎない

- MysqlWorkBench
    - MySQLにアクセスするためのGUIであることを忘れないでください。

- EntityManager
    - EntityManager は、JPA（Java Persistence API）(仕様)を使ってデータベースとやり取りするためのオブジェクト
        - クエリの作成などに使うメインコンポーネント。entity manager is from JPA

    - EntityManager を使った基本操作
        - 1️⃣ EntityManager の取得
            - 通常、Spring Boot では JpaRepository を使うことが多いが、
            - EntityManager を使えば、もっと細かい制御が可能になる。
            ```java
                @PersistenceContext
                private EntityManager entityManager;
            ```
            - 👆 @PersistenceContext を使うと、Spring が EntityManager を自動で Inject してくれる。
        - 2️⃣ エンティティを保存（persist()）
            - データベースに新しいエンティティを保存するには persist() を使う。
            ```java
                User user = new User();
                user.setName("John Doe");
                entityManager.persist(user);
            ```
            - 👆 データベースに user を登録！（INSERT 相当）

        - 3️⃣ エンティティを取得（find()）
            - ID を指定してデータを取得する場合。
            ```java
                User user = entityManager.find(User.class, 1L);
            ```
            - 👆 ID が 1 の User を取得！（SELECT 相当）

        - 4️⃣ エンティティを更新（merge()）
            - データを更新するときは merge() を使う。
            ```java
                User user = entityManager.find(User.class, 1L);
                user.setName("Jane Doe");
                entityManager.merge(user);
            ```
            - 👆 ID=1 のユーザー名を Jane Doe に変更！（UPDATE 相当）

        - 5️⃣ エンティティを削除（remove()）
            - データを削除するときは remove() を使う。
            ```java
                User user = entityManager.find(User.class, 1L);
                entityManager.remove(user);
            ```
            👆 ID=1 の User を削除！（DELETE 相当）


- Command Line RUnner
    - SPringBootフレームワークのもの
    - spring Beanがロードされた後に実行されます。
    ```java
    package com.luv2code.cruddemo;

    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.annotation.Bean;

    @SpringBootApplication
    public class CruddemoApplication {

        public static void main(String[] args) {
            SpringApplication.run(CruddemoApplication.class, args);
        }

        @Bean
        public CommandLineRunner commandLineRunner(String[] args){
            return runner ->{
                System.out.println("Hello World"); // カスタムコード
            };
        }
    }

    ```
- DBと接続したspring projectを作ってゆく
    - application.propertiesファイルに以下のように記載し、アプリを起動すると
    ```txt
    spring.datasource.url=jdbc:mysql://localhost:3306/student_tracker
    spring.datasource.username=springstudent
    spring.datasource.password=springstudent
    ```

    - 以下のようなログが出て接続成功する
    ```txt
    .   ____          _            __ _ _
    /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
    \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
    '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/

    :: Spring Boot ::                (v3.4.2)

    2025-02-21T00:41:02.243+09:00  INFO 2507 --- [           main] c.luv2code.cruddemo.CruddemoApplication  : Starting CruddemoApplication using Java 23.0.2 with PID 2507 (/Users/k_tanaka/other_learn/spring/dev-spring-boot/03-spring-boot-hibernate-jpa-crud/01-cruddemo-student/target/classes started by k_tanaka in /Users/k_tanaka/other_learn/spring/dev-spring-boot/03-spring-boot-hibernate-jpa-crud/01-cruddemo-student)
    2025-02-21T00:41:02.243+09:00  INFO 2507 --- [           main] c.luv2code.cruddemo.CruddemoApplication  : No active profile set, falling back to 1 default profile: "default"
    2025-02-21T00:41:02.429+09:00  INFO 2507 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
    2025-02-21T00:41:02.437+09:00  INFO 2507 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 4 ms. Found 0 JPA repository interfaces.
    2025-02-21T00:41:02.557+09:00  INFO 2507 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
    2025-02-21T00:41:02.743+09:00  INFO 2507 --- [           main] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.mysql.cj.jdbc.ConnectionImpl@315f09ef
    2025-02-21T00:41:02.744+09:00  INFO 2507 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
    2025-02-21T00:41:02.770+09:00  INFO 2507 --- [           main] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
    2025-02-21T00:41:02.793+09:00  INFO 2507 --- [           main] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 6.6.5.Final
    2025-02-21T00:41:02.806+09:00  INFO 2507 --- [           main] o.h.c.internal.RegionFactoryInitiator    : HHH000026: Second-level cache disabled
    2025-02-21T00:41:02.910+09:00  INFO 2507 --- [           main] o.s.o.j.p.SpringPersistenceUnitInfo      : No LoadTimeWeaver setup: ignoring JPA class transformer
    2025-02-21T00:41:02.993+09:00  INFO 2507 --- [           main] org.hibernate.orm.connections.pooling    : HHH10001005: Database info:
        Database JDBC URL [Connecting through datasource 'HikariDataSource (HikariPool-1)']
        Database driver: undefined/unknown
        Database version: 9.2
        Autocommit mode: undefined/unknown
        Isolation level: undefined/unknown
        Minimum pool size: undefined/unknown
        Maximum pool size: undefined/unknown
    2025-02-21T00:41:03.126+09:00  INFO 2507 --- [           main] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000489: No JTA platform available (set 'hibernate.transaction.jta.platform' to enable JTA platform integration)
    2025-02-21T00:41:03.127+09:00  INFO 2507 --- [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
    2025-02-21T00:41:03.184+09:00  INFO 2507 --- [           main] c.luv2code.cruddemo.CruddemoApplication  : Started CruddemoApplication in 1.116 seconds (process running for 1.325)
    Hello World
    2025-02-21T00:41:03.187+09:00  INFO 2507 --- [ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
    2025-02-21T00:41:03.188+09:00  INFO 2507 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
    2025-02-21T00:41:03.216+09:00  INFO 2507 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.

    Process finished with exit code 0

    ```

- JPA Development process
    - JPAを介してDBテーブルとJavaオブジェクトがマッピングされる

    ![alt text](../../image/image15.png)
    - Entity classはデータベースのテーブルにマッピングされるJavaクラスのこと
        - Must be annnoteted with @Entity
        - public またはprotectedの引数なしのコンストラクタを持たなければならない

    
    - Entityクラスを作成。以下のようになる。DBテーブルとマッピングさせる
    ```java
    package com.luv2code.cruddemo.entity;

    import jakarta.persistence.*;

    @Entity
    @Table(name="student")
    public class Student {
        //define field
        @Id
        @GeneratedValue(strategy= GenerationType.IDENTITY)
        @Column(name="id")
        private int id;

        @Column(name="first_name")
        private String firstName;

        @Column(name="last_name")
        private String lastName;

        @Column(name="email")
        private String email;

        public Student(){}

        public Student(String firstName, String lastName, String email) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.email = email;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getFirstName() {
            return firstName;
        }

        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public void setLastName(String lastName) {
            this.lastName = lastName;
        }

        public String getEmail() {
            return email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        @Override
        public String toString() {
            return "Student{" +
                    "id=" + id +
                    ", firstName='" + firstName + '\'' +
                    ", lastName='" + lastName + '\'' +
                    ", email='" + email + '\'' +
                    '}';
        }
    }

    ```

- Saving a java object with JPA 
    - データアクセスオブジェクトはデータベースとのインタフェースを担当する。データベースと通信するためのヘルパークラスのようなもの
    - Data Access Object(DAO)
    - DAOには多くのメソッドがある
        - save()
        - findById()
        - findAll()
        - findByLastNmae()
        - update()
        - delete()
        - deleteAll()
    - DAOはEntityマネージャーを利用し、DBと通信する。
    ![alt text](../../image/image16.png)

    - 低レベルの制御と柔軟性が必要な倍はEntityManagerを使う
    - 高い抽象度を求めるのであればJpaRepository。

    - @Transactional
        - 自動でJPAのコードのトランザクションを開始終了する。
        - なのでjavaで明示的にこれを行う必要がない

    - StudentDAO.java
    ```java
    package com.luv2code.cruddemo.dao;
    import com.luv2code.cruddemo.entity.Student;
    public interface StudentDAO {
        void save(Student theStudent);
    }
    ```

    - StudentDAOImpl.java
    ```java
    package com.luv2code.cruddemo.dao;

    import com.luv2code.cruddemo.entity.Student;
    import jakarta.persistence.EntityManager;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Repository;
    import org.springframework.transaction.annotation.Transactional;

    @Repository //コンポ年とスキャン、例外処理をサポート
    public class StudentDAOImpl implements StudentDAO{

        private EntityManager entityManager;

        @Autowired
        public StudentDAOImpl(EntityManager entityManager){//コンストラクタ
            this.entityManager = entityManager;
        }

        @Override
        @Transactional //トランザクション管理を適切にしてくれる
        public void save(Student theStudent) {// DAOInterfaceのメソッドをオーバーライドする
            entityManager.persist(theStudent); // DBに生徒が保存される
        }
    }

    ```

    - CruddemoApplication.java
        - javaのオブジェクト作って、DBにセーブするっていう。Djangoでもおんなじことしたよね！
    ```java
    package com.luv2code.cruddemo;

    import com.luv2code.cruddemo.dao.StudentDAO;
    import com.luv2code.cruddemo.entity.Student;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.annotation.Bean;

    @SpringBootApplication
    public class CruddemoApplication {

        public static void main(String[] args) {
            SpringApplication.run(CruddemoApplication.class, args);
        }

        @Bean
        public CommandLineRunner commandLineRunner(StudentDAO studentDAO){
            return runner ->{
                createStudent(studentDAO);
            };
        }

        private void createStudent(StudentDAO studentDAO) {

            System.out.println("creating a new student object");
            Student tempStudent = new Student("tanaka","kenya","kenya6111@gmail.com");

            System.out.println("saving the student");
            studentDAO.save(tempStudent);

            System.out.println("saved student. generated id: "+ tempStudent.getId());
        }

    }

    ```

    - 上記ソースでDBにレコードが１行追加されるって感じ
    - Entityアノテーション
        - データベースのテーブルに対応するクラス
        - Spring JPA がこのクラスをもとに DB 操作をする
        - ポイント
            - @Entity → このクラスが データベースのテーブルに対応することを JPA に伝える
            - @Id @GeneratedValue → id は 自動で増える主キー
            - @Table(name = "student") → DBのテーブル名を明示
            - 🔹 英単語の意味
            - Entity = 実体・存在するもの
            - 「データベースに存在するもの」という意味で使われる
            - Java では、DB に対応するオブジェクトを @Entity でマークする
            - データベースの世界では「データの単位（レコード）」を指す

    - DAO(data access object)
        - エンティティ（Student）とデータベースのやり取りを担当
        - entityManagerを使ってDbへの保存や検索、更新削除などなど行う
        - Djangoでいう、ｍodel.save()やobjects.get()に相当
    
    - StudentDAOImpl
        - Spring が データアクセス層（DAO）として管理する

    - Repository
        - 🔹 英単語の意味
        - Repository = 倉庫・保管庫・情報の蓄積場所
        - データを保存する場所（データベース）とのやりとりをする部分
        - 🔹 由来
        - 英語の Repository は「倉庫」「情報の保管庫」
        - データを「出し入れする場所」なので、DB とのやりとりを担当
        - 「データの出入り口」と考えると覚えやすい！
        - 🔹 イメージ
        🏢 データベースが「倉庫」で、Repository は「倉庫管理人」
    
    - Beanアノテーション
    Bean（ビーン）
        🔹 英単語の意味
        Bean = JavaBeans（設定情報を持つオブジェクト）
        Spring では「Spring に管理されるオブジェクト」のことを指す

    - Service（サービス）
        🔹 英単語の意味
        Service = サービス、業務、処理をまとめるもの
        「ビジネスロジックをまとめるクラス」
        現実世界でも「サービス業」は色んな処理をまとめて提供
        アプリでも「データの加工・変換」をまとめる
        Service 層を作ると Controller と DAO を分離できる

    - 🚀 覚えやすくするコツ
        Entity は「データの実体（実在するもの）」
        Repository は「データの倉庫（倉庫管理人）」
        DAO は「データベースとの接続プラグ」
        Bean は「Spring に管理される豆」
        Service は「データ処理をまとめる業務」
    
    - Spring では インターフェース を指定してインジェクトするのが基本
    - CruddemoApplication.java では、以下のように StudentDAO を受け取っている：

        ```java
            @Bean
            public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
                return runner -> {
                    createStudent(studentDAO);
                };
            }
        ```
        - この StudentDAO は インターフェース（StudentDAOImpl ではない）。
        - でも、この studentDAO.save(tempStudent); は普通に動く。

    - 2️⃣ Spring が StudentDAO の実装クラス（StudentDAOImpl）を自動でインジェクト
        StudentDAOImpl.java を見ると、@Repository がついている：

        ```java
            @Repository
            public class StudentDAOImpl implements StudentDAO {
            @Repository をつけると、Spring がこのクラスを DI コンテナに登録 する
            StudentDAO の型で StudentDAOImpl のインスタンスを管理
            そのため @Autowired で StudentDAO を受け取ると、Spring は StudentDAOImpl のインスタンスを渡してくれる
            3️⃣ だから StudentDAO を使っても、実際は StudentDAOImpl のメソッドが呼ばれる
        ```
        
        ```java
            studentDAO.save(tempStudent);
        ```
        - これは、実際には：

        ```java
        StudentDAOImpl studentDAO = new StudentDAOImpl(entityManager);
        studentDAO.save(tempStudent);
        ```
        と同じ動きをする。

        - ✅ じゃあ StudentDAOImpl を直接指定すればいいのでは？
        - これは 以前の「@Qualifier を使う理由」の話と同じ。

        - もし、以下のように StudentDAOImpl を直接指定すると：

        ```java
        @Bean
        public CommandLineRunner commandLineRunner(StudentDAOImpl studentDAO) {  // ← 直接指定
            return runner -> {
                createStudent(studentDAO);
            };
        }
        ```
        - StudentDAOImpl に依存する形になり、もし StudentDAOImpl を 別の実装（例えば StudentJdbcDAOImpl）に変更したいときに影響が大きい
        - インターフェースを使うことで「どの実装を使うか？」を変更しやすくする
        - → これが 「インターフェースを使う最大のメリット」

        - 以下を実行すると、プライマリーキーのインクリメントを自由に設定できる
        ```sql
        Alter table student_tracker.student AUTO_INCREMENT=3000;
        ```

        - 以下実行することでDBテーブル内のデータというかレコードをけして、インクリメントも1にリセットできる
            truncate student_tracker.student;