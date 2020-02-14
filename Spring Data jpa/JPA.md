# JPA

## 1. 概念

JPA其实就是对ORM思想的一个规范,ORM思想其实就是操作数据对象就相当于操作数据库

其实就是为了简化持久层的操作,SQL语言完全由ROM框架来为我们实现,避免了繁琐的操作

## 2. 基本操作

1. 创建一个实体类,跟数据库中的表一一映射,映射规则可以使用默认的映射规则,也可以使用自身声明的映射规则,自身声明的映射规则可以使用注解来实现

2. 在MATA-INF文件夹下创建文件persistence.xml文件,这个是JPA规范对应的配置文件,其中配置了JPA的具体实现，数据库的属性和对应实现的具体设置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
       <persistence-unit name="jpa1" transaction-type="RESOURCE_LOCAL">
           <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
           <properties>
               <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver" />
               <!-- 数据库地址 -->
               <property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/jpa?serverTimezone=UTC" />
               <!-- 数据库用户名 -->
               <property name="javax.persistence.jdbc.user" value="root" />
               <!-- 数据库密码 -->
               <property name="javax.persistence.jdbc.password" value="1234567890" />
               <property name="hibernate.show_sql" value="true" />
               <property name="hibernate.format_sql" value="true" />
               <property name="hibernate.hbm2ddl.auto" value="update" />
   
           </properties>
       </persistence-unit>
   </persistence>
   
   ```

3. 创建出entityManagerFactory和entityManager,其中entityManagerFactory是实体管理器工厂,

     entityManager便是用于与数据库交互的具体的实体管理器

```java
public class JpaUtil {
    private static EntityManagerFactory entityManagerFactory;
    static{
        entityManagerFactory = Persistence.createEntityManagerFactory("jpa1");
    }

    public static  EntityManager getEntityManager(){
        return entityManagerFactory.createEntityManager();
    }
}
```



4. JPA提供的根据主键值操作的接口

   ```java
    public void test01(){
           EntityManagerFactory entityManagerFactory = 	Persistence.createEntityManagerFactory("jpa1");
           EntityManager entityManager = entityManagerFactory.createEntityManager();
           EntityTransaction transaction = entityManager.getTransaction();
           transaction.begin();
          
        	//存储操作
        	Customer c = new Customer();
           c.setCustName("hello");
           entityManager.persist(c);
         
        	//更新操作
           Customer customer = entityManager.find(Customer.class, 2L);
           customer.setCustName("黑马");
           entityManager.merge(customer);
         
           //查找和删除操作
      	    Customer customer = entityManager.find(Customer.class, 1L);
           customer.setCustName("黑马");
           entityManager.remove(customer);
        
        
        	transaction.commit();
           entityManager.close();
           entityManagerFactory.close();
       }
   ```

   ```
   entityManager.find();  //马上发送sql语句
   entityManager.getReference(); //在使用所需要的数据发送sql语句
   ```

5. JPA提供的复杂的操作

```java
  public void test01(){
        EntityManager entityManager = JpaUtil.getEntityManager();
        EntityTransaction entityTransaction =  entityManager.getTransaction();
        entityTransaction.begin();
      
       //查询全部
        String jpql= "from  Customer";
        Query query = entityManager.createQuery(jpql);
        List list = query.getResultList();
      
      //分页查询
      String jpql= "from  Customer";
        Query query = entityManager.createQuery(jpql);
        query.setFirstResult(0);
        query.setMaxResults(2);
        List list = query.getResultList();
        
      
      //条件查询
       String jpql= "from  Customer where custName = ?";
        Query query = entityManager.createQuery(jpql);
        query.setParameter(1,"黑马");
        List list = query.getResultList();
       
      //对结果进行排序
       String jpql= "from  Customer order by custId desc";
        Query query = entityManager.createQuery(jpql);
        List list = query.getResultList();
       
      //统计查询
        String jpql= "select count(*) from Customer";
        Query query = entityManager.createQuery(jpql);
        Object o = query.getSingleResult();
      
        entityTransaction.commit();
    }
```

