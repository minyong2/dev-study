```java
package com.util.repository;

import org.qlrm.mapper.JpaResultMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Query;
import java.util.HashMap;
import java.util.List;

@Repository
public class ComNativeQueryRepository {

    @Autowired
    private EntityManagerFactory entityManagerFactory;

    public List<String> findByPostgresTableInfoQuery(HashMap<String,String> map){
        EntityManager em;
        String sql= null;
        List<String> query = null;
        Query nativeQuery;
        JpaResultMapper jpaResultMapper = new JpaResultMapper();
        Object obj;

        em = entityManagerFactory.createEntityManager();

        sql = "SELECT 'private '||data_type||' '||COLUMN_NAME||';'\n"
                + "from(\n"
                + "SELECT replace(substring(INITCAP('z'||COLUMN_NAME),2),'_','') AS COLUMN_NAME\n"
                + ", CASE WHEN data_type like '%character%' THEN 'String'\n"
                + "WHEN (data_type like '%int%' or data_type = 'number') THEN 'int'\n"
                + "WHEN data_type = 'timestamp with time zone' THEN 'Timestamp'\n"
                + "END data_type\n"
                + ", LOWER(COLUMN_NAME) AS org_COLUMN_NAME\n"
                + "from INFORMATION_SCHEMA.COLUMNS\n"
                + "where TABLE_NAME = ?1\n"
                + "AND TABLE_SCHEMA = ?2\n"
                + ")x";

        nativeQuery = em.createNativeQuery(sql);
        nativeQuery.setParameter(1, map.get("tableName"));
        nativeQuery.setParameter(2, map.get("tableSchema"));

        query = (List<String>)nativeQuery.getResultList();

        em.close();

        return query;
    }
```
- 테이블 조회해서 DTO 만드는 NativeQuery
```java

    public List<String> findByEntityEmbeddedTableInfoQuery(HashMap<String,String> map){
        EntityManager em;
        String sql = null;
        List<String> query = null;
        Query nativeQuery;
        JpaResultMapper jpaResultMapper = new JpaResultMapper();
        Object obj;

        em = entityManagerFactory.createEntityManager();

        sql = "SELECT '\t@Column(name = \"'||ORG_COLUMN_NAME||'\") '||CHR(10)||'\tprivate '||DATA_TYPE||' '||COLUMN_NAME||';' AS DATA\n"
                + "  FROM(\n"
                + "        SELECT REPLACE(SUBSTRING(INITCAP('z'||a.COLUMN_NAME),2),'_','') AS COLUMN_NAME\n"
                + "             , CASE WHEN A.DATA_TYPE LIKE '%character%' THEN 'String'\n"
                + "                    WHEN A.DATA_TYPE LIKE '%int%' THEN 'int'\n"
                + "                    WHEN A.DATA_TYPE = 'timestamp with time zone' THEN 'Timestamp'\n"
                + "                    ELSE 'String'\n"
                + "                END DATA_TYPE\n"
                + "             , LOWER(A.COLUMN_NAME) AS ORG_COLUMN_NAME\n"
                + "             , CONSTRAINT_TYPE\n"
                + "             , A.ORDINAL_POSITION\n"
                + "          FROM INFORMATION_SCHEMA.COLUMNS A\n"
                + "          LEFT OUTER JOIN(\n"
                + "                 SELECT A.TABLE_SCHEMA, A.TABLE_NAME, COLUMN_NAME, CONSTRAINT_TYPE\n"
                + "                   FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS A\n"
                + "                   LEFT OUTER JOIN INFORMATION_SCHEMA.CONSTRAINT_COLUMN_USAGE B\n"
                + "                     ON A.TABLE_CATALOG = B.TABLE_CATALOG\n"
                + "                    AND A.TABLE_SCHEMA = B.TABLE_SCHEMA\n"
                + "                    AND A.TABLE_NAME = B.TABLE_NAME\n"
                + "                    AND A.CONSTRAINT_NAME = B.CONSTRAINT_NAME\n"
                + "                  WHERE A.TABLE_SCHEMA = ?2\n"
                + "                    AND A.TABLE_NAME = ?1\n"
                + "                    AND A.CONSTRAINT_TYPE = 'PRIMARY KEY'\n"
                + "          ) B\n"
                + "            ON A.COLUMN_NAME = B.COLUMN_NAME\n"
                + "         WHERE A.TABLE_NAME = ?1\n"
                + ") X\n"
                + " WHERE CONSTRAINT_TYPE is not NULL\n"
                + " ORDER BY ORDINAL_POSITION";

        nativeQuery = em.createNativeQuery(sql);
        nativeQuery.setParameter(1,map.get("tableName"));
        nativeQuery.setParameter(2,map.get("tableSchema"));

        query = (List<String>)nativeQuery.getResultList();
        em.close();

        return query;
    }

    public List<String> findByEntityTableInfoQuery(HashMap<String,String> map){
        EntityManager em;
        String sql;
        List<String> query = null;
        Query nq;
        JpaResultMapper jpaResultMapper = new JpaResultMapper();
        Object obj;

        em = entityManagerFactory.createEntityManager();

        sql = "\tSELECT '\t@Column(name = \"'||ORG_COLUMN_NAME||'\") '||CHR(10)||'\tprivate '||DATA_TYPE||' '||COLUMN_NAME||';' AS DATA\n" +
                "  FROM(\n" +
                "        SELECT REPLACE(SUBSTRING(INITCAP('z'||a.COLUMN_NAME),2),'_','') AS COLUMN_NAME\n" +
                "             , CASE WHEN A.DATA_TYPE LIKE '%character%' THEN 'String'\n" +
                "                    WHEN A.DATA_TYPE LIKE '%int%' THEN 'int'\n" +
                "                    WHEN A.DATA_TYPE = 'timestamp with time zone' THEN 'Timestamp'\n" +
                "                    ELSE 'String'\n" +
                "                END DATA_TYPE\n" +
                "             , LOWER(A.COLUMN_NAME) AS ORG_COLUMN_NAME\n" +
                "             , CONSTRAINT_TYPE\n" +
                "             , A.ORDINAL_POSITION\n" +
                "          FROM INFORMATION_SCHEMA.COLUMNS A\n" +
                "          LEFT OUTER JOIN(\n" +
                "                 SELECT A.TABLE_SCHEMA, A.TABLE_NAME, COLUMN_NAME, CONSTRAINT_TYPE\n" +
                "                   FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS A\n" +
                "                   LEFT OUTER JOIN INFORMATION_SCHEMA.CONSTRAINT_COLUMN_USAGE B\n" +
                "                     ON A.TABLE_CATALOG = B.TABLE_CATALOG\n" +
                "                    AND A.TABLE_SCHEMA = B.TABLE_SCHEMA\n" +
                "                    AND A.TABLE_NAME = B.TABLE_NAME\n" +
                "                    AND A.CONSTRAINT_NAME = B.CONSTRAINT_NAME\n" +
                "                  WHERE A.TABLE_SCHEMA = ?2\n" +
                "                    AND A.TABLE_NAME = ?1\n" +
                "                    AND A.CONSTRAINT_TYPE = 'PRIMARY KEY'\n" +
                "          ) B\n" +
                "            ON A.COLUMN_NAME = B.COLUMN_NAME\n" +
                "         WHERE A.TABLE_NAME = ?1\n" +
                ") X\n" +
                " WHERE CONSTRAINT_TYPE is NULL\n" +
                " ORDER BY ORDINAL_POSITION";

        nq = em.createNativeQuery(sql);
        nq.setParameter(1,map.get("tableName"));
        nq.setParameter(2,map.get("tableSchema"));

        query = nq.getResultList();
        em.close();

        return query;

    }
}
```
- 테이블 조회해서 복합키랑 Entity 생성하는 쿼리

