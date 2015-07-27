---
title: The first test
layout: default
titleImage: /assets/earth-mass.png
---

So I wrote the first test… As I want to access the app using jmx later i used spring to setup my unit tests. meaning 
i wanted to instantiate my WebDriver (in this case firefox) and my page objects with spring. I configured the spring 
beans using xml, but that is just annoying, at least using the architecture i chose….

I read the Page Objects wiki page from selenium ([here](http://code.google.com/p/selenium/wiki/PageObjects)). At first 
it all makes sense: you use the page objects to hide 
selenium from your actual tests in order to make them more readable and easier to write. Also the DSL stuff makes sense 
(however i dont like that for now as my tests are just plain simple without much clutter). But they are also suggesting 
to return other page objects when you are changing to another page by using new PageObject(). And that seems a little 
gross to me as you might have many page objects and you have to instantiate all of the by hand? lame…

{% highlight java %}
public class LoginPage {
    private final WebDriver driver;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public HomePage loginAs(String username, String password) {
        driver.findElement(By.id("username")).sendKeys(username);
        driver.findElement(By.id("passwd")).sendKeys(password);
        driver.findElement(By.id("login")).submit();

        return new HomePage(driver);
    }
}
{% endhighlight %}

So I thought it might be better to instantiate them all using spring and inject all sites / page objects that a site 
can refer to into that page object. And when a change to another site happens the correct page object is returned.

{% highlight java %}
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:/config/spring-config.xml")
public class AddTest {

	@Autowired
	private ListPage listPage;

	/**
	 * @throws java.lang.Exception
	 */
	@Before
	public void setUp() throws Exception {
	}

	@Test
	public void test() {
		listPage.open();

		AddPage addPage = listPage.clickAddPage();

		addPage.setFirstname("Peter");
		addPage.setLastname("Pan");
		addPage.submitForm();

	}

}
{% endhighlight %}

{% highlight java %}
public class ListPage extends Page {

	private AddPage addPage;

	private final String path;

	/**
	 * @param addPage
	 *            the addPage to set
	 */
	public void setAddPage(AddPage addPage) {
		this.addPage = addPage;
	}

	public ListPage() {
		path = "/list";
	}

	public AddPage clickAddPage() {

		WebElement link = driver.findElement(id("addLink"));
		link.click();

		return addPage;
	}

	public void open() {
		driver.get(baseUrl + path);
	}

}
{% endhighlight %}

however this creates too much logic in the page object. For now it does not seem that way but if you have a bigger site 
with many links and what ever you will have some pain maintaining your page objects to return the correct next page 
object (maybe depending on the given input). and also i configured everything with xml… so that is even more pain…. 
i like having xml in the application itself for reasons of configuration with maven and everybody in the team knows 
where to look when there is a problem with spring. however i will refactor the test to using annotations and autowiring 
as much as possible just because its easier….

back to topic: i think returning page objects other than this when using them DSLstyle makes maintaining your tests 
much harder later as they grow. I will inject my page objects directly into my test case and just use them as i need 
them. this way i reduce dependencies between the page objects and i can work more freely in my tests as i can do whatever 
i like and do not depend on whatever the page objects dictate.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.1.xsd">

		<!-- Scan for page objects -->
		<context:component-scan base-package="us.getit.hitlist.page" />

		<!-- our "browser" -->
		<bean id="driver" class="org.openqa.selenium.firefox.FirefoxDriver" destroy-method="close" />

		<util:map id="configData" key-type="java.lang.String" value-type="java.lang.String">
			<entry key="baseUrl" value="http://localhost:8080/hitlist" />
			<entry key="listPage" value="/list" />
			<entry key="addPage" value="/add" />
			<entry key="showPage" value="/show" />
		</util:map>

</beans>
{% endhighlight %}

{% highlight java %}
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:/config/spring-config.xml")
public class AddTest {

	@Autowired
	private ListPage listPage;

	@Autowired
	private AddPage addPage;

	/**
	 * @throws java.lang.Exception
	 */
	@Before
	public void setUp() throws Exception {
	}

	@Test
	public void test() {
		listPage.open();

		listPage.clickAddPage();

		addPage.setFirstname("Peter");
		addPage.setLastname("Pan");
		addPage.submitForm();

	}

}
{% endhighlight %}

{% highlight java %}
@Component
public class ListPage extends Page {

	@Value("#{configData.listPage}")
	private String path;

	public void clickAddPage() {

		WebElement link = driver.findElement(id("addLink"));
		link.click();

	}

	public void open() {
		driver.get(baseUrl + path);
	}

}
{% endhighlight %}

There is one downside to this approach: I will have to create a method on each page object to check that i am actually 
on the given page right now… but as i think about it i might dont even need that… my test will just fail when i am not 
on the correct page… so no need for that… or is there? we’ll see….

On the other hand they also said that you should individual page objects for your tests. I am too lazy to do that for 
now… so that argument doesnt count

