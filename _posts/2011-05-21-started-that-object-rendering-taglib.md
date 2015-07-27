---
title: Started that object rendering taglib
layout: default
titleImage: /assets/copy-pasta.jpg
---

So I started writing my own object oriented rendering JSP taglib as I could not find anything like that anywhere. 
Ok I just played around with taglibs in general because I had no idea how to write Java tags, I only did JSP tags before.

{% highlight java %}
public class SampleTag extends TagSupport {

	private Object o;

	@Override
	public void setPageContext(PageContext pageContext) {
		super.setPageContext(pageContext);

	}

	@Override
	public int doStartTag() throws JspException {

		try {
			pageContext.getOut().flush();
			ServletRequest req = new HttpServletRequestWrapper((HttpServletRequest) pageContext.getRequest()) {
				private final Map<string, object=""> wrappingAttributes = new HashMap<string, object="">();

				@Override
				public void setAttribute(String name, Object o) {
					wrappingAttributes.put(name, o);
				};

				@Override
				public Object getAttribute(String name) {
					Object o = wrappingAttributes.get(name);
					if (o != null) {
						return o;
					}
					return super.getAttribute(name);
				};

			};
			req.setAttribute("something", o);
			pageContext.getRequest().getRequestDispatcher("/WEB-INF/jsp/blubb.jsp")
					.include(req, pageContext.getResponse());
		} catch (ServletException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			throw new JspException(e);
		}

		return SKIP_BODY;
	}

	public void setSomething(Object o) {
		this.o = o;
	}

}
{% endhighlight %}

I must admit it is not very pretty (yet) but it does some stuff. For now I am able to pass a variable something through 
the tag to another JSP (blubb.jsp). And as I put the object in my request wrapper it can be used in the blubb.jsp. I 
used the wrapper because I did not want to write in my actual request (who the fuck knows what will happen if I mess 
with that :P ). But the question is what happens if I use the tag within my blubb.jsp again? Is a new instance of the 
tag class created on each use and will there be problems with the wrapped request then?

Also is this the best way to include a JSP? you might wonder why I need to flush the out at the beginning of the 
method… well if I don’t do that it my included jsp is rendered first before the jsp using my tag… but this flush causes 
the site being transfered to the browser before it is completely rendered which kind of gives me cancer. 
I cunt pasted that bloody dirty piece of code from [here](http://stackoverflow.com/questions/4550446/including-a-jsp-page-programatically)

Also here is the tld of my little taglib (copy pasta from JSTL core):

{% highlight xml %}
<!--?xml version="1.0" encoding="UTF-8"?-->
<taglib xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemalocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd" version="2.1">
	<display-name>Sample taglib</display-name>
	<tlib-version>1.0</tlib-version>
	<short-name>sample</short-name>
	<uri>http://meh.get-it.us/taglib/sample</uri>
	<tag>
		<name>sample</name>
		<tag-class>agagagag.SampleTag</tag-class>
		<body-content>empty</body-content>
		<attribute>
			<name>something</name>
			<required>true</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
	</tag>
</taglib>

{% endhighlight %}