---
```java
package com.util.code.domain.service;

import com.util.code.common.utility.LoggerPrint;
import com.util.entity.entity.key.AlarmEntityKey;
import com.util.repository.ComNativeQueryRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;


@Service("EntityService")
public class EntityService {

    @Autowired
    ComNativeQueryRepository comNativeQueryRepository;

    @Autowired
    DTOService dtoService;


    public void entityEmbeddedService(HashMap<String,String> map){

        List<String> entityEmbeddedInfo = comNativeQueryRepository.findByEntityEmbeddedTableInfoQuery(map);

        try {
            StringBuilder code = new StringBuilder();
            code.append("package "+map.get("package")+";\n");
            code.append("import lombok.Data;\n");
            code.append("import lombok.ToString;\n");
            code.append("import javax.persistence.Column;\n");
            code.append("import javax.persistence.Embeddable;\n");
            code.append("import java.io.Serializable;\n\n");
            code.append("@Data\n");
            code.append("@ToString\n");
            code.append("@Embeddable\n\n");
            code.append("public class "+map.get("fileName").split("\\.")[0]+" implements "+ "Serializable {\n");
            for (String data : entityEmbeddedInfo){
                if (map.get(data)!= null){
                    code.append("@Column(name = \""+map.get(data)+"\")\n");
                }
                code.append("\t"+data+"\n");
            }
            code.append("}");
            map.put("code",code.toString());
            dtoService.queryWrite(map);
        }catch (Exception e){
            LoggerPrint.errorLog(e);
        }
    }
```
- 복합키 클래스 생성
```java

    public void entityService(HashMap<String,String> map){
        List<String> entityInfo = comNativeQueryRepository.findByEntityTableInfoQuery(map);
        String Key = "AlarmEntityKey";
        String key = " alarmEntityKey";

        try{
            StringBuilder code = new StringBuilder();
            code.append("package "+map.get("package")+";\n");
            code.append("import com.fasterxml.jackson.annotation.JsonIgnoreProperties;\n");
            code.append("import lombok.*;\n");
            code.append("import org.springframework.data.domain.Persistable;\n");
            code.append("import java.io.Serializable;\n");
            code.append("import java.sql.Timestamp;\n");
            code.append("import com.util.entity.entity.key.AlarmEntityKey;\n");
            code.append("import javax.persistence.*;\n\n");

            code.append("@Entity\n");
            code.append("@Getter\n");
            code.append("@Setter\n\n");
            code.append("@JsonIgnoreProperties(ignoreUnknown = true)\n");
            code.append("@NoArgsConstructor(access = AccessLevel.PUBLIC)\n");
            code.append("@Table(name = \""+map.get("tableName")+"\" , schema = \""+map.get("tableSchema")+"\")\n");

            code.append("public class " +map.get("fileName").split("\\.")[0]+" implements "+ "Persistable<AlarmEntityKey>, Serializable {\n");

            code.append("@EmbeddedId\n");
            code.append("private "+map.get("fileName").split("\\.")[0]+"Key" + key+";\n");

            for(String data : entityInfo){
                if (map.get(data) != null){
                    code.append("@Column(name = \""+map.get(data)+"\")\n");
                }
                code.append("\t"+data+"\n");
            }

            code.append("\n\n@Override\n");
            code.append("public AlarmEntityKey getId() {\n");
            code.append("\treturn"+key+";\n");
            code.append("}\n");

            code.append("@Override\n");
            code.append("public boolean isNew() {\n");
            code.append("\treturn true;\n");
            code.append("\t}\n");
            code.append("}");
            map.put("code",code.toString());
            dtoService.queryWrite(map);

        }catch (Exception e){
            LoggerPrint.errorLog(e);

        }

    }
}
```
- Entity 클래스 파일 생성부

