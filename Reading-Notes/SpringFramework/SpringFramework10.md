<h2>《Spring 2.5开发手册》 :books: </h2> 

```java
48.利用 JDBC 核心类控制 JDBC 的基本操作和错误处理。
  (1) JdbcTemplate 类:
  int countOfActorsNamedJoe = this.jdbcTemplate.queryForInt(
       "select count(0) from t_actors where first_name = ?", new Object[]{"Joe"});

  public Collection findAllActors() {
      return this.jdbcTemplate.query( "select first_name, surname from t_actor", new ActorMapper());
  }
  private static final class ActorMapper implements RowMapper {
      public Object mapRow(ResultSet rs, int rowNum) throws SQLException {
         Actor actor = new Actor();
         actor.setFirstName(rs.getString("first_name"));
         actor.setSurname(rs.getString("surname"));
         return actor;
      }
  }

  this.jdbcTemplate.update("delete from actor where id = ?", new Object[] {new Long.valueOf(actorId)});

  this.jdbcTemplate.execute("create table mytable (id integer, name varchar(100))");

  public class JdbcCorporateEventDao implements CorporateEventDao {
     private JdbcTemplate jdbcTemplate;
     public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
     }
  }

  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
    <bean id="corporateEventDao" class="com.example.JdbcCorporateEventDao">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!-- the DataSource (parameterized for configuration via a PropertyPlaceHolderConfigurer) -->
    <bean id="dataSource" destroy-method="close" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
  </beans>

  (2) NamedParameterJdbcTemplate 类:
   private NamedParameterJdbcTemplate namedParameterJdbcTemplate;
   public void setDataSource(DataSource dataSource) {
      this.namedParameterJdbcTemplate = new NamedParameterJdbcTemplate(dataSource);
   }
   public int countOfActorsByFirstName(String firstName) {
      String sql = "select count(0) from T_ACTOR where first_name = :first_name";
      SqlParameterSource namedParameters = new MapSqlParameterSource("first_name", firstName);
      return namedParameterJdbcTemplate.queryForInt(sql, namedParameters);
   }

  (3) SimpleJdbcTemplate 类:
   private SimpleJdbcTemplate simpleJdbcTemplate;
   public void setDataSource(DataSource dataSource) {
      this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
   }
   public Actor findActor(long id) {
      String sql = "select id, first_name, last_name from T_ACTOR where id = ?";
      ParameterizedRowMapper<Actor> mapper = new ParameterizedRowMapper<Actor>() {
         // notice the return type with respect to Java 5 covariant return types
         public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
            Actor actor = new Actor();
            actor.setId(rs.getLong("id"));
            actor.setFirstName(rs.getString("first_name"));
            actor.setLastName(rs.getString("last_name"));
            return actor;
         }
      };
      return this.simpleJdbcTemplate.queryForObject(sql, mapper, id);
   }
   
  (4) DataSource 接口:
   DriverManagerDataSource dataSource = new DriverManagerDataSource();
   dataSource.setDriverClassName("org.hsqldb.jdbcDriver");
   dataSource.setUrl("jdbc:hsqldb:hsql://localhost:");
   dataSource.setUsername("sa");
   dataSource.setPassword("");

  (5) SQLExceptionTranslator 接口:
   public class MySQLErrorCodesTranslator extends SQLErrorCodeSQLExceptionTranslator {
      protected DataAccessException customTranslate(String task, String sql, SQLException sqlex) {
         if (sqlex.getErrorCode() == -12345) {
            return new DeadlockLoserDataAccessException(task, sqlex);
         }
         return null;
      }
   }
   
  (6) 执行 SQL 语句:
   import javax.sql.DataSource;
   import org.springframework.jdbc.core.JdbcTemplate;
   public class ExecuteStatement {
      private JdbcTemplate jdbcTemplate;
      public void setDataSource(DataSource dataSource) {
         this.jdbcTemplate = new JdbcTemplate(dataSource);
      }
      public void doExecute() {
         this.jdbcTemplate.execute("create table mytable (id integer, name varchar(100))");
      }
   }

  (7) 执行查询:
   import javax.sql.DataSource;
   import org.springframework.jdbc.core.JdbcTemplate;
   public class RunQuery {
      private JdbcTemplate jdbcTemplate;
      public void setDataSource(DataSource dataSource) {
         this.jdbcTemplate = new JdbcTemplate(dataSource);
      }
      public int getCount() {
         return this.jdbcTemplate.queryForInt("select count(*) from mytable");
      }
      public String getName() {
         return (String) this.jdbcTemplate.queryForObject("select name from mytable", String.class);
      }
      public void setDataSource(DataSource dataSource) {
         this.dataSource = dataSource;
      }
   }

  (8) 更新数据库:
   import javax.sql.DataSource;
   import org.springframework.jdbc.core.JdbcTemplate;
   public class ExecuteUpdate {
      private JdbcTemplate jdbcTemplate;
      public void setDataSource(DataSource dataSource) {
         this.jdbcTemplate = new JdbcTemplate(dataSource);
      }
      public void setName(int id, String name) {
         this.jdbcTemplate.update("update mytable set name = ? where id = ?", 
              new Object[] {name, new Integer(id)});
      }
   }

  (9) 获取自动生成的主键:
   final String INSERT_SQL = "insert into my_test (name) values(?)";
   final String name = "Rob";
   KeyHolder keyHolder = new GeneratedKeyHolder();
   jdbcTemplate.update(
      new PreparedStatementCreator() {
         public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
            PreparedStatement ps = connection.prepareStatement(INSERT_SQL, new String[] {"id"});
            ps.setString(1, name);
            return ps;
         }
      },
      keyHolder);

49.控制数据库连接。
  (1) DataSourceUtils 类: 提供支持线程绑定的数据库连接。
  (2) SmartDataSource 接口: DataSource 接口的一个扩展，用来提供数据库连接。
  (3) AbstractDataSource 类: AbstractDataSource 是一个实现了 DataSource 接口的 abstract 基类。
  (4) SingleConnectionDataSource 类: SmartDataSource 接口的一个实现，其内部包装了一个单连接。
  (5) DriverManagerDataSource 类: 实现了 SmartDataSource接口。
可以使用 bean properties 来设置 JDBC Driver 属性，该类每次返回的都是一个新的连接。 
  (6) TransactionAwareDataSourceProxy 类: 目标 DataSource 的一个代理， 
在对目标 DataSource 包装的同时，还增加了 Spring 的事务管理能力。
  (7) DataSourceTransactionManager 类: PlatformTransactionManager 接口的一个实现，用于处理单 JDBC 数据源。
  (8) NativeJdbcExtractor: SimpleNativeJdbcExtractor、C3P0NativeJdbcExtractor、
CommonsDbcpNativeJdbcExtractor、JBossNativeJdbcExtractor、WebLogicNativeJdbcExtractor、
WebSphereNativeJdbcExtractor、XAPoolNativeJdbcExtractor。

50.JDBC 批量操作。
  (1) 使用 JdbcTemplate 进行批量操作:
   public class JdbcActorDao implements ActorDao {
     private JdbcTemplate jdbcTemplate;
     public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
     }
     public int[] batchUpdate(final List actors) {
        int[] updateCounts = jdbcTemplate.batchUpdate(
              "update t_actor set first_name = ?, last_name = ? where id = ?",
               new BatchPreparedStatementSetter() {
                   public void setValues(PreparedStatement ps, int i) throws SQLException {
                       ps.setString(1, ((Actor)actors.get(i)).getFirstName());
                       ps.setString(2, ((Actor)actors.get(i)).getLastName());
                       ps.setLong(3, ((Actor)actors.get(i)).getId().longValue());
                   }
                   public int getBatchSize() {
                       return actors.size();
                   }
               } );
         return updateCounts;
     }
   }
  (2) 使用 SimpleJdbcTemplate 进行批量操作:
   public class JdbcActorDao implements ActorDao {
      private SimpleJdbcTemplate simpleJdbcTemplate;
      public void setDataSource(DataSource dataSource) {
         this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
      }
      public int[] batchUpdate(final List<Actor> actors) {
         SqlParameterSource[] batch = SqlParameterSourceUtils.createBatch(actors.toArray());
         int[] updateCounts = simpleJdbcTemplate.batchUpdate(
               "update t_actor set first_name = :firstName, last_name = :lastName where id = :id", batch);
         return updateCounts;
      }
   }
   
51.通过使用 SimpleJdbc 类简化 JDBC 操作。
  (1) 使用 SimpleJdbcInsert 插入数据:
   public class JdbcActorDao implements ActorDao {
      private SimpleJdbcTemplate simpleJdbcTemplate;
      private SimpleJdbcInsert insertActor;
      public void setDataSource(DataSource dataSource) {
         this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
         this.insertActor = new SimpleJdbcInsert(dataSource).withTableName("t_actor");
      }
      public void add(Actor actor) {
         Map<String, Object> parameters = new HashMap<String, Object>(3);
         parameters.put("id", actor.getId());
         parameters.put("first_name", actor.getFirstName());
         parameters.put("last_name", actor.getLastName());
         insertActor.execute(parameters);
      }
   }
   
  (2) 使用 SimpleJdbcInsert 来获取自动生成的主键:
   public class JdbcActorDao implements ActorDao {
      private SimpleJdbcTemplate simpleJdbcTemplate;
      private SimpleJdbcInsert insertActor;
      public void setDataSource(DataSource dataSource) {
         this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
         this.insertActor = new SimpleJdbcInsert(dataSource).withTableName("t_actor")
                       .usingGeneratedKeyColumns("id");
      }
      public void add(Actor actor) {
         Map<String, Object> parameters = new HashMap<String, Object>(2);
         parameters.put("first_name", actor.getFirstName());
         parameters.put("last_name", actor.getLastName());
         Number newId = insertActor.executeAndReturnKey(parameters);
         actor.setId(newId.longValue());
      }
   }

  (3) 指定 SimpleJdbcInsert 所使用的字段:
   public class JdbcActorDao implements ActorDao {
     private SimpleJdbcTemplate simpleJdbcTemplate;
     private SimpleJdbcInsert insertActor;
     public void setDataSource(DataSource dataSource) {
        this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
        this.insertActor = new SimpleJdbcInsert(dataSource).withTableName("t_actor")
                       .usingColumns("first_name", "last_name").usingGeneratedKeyColumns("id");
     }
     public void add(Actor actor) {
        Map<String, Object> parameters = new HashMap<String, Object>(2);
        parameters.put("first_name", actor.getFirstName());
        parameters.put("last_name", actor.getLastName());
        Number newId = insertActor.executeAndReturnKey(parameters);
        actor.setId(newId.longValue());
     }
   }

  (4) 使用 SqlParameterSource 提供参数值:
    // 前面的方法与 (3) 相同...
    public void add(Actor actor) {
       SqlParameterSource parameters = new BeanPropertySqlParameterSource(actor);
       Number newId = insertActor.executeAndReturnKey(parameters);
       actor.setId(newId.longValue());
    }

    public void add(Actor actor) {
        SqlParameterSource parameters = new MapSqlParameterSource()
                .addValue("first_name", actor.getFirstName())
                .addValue("last_name", actor.getLastName());
        Number newId = insertActor.executeAndReturnKey(parameters);
        actor.setId(newId.longValue());
    }

  (5) 使用 SimpleJdbcCall 调用存储过程:
    public class JdbcActorDao implements ActorDao {
       private SimpleJdbcTemplate simpleJdbcTemplate;
       private SimpleJdbcCall procReadActor;
       public void setDataSource(DataSource dataSource) {
          this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
          this.procReadActor = new SimpleJdbcCall(dataSource).withProcedureName("read_actor");
       }
       public Actor readActor(Long id) {
          SqlParameterSource in = new MapSqlParameterSource().addValue("in_id", id); 
          Map out = procReadActor.execute(in);
          Actor actor = new Actor();
          actor.setId(id);
          actor.setFirstName((String) out.get("out_first_name"));
          actor.setLastName((String) out.get("out_last_name"));
          actor.setBirthDate((Date) out.get("out_birth_date"));
          return actor;
       }
    }
    
   (6) 声明 SimpleJdbcCall 使用的参数:
    public class JdbcActorDao implements ActorDao {
       private SimpleJdbcCall procReadActor;
       public void setDataSource(DataSource dataSource) {
          JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
          jdbcTemplate.setResultsMapCaseInsensitive(true);
          this.procReadActor =
               new SimpleJdbcCall(jdbcTemplate)
                       .withProcedureName("read_actor")
                       .withoutProcedureColumnMetaDataAccess()
                       .useInParameterNames("in_id")
                       .declareParameters(
                               new SqlParameter("in_id", Types.NUMERIC),
                               new SqlOutParameter("out_first_name", Types.VARCHAR),
                               new SqlOutParameter("out_last_name", Types.VARCHAR),
                               new SqlOutParameter("out_birth_date", Types.DATE)
                       );
       }
    }

   (7) 使用 SimpleJdbcCall 调用内置函数:
    public class JdbcActorDao implements ActorDao {
       private SimpleJdbcTemplate simpleJdbcTemplate;
       private SimpleJdbcCall funcGetActorName;
       public void setDataSource(DataSource dataSource) {
          this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
          JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
          jdbcTemplate.setResultsMapCaseInsensitive(true);
          this.funcGetActorName = new SimpleJdbcCall(jdbcTemplate)
                       .withFunctionName("get_actor_name");
       }
       public String getActorName(Long id) {
          SqlParameterSource in = new MapSqlParameterSource().addValue("in_id", id); 
          String name = funcGetActorName.executeFunction(String.class, in);
          return name;
       }
    }

  (8) 使用 SimpleJdbcCall 返回的 ResultSet/REF Cursor:
    public class JdbcActorDao implements ActorDao {
       private SimpleJdbcTemplate simpleJdbcTemplate;
       private SimpleJdbcCall procReadAllActors;
       public void setDataSource(DataSource dataSource) {
          this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);
          JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
          jdbcTemplate.setResultsMapCaseInsensitive(true);
          this.procReadAllActors = new SimpleJdbcCall(jdbcTemplate)
                      .withProcedureName("read_all_actors")
                      .returningResultSet("actors", ParameterizedBeanPropertyRowMapper.newInstance(Actor.class));
       }
       public List getActorsList() {
          Map m = procReadAllActors.execute(new HashMap<String, Object>(0));
          return (List) m.get("actors");
       }
   }
  
52.用 Java 对象来表达 JDBC 操作。
  (1) SqlQuery 类: 可重用、线程安全的类，它封装了一个 SQL 查询。
其子类必须实现 newResultReader() 方法，该方法用来在遍历 ResultSet 的时候能使用一个类来保存结果。 
  (2) MappingSqlQuery 类: 可重用的查询抽象类，其具体类必须实现 mapRow(ResultSet, int) 抽象方法
来将结果集中的每一行转换成 Java 对象。
   private class CustomerMappingQuery extends MappingSqlQuery {
      public CustomerMappingQuery(DataSource ds) {
         super(ds, "SELECT id, name FROM customer WHERE id = ?");
         super.declareParameter(new SqlParameter("id", Types.INTEGER));
         compile();
      }
      public Object mapRow(ResultSet rs, int rowNumber) throws SQLException {
         Customer cust = new Customer();
         cust.setId((Integer) rs.getObject("id"));
         cust.setName(rs.getString("name"));
         return cust;
      } 
  }
 (3) SqlUpdate 类: 封装了一个可重复使用的 SQL 更新操作。
  import java.sql.Types;
  import javax.sql.DataSource;
  import org.springframework.jdbc.core.SqlParameter;
  import org.springframework.jdbc.object.SqlUpdate;
  public class UpdateCreditRating extends SqlUpdate {
     public UpdateCreditRating(DataSource ds) {
        setDataSource(ds);
        setSql("update customer set credit_rating = ? where id = ?");
        declareParameter(new SqlParameter(Types.NUMERIC));
        declareParameter(new SqlParameter(Types.NUMERIC));
        compile();
     }
     public int run(int id, int rating) {
        Object[] params = new Object[] { new Integer(rating), new Integer(id) };
        return update(params);
     }
  }
 (4) StoredProcedure 类: 抽象基类，它是对 RDBMS 存储过程的一种抽象。
  import oracle.jdbc.driver.OracleTypes;
  import org.springframework.jdbc.core.SqlOutParameter;
  import org.springframework.jdbc.object.StoredProcedure;
  import javax.sql.DataSource;
  import java.util.HashMap;
  import java.util.Map;
  public class TitlesAndGenresStoredProcedure extends StoredProcedure {
     private static final String SPROC_NAME = "AllTitlesAndGenres";
     public TitlesAndGenresStoredProcedure(DataSource dataSource) {
        super(dataSource, SPROC_NAME);
        declareParameter(new SqlOutParameter("titles", OracleTypes.CURSOR, new TitleMapper()));
        declareParameter(new SqlOutParameter("genres", OracleTypes.CURSOR, new GenreMapper()));
        compile();
     }
     public Map execute() {
       return super.execute(new HashMap());
     }
  }
 (5) SqlFunction 类: RDBMS 操作类封装了一个 SQL“ 函数”包装器(wrapper), 该包装器适用于查询并返回一个单行结果集。
  public int countRows() {
     SqlFunction sf = new SqlFunction(dataSource, "select count(*) from mytable");
     sf.compile();
     return sf.run();
  }
```