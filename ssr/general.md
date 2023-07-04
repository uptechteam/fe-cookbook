## SSR

*   [General information](#general-information)
*   [When should we use it?](#when-should-we-use-it)
*   [Cons and pros](#cons-and-pros)

## General information

In web development, SSR stands for Server-Side Rendering. It is a technique used to generate HTML on the server and send it to the client (web browser) as a complete page. With SSR, the server processes the request and dynamically generates the HTML content, including any data or templates, before sending it to the client.

Here's a brief explanation of how SSR works:

1.  The client sends a request to the server for a specific URL.
2.  The server receives the request and starts processing it.
3.  The server fetches the necessary data from databases or APIs.
4.  The server renders the HTML template with the fetched data.
5.  The server sends the fully rendered HTML to the client.
6.  The client receives the HTML and displays it in the browser.

---

## When should we use it?

1.  Content-driven websites: If your website primarily focuses on delivering content, such as blogs, news articles, or informational pages, SSR can be advantageous. It ensures that search engines can easily index your content, leading to better SEO and organic traffic.
2.  Landing pages: When you have landing pages for marketing or promotional campaigns, SSR can provide a faster and more complete initial page load experience. This helps in capturing the user's attention quickly and increasing the likelihood of conversion.
3.  Websites with static or slowly changing content: If your website's content doesn't change frequently or updates infrequently, SSR can be a suitable choice. It allows you to pre-generate the HTML and serve it to users, reducing the need for client-side rendering and optimizing performance.
4.  SEO-driven websites: If your primary goal is to optimize your website for search engine visibility, SSR can be a valuable technique. By providing fully rendered HTML to search engine crawlers, you enhance the chances of your pages being indexed and ranked higher in search results.
5.  Websites targeting users with slow connections or limited devices: If your target audience includes users with slower internet connections or devices with limited processing power, SSR can provide a better user experience. By reducing the processing required on the client side, you ensure that your website is accessible and usable for a broader range of users.
6.  Websites with personalized or user-specific content: While SSR has limitations with dynamic content, it can still be beneficial when it comes to rendering personalized or user-specific content on the server. By generating the HTML dynamically based on user data, you can deliver personalized experiences without relying solely on client-side rendering.

☝️ It's important to consider the specific needs and goals of your project when deciding whether to use SSR. Factors such as the nature of your content, target audience, performance requirements, and SEO priorities should all be taken into account. In some cases, a hybrid approach combining SSR and client-side rendering (CSR) may be appropriate to leverage the benefits of both techniques.

---

## Cons and pros

#### Advantages of SSR:

1.  Improved SEO: SSR makes it easier for search engine crawlers to index and rank your website since the fully rendered HTML is readily available. This can lead to better search engine visibility and organic traffic.
2.  Faster initial page load: With SSR, the server sends a fully rendered HTML page to the client, reducing the time required for JavaScript execution and data fetching. This results in a faster initial page load and a better user experience, especially on slower connections or devices with limited processing power.
3.  Enhanced user experience: SSR can provide a more consistent and usable experience, as the user sees the complete content immediately. There is no need to wait for JavaScript to load and render the page.
4.  Improved social media sharing: When sharing links on social media platforms, SSR ensures that the shared content is accurate and complete. Social media crawlers can scrape the fully rendered HTML, resulting in better previews and information when sharing links.
5.  Compatibility with older browsers and disabled JavaScript: SSR allows your website to be accessible and functional even for users with older browsers or those who have JavaScript disabled. The server generates the complete HTML, ensuring that the content is accessible to a broader range of users.

#### Disadvantages of SSR:

1.  Increased server-side processing: SSR requires the server to perform the rendering process for each request. This can put additional load on the server and require more processing resources compared to client-side rendering (CSR).
2.  Limitations in dynamic functionality: SSR may have limitations when it comes to highly dynamic or interactive features that heavily rely on client-side JavaScript. Certain functionalities, such as real-time updates or complex user interactions, may require additional client-side scripting even when using SSR.
3.  Higher complexity in development: Implementing SSR can introduce additional complexity to the development process. It may require specialized frameworks or libraries, and developers need to ensure proper server-side rendering setup and data hydration.
4.  Potential for longer time to first interaction: While SSR reduces the initial page load time, it may take longer for the user to interact with the page fully. This is because client-side JavaScript frameworks need to rehydrate and take over the interactivity after the initial HTML rendering.
5.  Caching challenges: SSR can present challenges with caching. Since the content is generated dynamically on the server, caching can be more complex to implement, especially when dealing with personalized or user-specific content.