```java
package com.util.code.domain.service;

import com.util.code.common.utility.LoggerPrint;
import com.util.repository.ComNativeQueryRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.File;
import java.io.FileWriter;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

@Service("DTOService")
public class DTOService {

    @Autowired
    ComNativeQueryRepository comNativeQueryRepository;

    public void dtoService(HashMap<String,String> map){

//        String tableName = "tb_alarm_cur_mst";
//        String tableSchema = "rca";
         List<String> dto = comNativeQueryRepository.findByPostgresTableInfoQuery(map);
//        List<String> rs = new ArrayList<>();
//        HashMap<String,String> dtoInfo = new HashMap<>();

        try {
            StringBuilder code = new StringBuilder();
            code.append("package "+map.get("package")+";"+"\n");
            code.append("import lombok.Data;\n");
            code.append("import org.springframework.context.annotation.Scope;\n");
            code.append("import org.springframework.stereotype.Component;\n");
            code.append("import java.io.Serializable;\n");
            code.append("import java.sql.Timestamp;\n");
            code.append("@Component\n");
            code.append("@Scope(value = \"prototype\")\n");
            code.append("public class "+map.get("fileName").split("\\.")[0]+" implements Serializable {\n");
            for (String data : dto){
                code.append("\t"+data+"\n");
            }
            code.append("}");
            map.put("code",code.toString());
            queryWrite(map);

        }catch (Exception e){
            LoggerPrint.errorLog(e);
        }

    }

    public void queryWrite(HashMap<String,String> map) throws Exception{


        File file = new File(map.get("fileloc")+map.get("fileName"));
        FileWriter fw = new FileWriter(file);
        fw.write(map.get("code"));
        fw.close();
    }
}
```
- DTO 클래스 파일 생성부

---

```java
package com.util;

import com.ulisesbocchio.jasyptspringboot.annotation.EnableEncryptableProperties;
import com.util.code.domain.service.DTOService;
import com.util.code.domain.service.EntityService;
import com.util.repository.ComNativeQueryRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.util.HashMap;

@EnableEncryptableProperties
@SpringBootApplication
//public class AlarmFilterApplication extends SpringBootServletInitializer implements CommandLineRunner {
public class UtilCodeApplication implements CommandLineRunner {

	@Autowired
	ComNativeQueryRepository comNativeQueryRepository;

	@Autowired
	@Qualifier("DTOService")
	DTOService dtoService;

	@Autowired
	@Qualifier("EntityService")
	EntityService entityService;




	public static void main(String[] args) {
		SpringApplication.run(UtilCodeApplication.class, args);

	}


	@Override
	public void run(String... args) throws Exception {
		HashMap<String,String> map = new HashMap<>();
		map.put("tableName","tb_alarm_cur_mst_elk");
		map.put("tableSchema","rca");
		map.put("fileloc","D:\\project\\util-code\\core\\entity\\src\\main\\java\\com\\util\\entity\\entity\\");
		map.put("fileName","AlarmEntity.java");
		map.put("package","com.util.entity.entity");
//		dtoService.dtoService(map);
//		entityService.entityEmbeddedService(map);
		entityService.entityService(map);


//		comNativeQueryRepository.findByPostgresTableInfoQuery("tb_alarm_cur_mst","rca");

	}
}
```
- 실행부

---
---
---

## 생성된 파일
![image](https://user-images.githubusercontent.com/97263974/187595304-d41bbea5-c421-467b-a65d-48193992f822.png)
---
![image](https://user-images.githubusercontent.com/97263974/187595502-0ef05f14-187a-4ca5-be5f-ac3d5727392b.png)