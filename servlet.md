## web.xml

#### web.xml: 最小構成
```
<?xml version="1.0"?>
<web-app>
</web-app>
```

```
<web-app>
  <servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>Hello</servlet-class>
  </servlet>
  <servlet>
    <servlet-name>helloWorld</servlet-name>
    <servlet-class>servletbasic.HelloServletWorld</servlet-class>
  </servlet>
  <servlet-mapping>
      <servlet-name>hello</servlet-name>
      <url-pattern>/CallHello</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>helloWorld</servlet-name>
      <url-pattern>/CallHelloWorld</url-pattern>
  </servlet-mapping>
</web-app>
```

- servlet-class にクラスを指定する場合は、a.b.cという形式で指定する。

- SESSION timeout

  - 単位は、分

```
  <web-app>
    <servlet></servlet>
    <servlet-mapping> <servlet-mapping>

    <session-config>
      <session-timeout>
	 120
      </session-timeout>
    </session-config>
  </web-app>
```

SESSION
  HttpSession session = req.getSession();

  session.setMaxInactiveInterval(7200);
  session.getMaxInactiveInterval();

  単位は、秒

### load-on-startup

- Webアプリケーションのデプロイ時に自動的に実行
- サーブレットを<load-on-startup>要素を指定してweb.xmlに登録
- webアプリケーションロード時に、指定したサーブレットもインスタンス化され、サーブレット内のinit()メソッドが実行されます

- 定義
  - 1とか2とかの整数値を指定。
  - 複数のサーブレット指定がある場合の、ウェブアプリ起動時の初期化順序（インスタンス化してinit()が呼ばれる順番）を示す。
  - 若い方から実行される。（0が一番早いと思われるが、APサーバーによって異なる？→Tomcatの場合）
  - 同じ番号を指定すると、たぶんweb.xml内に書かれた順番で初期化される。
  - Servlet2.4では値は必須。
  - 負数を指定するかload-on-startup自体を指定しないと、起動時には初期化されない（そのサーブレットが使われる時に初めて初期化される）。
  
- sample code

- サーブレットSample50012Init.javaとSample50012.javaを定義
  - Sample50012Init.javaの定義内ではload-on-startupタグを使ってSample50012Init.javaをアプリケーションロード時に一緒にロードする

```
//
// web.xml
// 
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
        <servlet>
                <servlet-name>50012Init</servlet-name>
                <servlet-class>Sample50012Init</servlet-class>
                <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet>
                <servlet-name>50012</servlet-name>
                <servlet-class>Sample50012</servlet-class>
        </servlet>
        <servlet-mapping>
                <servlet-name>50012</servlet-name>
                <url-pattern>/50012</url-pattern>
        </servlet-mapping>
</web-app>

//
// Sample50012Init.java
//
// init()メソッドでアプリケーションスコープに
// 「キー：test」、「値：3123213123」のオブジェクトを設定
//
import javax.servlet.ServletContext;
import javax.servlet.http.HttpServlet;

public class Sample50012Init extends HttpServlet {
        public void init() {
                System.out.println("load-on-startup");
                ServletContext sc = getServletContext();
                sc.setAttribute("test", "3123213123");
        }
}

//
// Sample50012.java
//
// Sample50012Init.javaのinit()メソッドで設定したオブジェクトを
// アプリケーションスコープから取得し、画面に表示
//
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Sample50012 extends HttpServlet {

        public void doGet(HttpServletRequest request,
                        HttpServletResponse response)
        throws IOException, ServletException {

                response.setContentType("text/html; charset=Windows-31J");
                PrintWriter out = response.getWriter();

                out.println("<html>");
                out.println("<head>");
                out.println("</head>");
                out.println("<body>");

                ServletContext sc = getServletContext();
                out.println(sc.getAttribute("test"));
		
                out.println("</body>");
                out.println("</html>");

        }
}
```
