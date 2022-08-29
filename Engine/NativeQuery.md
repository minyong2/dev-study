```sql
package com.util.repository;

import org.qlrm.mapper.JpaResultMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Query;
import java.util.List;

@Repository
public class ComNativeQueryRepository {

    @Autowired
    private EntityManagerFactory entityManagerFactory;

    public List<String> findByOracleInsertQuery(String tableName, String tableSchema){
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
        nativeQuery.setParameter(1, tableName);
        nativeQuery.setParameter(2, tableSchema);

        query = (List<String>)nativeQuery.getResultList();

        em.close();

        return query;
    }
}

```

