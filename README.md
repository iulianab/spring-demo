# Spring-demo

### Checkout Spring project

1. In Intellij Go to File -> New -> New Project from Version Control...

2. Type the url
https://github.com/CegekaJavaAcademy2021/sql-jdbc.git

3. Choose your desired workspace and select Clone.

These steps will download the project into spring folder in your selected workspace.

### Compile with Intellij and Maven

1. In Intellij click on Terminal Tab in the footer section of the IDE.


2. Run a clean install on the maven project
```
mvn clean install
```

3. Checkout a new branch
```
git checkout -b nume-prenume-spring
```

### 1. Java Basic Classes
#### 1.1 Under academy package create a new package named basicjava
#### 1.2 Under basicjava package create new Java Classes

##### Address
```bash
public class Address {
    private String city;
    private String street;

    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }

    public String formatAddress() {
        return "street: " + street + ", city: " + city;
    }
}
```

##### Product
```bash
public class Product {
    private String name;

    public Product(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

##### Order
```bash
public class Order {
    private Integer id;
    private List<Product> products;
    private Address address;

    public Order(Integer id, List<Product> products, Address address) {
        this.id = id;
        this.products = products;
        this.address = address;
    }

    public Integer getId() {
        return id;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String getProductStringList() {
        return products.stream().map(Product::getName)
                .collect(Collectors.joining(", "));
    }
}
```

##### CreditCard
```bash
public class CreditCard {
    private String cardNo;

    public CreditCard(String cardNo) {
        this.cardNo = cardNo;
    }
}
```

##### Customer
```bash
public class Customer {
    private List<Order> orders;
    private List<CreditCard> creditCards;

    public Customer(List<Order> orders, List<CreditCard> creditCards) {
        this.orders = orders;
        this.creditCards = creditCards;
    }

    public void printOrdersDetails() {
        orders.stream().forEach(
                order -> System.out.println("Order with id " + order.getId() + " and products: "
                        + order.getProductStringList() + " was delivered to address "
                        + order.getAddress().formatAddress())
        );
    }
}
```
#### 1.3 Under basicjava package create a class with a main method and test printOrderDetails() method of an Customer instance

```bash
public static void main(String[] args) {
    CreditCard creditCard = new CreditCard(UUID.randomUUID().toString());
    Address address = new Address("Bucharest", "Lipscani");
    Product product = new Product("bread");
    Order order = new Order(1,Collections.singletonList(product), address);
    Customer customer = new Customer(Collections.singletonList(order), Collections.singletonList(creditCard));

    customer.printOrdersDetails();
}
```

### 2. Beans
#### 2.1 Under spring package create a new package named beans

#### 2.2 Under beans package create a class named Address 
```bash
public class Address {
    private String city;
    private String street;

    public String formatAddress() {
        return "street: " + street + ", city: " + city;
    }

    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }
}
```
#### 2.3 Under spring package create a class named JavaConfig
```bash
@Configuration
public class JavaConfig {

    @Bean
    public Address address() {
        return new Address("Bucharest", "Lipscani");
    }
}
```
#### 2.4 Under beans package create a class named Product
```bash
public class Product {
    private String name;

