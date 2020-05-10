### 操作

##### 生成JPAEntity

自定义 POJOs.groovy 文件

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
3. 普通业务实体，编写变量后可以通过IDEA工具自动生成 setter/getter等
4. 普通业务实体，也可以使用 Lombok 省去 setter/getter。通过@data和@setter/@getter注解





### 坑洞

##### JPA 

###### IllegalArgumentException: Not a managed type

1. 实体类没有添加@Entity
2. 实体中@Entity、@Table、@Id引入类型错误
3. 没有默认按照springboot的默认扫描方式，默认扫描（application.java入口类的相对的兄弟包和及其子包）

