I was tasked to build out a landing page for <a href="https://us.honmagolf.com/" target="_blank">Honma Golf's</a> marketing efforts. Honma recently re-developed their US eCommerce website and decided to build it on Shopify. They needed a quick solution for their sales reps to send out an email blast to current and prospective clients about their new clubs. Because of the design complexities and number of products they wanted to showcase, we decided to build a landing page template in Shopify vs an HTML email template. In the email blast, we included an image banner that links to the Shopify landing page.
<a href="https://us.honmagolf.com/pages/discover-honma" target="_blank">View Landing Page</a>


![Honma Golf Landing Page](img/work/proj-13/honma_landing_page_mockup.jpg)

## Key Takeaways

- Since this was a landing page, you don't want the page to show up in search results. You can prevent a page from appearing in search results by including a noindex meta tag in the page's HTML code. I created an 'if' statement in ``` theme.liquid```'s header that will only inject this noindex code in the 'landing1' template.
```
  {% if template contains 'landing1' %}
    <meta name="robots" content="noindex">
  {% endif %}
```
- Shopify is limited when it comes to customizing URL structures, so if you were to create a landing page the URL structure would be sitename.com/pages/landing-page. Unfortunately, you can remove 'pages' from the URL so take this into consideration when building landing pages in Shopify. 