    public Product(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
#### 2.5 Under spring package create a class named JavaProductConfig
```bash
@Configuration
public class JavaProductConfig {
    @Bean
    @Order(2)
    public Product product1() {
        return new Product("bread");
    }

    @Bean
    @Order(1)
    public Product product2() {
        return new Product("water");
    }
}
```
#### 2.6 In JavaConfigClass inject product1 and product2 beans and create a new bean of type List\<Product>
```bash
 @Autowired
 private Product product1;

 @Autowired
 private Product product2;
```
```bash
@Bean
public List<Product> productList() {
    return Arrays.asList(product1, product2);
}
```
#### 2.7 Under package beans create a class named Order
```bash
@Component
@Scope("prototype")
public class Order {
    private Integer id;
    private List<Product> products;
    private Address address;

    @Autowired
    public Order(List<Product> products, Address address) {
        this.products = products;
        this.address = address;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getProductStringList() {
        return products.stream().map(Product::getName)
                .collect(Collectors.joining(", "));
    }
}
```
#### 2.8 Under package beans create a class named CreditCard
```bash
@Component
public class CreditCard {
    private String cardNo;

    public String getCardNo() {
        return cardNo;
    }

    public void setCardNo(String cardNo) {
        this.cardNo = cardNo;
    }
}
```
#### 2.9 Under package beans create a class named Customer
```bash
@Component
public class Customer {
    private List<Order> orders = new ArrayList<>();
    private List<CreditCard> creditCards = new ArrayList<>();

    @Autowired
    public void setOrder1(Order order) {
        order.setId(1);
        orders.add(order);
    }

    @Autowired
    public void setOrder2(Order order) {
        order.setId(2);
        orders.add(order);
    }

    @Autowired
    private void setCardBrd(CreditCard creditCard) {
        creditCard.setCardNo(UUID.randomUUID().toString());
        creditCards.add(creditCard);
    }

    @Autowired
    private void setCardBcr(CreditCard creditCard) {
        creditCard.setCardNo(UUID.randomUUID().toString());
        creditCards.add(creditCard);
    }

    public void printOrdersDetails() {
        orders.stream().forEach(
                order -> System.out.println("Order with id " + order.getId() + " and products: " + order.getProductStringList() +
                        " was delivered to address " + order.getAddress().formatAddress())
        );
    }

    public void printCreditCards() {
        creditCards.forEach(
                creditCard -> System.out.println("Card no: " + creditCard.getCardNo())
        );
    }
}
```

#### 2.10 To test the beans go to Application class, implement CommandLineRunner interface, override run method, inject Customer bean and call printCreditCards() and printOrderDetails() methods
```bash
@SpringBootApplication
public class Application implements CommandLineRunner {

	@Autowired
	private Customer customer;

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Override
	public void run(String... args) throws Exception {
		customer.printCreditCards();
		customer.printOrdersDetails();
	}
}
```

### 3. Bean Life Cycle
#### 3.1 Under spring package create a new package named lifecycle
#### 3.2 Under lifecycle package create a class named WeekDay which implements InitializingBean and DisposableBean interfaces
```bash
@Component
public class WeekDay implements InitializingBean, DisposableBean {

    private String dayType = "weekend";

    public String getDayType() {
        return dayType;
    }
}
```
#### 3.3 Override mandatory methods
```bash
 @Override
 public void afterPropertiesSet() throws Exception {
    System.out.println("Verify day type");
    DayOfWeek d = LocalDateTime.now().getDayOfWeek();
    boolean verifyWeekend =  d == DayOfWeek.SATURDAY || d == DayOfWeek.SUNDAY;
    if (!verifyWeekend) {
        dayType = "workDay";
    }
}
```

```bash
 @Override
 public void destroy() throws Exception {
    System.out.println("Bean was destroyed");
}
```
#### 3.4 Create a method named afterInit() and annotate it with @PostConstruct
```bash
 @PostConstruct
 public void afterInit() {
    System.out.println("Bean was created cu dayType = " + dayType);
}
```

#### 3.5 Create a method named beforeDestroy() and annotate it with @PreDestroy
```bash
 @PreDestroy
 public void beforeDestroy() {
     System.out.println("Bean will be destroy");
}
```

#### Go to Application class, inject WeekDay bean and print dayType value in the run method. 
After running the application, you will see the life cycle of the bean
```bash
@Autowired
private WeekDay weekDay;
```
```bash
System.out.println("Current day type: " + weekDay.getDayType());
```


### 4.BeanPostProcessor
#### 4.1 Under spring package create a new package named postprocessor
#### 4.2 Under postprocessor package create a class named Animal
```bash
@Component
public class Animal {
    private String name;

    @Autowired
    public void setName() {
        this.name = "Azor";
    }

    public void presentAnimal() {
        System.out.println("Animal name is " + name);
    }
}
```
#### 4.3 Under postprocessor package create a class named CustomBeanProcessor which implements BeanPostProcessorInterface
```bash
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {

}
```
#### 4.4 Override method postProcessBeforeInitialization(Object bean, String beanName)
```bash
@Override
public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
   if (bean instanceof Animal) {
        System.out.println("Calling bean post processor before init for bean: " + beanName + " with class name: " + bean.getClass());
   }
   return bean;
}
```
#### 4.5 Override method Object postProcessAfterInitialization(Object bean, String beanName)
```bash
@Override
public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    if (bean instanceof Animal) {
        System.out.println("Calling bean post processor after init for bean: " + beanName + " with class name: " + bean.getClass());
        try {
             bean.getClass().getMethod("presentAnimal").invoke(bean);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }
    return bean;
}
```
#### 4.6 Run Application and see the results

### 5. Get values from application.properties file
#### 5.1 Under spring package create a new package named props
### Method 1 -> Using @Value annotation
#### 5.2 In application.properties file add:
```bash
database.url=jdbc:postgresql://localhost/CegekaAcademy
```
#### 5.2 Under props package create a class named DatabaseInfo
```bash
@Component
public class DatabaseInfo {
    @Value("${database.url}")
    private String databaseUrl;

    public String getDatabaseUrl() {
        return databaseUrl;
    }

    public void setDatabaseUrl(String databaseUrl) {
        this.databaseUrl = databaseUrl;
    }
}
```
#### 5.3 Go to Application class, inject DatabaseInfo bean and test getDatabaseUrl() getter in the run method
```bash
@Autowired
private DatabaseInfo databaseInfo;
```
```bash
System.out.println("Database url: " + databaseInfo.getDatabaseUrl());
```

### Method 2 -> using @ConfigurationProperties annotation
#### 5.4 In the application.properties file add:
```bash
application.details.name=Demo Spring
application.details.createdAt=10/04/2021
application.details.description=Beans Exercise
```
#### 5.5 Under props package add a class named ApplicationDetails
```bash
@Component
@ConfigurationProperties(prefix = "application.details")
public class ApplicationDetails {
    private String name;
    private String createdAt;
    private String description;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCreatedAt() {
        return createdAt;
    }

    public void setCreatedAt(String createdAt) {
        this.createdAt = createdAt;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public void describe() {
        System.out.println("Application with name: " +  name + " was created at " + createdAt + " and contains " + description);
    }
}
```
#### 5.6 Go to Application class, inject ApplicationDetails bean and test describe() method
```bash
@Autowired
private ApplicationDetails applicationDetails;
```

```bash
applicationDetails.describe();
```