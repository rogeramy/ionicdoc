### 操作

##### 生成JPAEntity

安装 JPA 模块，project strueture - Modules ，点击 + 添加 JPA 模块

添加 JPA 模块后，才能看到 Persistence，一般在 IDEA 左下侧栏

Database 添加数据库连接

从 Persistence 生成 Entity，Generate Persistence Mapping、By Database Schema打开Import Database Schema窗口

![image-20200513204824819](C:\ionicdoc\images\image-20200513204824819.png)





其它方式：自定义 POJOs.groovy 文件

```

import com.intellij.database.model.DasTable
import com.intellij.database.model.ObjectKind
import com.intellij.database.util.Case
import com.intellij.database.util.DasUtil
import java.io.*
import java.text.SimpleDateFormat
/*
 * Available context bindings:
 *   SELECTION   Iterable<DasObject>
 *   PROJECT     project
 *   FILES       files helper
 */

packageName = "com.om2000.om2000.entity;"
typeMapping = [
        (~/(?i)int/)                      : "long",
        (~/(?i)float|double|decimal|real/): "double",
        (~/(?i)datetime|timestamp/)       : "java.sql.Timestamp",
        (~/(?i)date/)                     : "java.sql.Date",
        (~/(?i)time/)                     : "java.sql.Time",
        (~/(?i)/)                         : "String"
]

FILES.chooseDirectoryAndSave("Choose directory", "Choose where to store generated files") { dir ->
    SELECTION.filter { it instanceof DasTable }.each { generate(it, dir) }
}

def generate(table, dir) {
    def className = javaName(table.getName(), true)
    //取上面设定的包名
//    packageName = getPackageName(dir)
    def fields = calcFields(table)
    new File(dir, className + ".java").withPrintWriter { out -> generate(out, className, fields, table) }
}
// 获取包所在文件夹路径
def getPackageName(dir) {
    return dir.toString().replaceAll("\\\\", ".").replaceAll("/", ".").replaceAll("^.*src(\\.main\\.java\\.)?", "") + ";"
}

def generate(out, className, fields, table) {
    out.println "package $packageName"

    out.println ""
//    out.println "import lombok.Data;"
    out.println "import javax.persistence.Column;"
    out.println "import javax.persistence.Entity;"
    out.println "import javax.persistence.Table;"
    out.println "import java.io.Serializable;"
//    out.println "import javax.persistence.Id;"
    out.println ""
    out.println "/**\n" +
            " * @Description  \n" +
            " * @Author IDEA Auto JPA database view generate entity Script Mxnter\n" +
            " * @Date "+ new SimpleDateFormat("yyyy-MM-dd").format(new Date()) + " \n" +
            " */"
    out.println ""
    out.println ""

    out.println "@Entity"
    out.println "@Table ( name =\""+table.getName() +"\" )"
//    暂时没有使用 @Data
//    out.println "@Data"
    out.println "public class $className implements Serializable {"
    out.println ""
    out.println ""
    out.println genSerialID()
    fields.each() {
        out.println "\t"
        if (isNotEmpty(it.commoent)) {
            out.println "\t/**"
            out.println "\t * ${it.commoent.toString()}"
            out.println "\t */"
        }

//    if (it.annos != "") out.println "   ${it.annos.replace("[@Id]", "")}"
        out.println "${it.annos}\t"
        out.println "\tprivate ${it.type} ${it.name};"
    }

    // 输出get/set方法
    fields.each() {
        out.println ""
        out.println "\tpublic ${it.type} get${it.name.capitalize()}() {"
        out.println "\t\treturn this.${it.name};"
        out.println "\t}"
        out.println ""

        out.println "\tpublic void set${it.name.capitalize()}(${it.type} ${it.name}) {"
        out.println "\t\tthis.${it.name} = ${it.name};"
        out.println "\t}"
    }

    out.println ""
    out.println "}"
}

def calcFields(table) {
    DasUtil.getColumns(table).reduce([]) { fields, col ->
        def spec = Case.LOWER.apply(col.getDataType().getSpecification())
        def typeStr = typeMapping.find { p, t -> p.matcher(spec).find() }.value
        fields += [[
                           colName : col.getName(),
                           name : javaName(col.getName(), false),
                           type : typeStr,
                           commoent: col.getComment(),
                           annos: ("UUID".equals(col.getName())?"\t@Id\n":"")+"\t@Column(name = \""+col.getName()+"\" )"]]
    }
}

def javaName(str, capitalize) {
    def s = com.intellij.psi.codeStyle.NameUtil.splitNameIntoWords(str)
            .collect { Case.LOWER.apply(it).capitalize() }
            .join("")
            .replaceAll(/[^\p{javaJavaIdentifierPart}[_]]/, "_")
    capitalize || s.length() == 1? s : Case.LOWER.apply(s[0]) + s[1..-1]
}

def isNotEmpty(content) {
    return content != null && content.toString().trim().length() > 0
}

static String genSerialID()
{
    return "\tprivate static final long serialVersionUID =  "+Math.abs(new Random().nextLong())+"L;"
}

```



##### 实体类操作

1. JPA 对应的实体，通过 IDEA Groovy模板生成

2. JPA 需要添加 @Entity注解并设置 @Id

3. 普通业务实体，编写变量后可以通过IDEA工具自动生成 setter/getter等，在字段 右键 -generate

4. 普通业务实体，也可以使用 Lombok 省去 setter/getter。通过@data和@setter/@getter注解

5. @Column 字段命名 默认驼峰转换，会根据转换后的驼峰字段格式去数据库找对应字段，禁用该策略在application.properties文件中加入:

   ```
   
   #PhysicalNamingStrategyStandardImpl
   spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
   ```

   

6. wait



##### spring-boot-starter-data-rest 使用

可以不编写Controller，JPA自动内置 Restful，通过IP地址/EntityName 获取，添加以下依赖，例如：http://127.0.0.1:8080/worktitleEntities

```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>
```

但权限怎么管理？

通常不建议使用



##### 4.生成 spring boot bcrptPassword

[B cript 地址](https://bcrypt-generator.com/)



##### 5.JWT Authentication

[doc]: https://dzone.com/articles/implementing-jwt-authentication-on-spring-boot-api

[样例](https://github.com/echisan/springboot-jwt-demo/blob/master/blog_content.md)

##### 6.文档在线浏览方案

[online](https://gitee.com/kekingcn/file-online-preview)



### 坑洞

##### JPA 

###### 1.IllegalArgumentException: Not a managed type

1. 实体类没有添加@Entity
2. 实体中@Entity、@Table、@Id引入类型错误
3. 没有默认按照springboot的默认扫描方式，默认扫描（application.java入口类的相对的兄弟包和及其子包）

###### 2.@Qualifier共用出现提示信息Cannot find bean with

在intellij idea file-settings-editor-Inspections-spring 把右边的Mixed 改为warning



###### 3.HikariConnectionPool - Failed to validate connection

在 application.properties 添加如下：

```
spring.datasource.testOnBorrow=true
spring.datasource.validationQuery=SELECT 1
```



###### 4.spring boot restful url 默认区分大小写



##### 文件上传

###### 1.Required request part 'file' is not present

后台参数名称与前端上传空间的name必须一直，一般默认都是"file"。@RequestParam("file")

