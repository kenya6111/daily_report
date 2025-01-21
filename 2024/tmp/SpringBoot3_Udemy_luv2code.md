## 1章
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