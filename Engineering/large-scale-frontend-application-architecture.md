# Large Scale Frontend Application Architecture

*This article is originally appeared in [here](https://mp.weixin.qq.com/s/D6Pk2NOoPWGMyG9NXZUY8Q), written by a frontend engineer working for Alibaba Group. Had worked for Alibaba Group for two years, I could not agree any more what's in the article so I decide to translate it into English.   
 
# 1. Overview
* where to use
What's a large scale frontend application? We defined that a project is developed more than 10 frontend engineers. Naturally, practices listed here is suitable for a project having more than 10 frontend engineers. 

General speaking, the expected economic return differs from projects. Bigger projects is, more complex it will be, more return is expected. For a smaller project, the practices listed here is not necessary applicable. 
 
* the Principal
The effort to have a well designed frontend architecture is to 1) solve existing problem or 2.) predict the problem in future, provide possible solutions or prevent the future problem arising. In the end, less technology debt, make the application more manageable, more stable and more extensible.

The decision making process should focus on evaluating the economy value a well designed frontend architecture will bring. 1.) how much human capital to be put into the application 2.) the money and time it could save in future. 3.) project risk it could help reduce. 4.) potential working as an infrastructure to support more application or business cases, in a both quantitative and qualitative way. In the other word, when talk about the effort to architecture an application, it will be really helpful if we can quantify the economy value. If we really couldn't give a dollar amount, at least we need to give common understandable, persuasive and acceptable reasons.
Data and facts should be used to quantify our claim. Especially, while trying to build an infrastructure whose business value might not as obvious as an application.
 
* where to start
In this article, we separate our discussion into two folds, one is called Infrastructure architecture. Infrastructure alway means that it will be commonly used by all applications, also means to be a foundation to all applications building upon on it; Secondly, we call it business application architecture. It is user facing, and meant to be solve a particular problem.
 
* Others
Designing a large scale frontend application needs comprehensive knowledge and skills across frontend, backend, devOps and test. The more complete the skill set is, the better the architecture could be for now and future. In the meantime, the problem it might solve includes but not limit to frontend application only.
 
 
# 2. Infrastructure architecture
1. GitLab
Why needs to have the in-house GitLab deployment while everyone is using about Github, or Github Enterprise or if you want to get a dedicated Github Service. However, it might worth another thinking, why not deploy your own Git server while the deploying a GitLab literally is effortless.
GitLab is a proven solution and used by companies such as Alibaba Group. It provides the benefits:
* the similar experience with Github, but you own your code on your server
* access control - who can view, add, read and delete code repositories.
* code review
* your own workflow
* the third-party plugin

the reasoning:
Code is the asset of a company. We see cloud services come and go. Own your code by putting a place that stays with the company.
 
 
2. Version Control Best Practice
There are quite a few of version control best practice. We focus on:
* Once release to the production, the production git branch is locked.
* Automating the process releasing to the production.
* co-existing multiple versions in production. For example, while version 0.2  is release, the version 0.1 is still available on the CDN. It is really for the easy rollback if something goes wrong with version 0.2.

the reasoning:
More manageable. We can quickly rollback to the previous working version we knew.
 
3. Continuous Integration
Once code is merged to the release branch, the following steps, build, package, release to CDN or static assets servers should be kicked off automatically. It should not be any involvement from developers.
 
the reasoning:
Let developers focus on development.
 
4. Release to Production 
It is suggested that there are two steps needed to release a frontend app to production
*step 1* Release the static resources (javascript, css, images, etc) to the production. You can directly visit your static resources by visiting the resource link. However, html pages are still the older version
*step 2* Release the html files having latest static resources.

the reasoning:
the frontend resources (javascript, css, images, etc) can be released to the production independently from the backend *(see blow to understand more. Basically, the backend is responsible for the html files)*. The drawback is that a custom tool is needed to make the two steps working smoothly.

The Value:
In this way, we can improve the efficiency of release, rollback quickly while needed.
 
5. Boilerplate
A boilerplate is an application that has some commonly used codes such as authentication, date format, money format, log, network, lint, test, mock, etc, and so on, to make it easier for developing a concrete project.  
The benefits
* reduce the time of configuring a new project
* common and unified file structure and practice to make the project much more managable, reduce the time of discussion of what's the best, and make the learning curve much more smooth
* make the unified technology stack possible
 
the value:
make developers working on multiple projects at the same time possible. It will also help the company build the deeper technology stack
 
6. API Gateway
It is not uncommon that an API gateway is developed by frontend team for traffic management, security and so on.

the value:  
Besides what an API gateway can offer, it is helpful for frontend to decouple from backend even better; 

7. Analytics
It is highly recommended that a large scale application needs have an in-house analytics and user tracking solution. An analytics solution example is [Google Analytics](https://analytics.google.com/analytics/web/), and it is different from the log system widely used by backend.
An analytics service provides insights such as page views (PV), user views (UV), etc, by day, month and year; It helps capture exception and errors. Normally, it provides some sort of data visualization for easily read and understand.

However, it's common that commercial solutions only provides aggregated data, not the raw data you can make advantage of, or not provide the raw data in real-time or near real-time fashion, or it doesn't provide out-of-box solution to do more complicated analytics.

The analytics data tracked by the frontend application is the first-hand information you can obtain while users interact with your application. It provides much needed data for product owner and marketing to make better business decision. The value for frontend developers is to understand business better, understand users better. It has advantage over data collected and aggregated by the backend.  
With data tracked or aggregated by analytics service, frontend developers would know better about application's performance, how users are actually using the application, where to improve the user experience, such as functionalities, layouts etc.

the reasoning:
First-hand data provides more insight.
 
8. Monitoring and alerting
Monitoring and alerting service is built upon the analytics service. An alert might be triggered while one of the following events triggered:
* daily PV/UV changes dramatically. For example, only 20% of normal value
* hourly error or exception increases dramatically. For example, 200% or above the normal value
* no PV/UV at all for a period of time. For example, 0 PV/UV in one hour but normally it is around 100
 
the reasoning:
Improve the stability of application; reduce bugs which are not easily found in the in-house test process.
 
9. Security
In general, we need to rely on the backend to secure our application. Here are some practices the frontend can follow
* XSS Injection - escape or validate or sanitize user's input
* https - no need to say more
* CSRF - it needs to have server side support of CSRF 
 
10. Eslint
Nowadays, most used frontend frameworks come with Eslint integration. It helps
* reduce typo-related bugs
* make the code much more readable and maintainable
* have the opportunity to enforce code style; make it easier for team collaboration and  new hires onboarding.
 
11. Grayscale Release
Grayscale release is commonly used in the large scale application. When release a new version of application, we steadily increase the percentage of user access when each percentage scale goes well; Or rollback to the older version once bugs or issues are found. It is particularly important to the web pages having majority traffic, or pages playing critical roles in business.

the benefits:
* production environment is much more complicated than the test or development environment. Grayscale release minimizes any possible unwanted impact, while provides most possible evaluation against the real environment.
* If this is a big release with tons of features, this is the chance to get users' feedback. We validate the initial thoughts about the new features from analytics data; Also it gives a chance to rollback quickly.
* A/B test
 
A common used scale:
1. 1%
2. 5~10%
3. 30~50%
4. 100% (full release)
 
The strategy of grayscale release depends on the business case. For instance, it can be IP range or user account criteria or certain geo locations.
 
12. Single Page or Multiple Page
In the modern web application, typically the frontend application is responsible for the static resources such as html, css, javascript and images, and the backend provides APIs and logic to match url with html. 
However, this is not typical in a large scale application. In a large scale application, the backend is also responsible for the html pages, the frontend is responsible for everything else (css, javascript, etc).
The reasoning:
* it is common that backend service needs to call other APIs to render a html. For example, a backend service needs to call a translation service API to provide different language meta data to render a html page
* header and footer of a html page
* it's better decoupling practice
* it is easier for frontend to release new module without modifying html; it will also be easier to do grayscale release
 
Value:
decouple html pages and functionalities; much more standardizing html page management.
 
13. Mock
To decouple the development of the frontend and the backend, the mock is widely used in the frontend. Mock can be part of boilerplate. One advantage to have mock in the boilerplate is the ease to use. for example, ```http://localhost:8080/index.html``` is a normal link in the development; ```http://localhost:8080/index.html?debug=true``` will load mock data.
Some other practice is to have a dedicated mock service, then let frontend point to API endpoints of the dedicated mock service. The advantage is that complicated data structure can be faked by the much more capable mock service.
 
Value:
The frontend and the backend can be developed fairly independently by having a data contract which defines the mocked data.
 
 
14. Backup
Backup regularly is still important, even though we rely on Git service extensively nowadays.
 
# 3 Application Architecture
1. Multiple Page Application(MPA) and Single Page Application(SPA)

It is highly recommended to choose MPA for large scale application.
* Pages are independent from each other, refactoring is much easier. In the end, it fits better for the life-cycle of a large scale application in terms of maintainability and refactorability.
* One of drawbacks of MPA is a white screen between pages. It is transient and has been proven it is acceptable by users.
* Version control is easier. If need to split pages or change the workflow, it will also much easier.
* better for grayscale release.
 
Value:
maintainability and refactorability.
 
2. Code Repositories
Obviously, each application needs to have its own repository.
 
3. Components Library
It is very important that developing and maintaining a common UI components library for reuse for a large scale application or for the company. However, open source UI components can also be used for this purpose. Developing in-house solution or using open-source projects, leave the options open but need to have one.
If decide to develop in-house UI components library, it requires a unified technology stack to maximum the value of the effort. Some suggestion include:
* use typescript (ts)
* as extensible as possible
* as adaptable as possible
* good documentation - consistent updating and be detailed 
* versioning each release
* working closely with UI team to align with the company's visual identify guideline
 
the value:
a better user experience by having a consistent user interface(UI) and user experience (UX) experience; reduce the developing time to define, discuss and develop styles; In the end, make the development more productive.
 
 
4. Unified Technology Stack
Right now, there are three major frontend technology stack: ```vue.js ecosystem```, ```react.js ecosystem``` and ` Angular ecosystem`. jQuery is still being used. There are also many UI frameworks, third party libraries.

For a large scale application, it is common that some or all technology stacks could be co-exited if decision are not made carefully. It will bring tons of challenge down to the road. One possible case is that `angular` is not popular anymore, no one wants to update the existing portion of `angular`. History repeats itself and we have seen that.
For that reason, it is highly recommended to consolidate the technology stack
* choose one from the three. `Vue.js` is most popular in China; `React.js` and `Angular` are most popular in western countries. `vue.js` is easier to pick up, fits the team that is not very technical strong. `react.js` and `angular` have certain learning curve to overcame. 
* if have to be compatible with IE8 or older, jQuery is recommended
* don't duplicate the effort, and as company, don't repeat yourself. Let one team build a common library unless there is a very compelling reason to build another one.
 
The value:
Hiring is easier; Easier to train new hires; Make project development more sustainable. 
 
5. Browser Compatibility
To compatible with IE6, IE7 and IE8, extra efforts are needed. It is recommended to have a common solution, maintained by dedicated person or team. Some solutions include
* config postcss so some of css can handle the compatible issue
* customize the loader of webpack to handle the compatible issue
* standardize how to write css to avoid common pitfalls which create compatible issues
* summarizing and sharing
* in the end, guide users to use the latest version of the browser

the value:
Avoid the compatible issues. Normally, locating and fixing this kind of bug is very costly.
 
6. Knowledge Base
Share new technology, best practices, ongoing projects and ideas without worrying about data privacy and security, it is worth the effort to set up an internal knowledge base application.
the benefit:
* share the experience that figured out an internal system, which could benefit dramatically for the people who are using the same system
* share the latest technology and learn from each other
* better company and team culture
* a history of the company

the value:
better company culture; team culture building; knowledge accumulating;
 
 
7. Access Control
A company should set up or develop an access control system to manage and governance user's access.
* automated process control to manage the life cycle of an employee within a company
* access control needs to have an expiration date
* the flow of applying an access control should be transparent, easy to understand and easy to get hold of the decision maker of each process
* needs to follow the work flow principal of the company, and be adaptable with company's policy's change
 
the value:
standardizing and digitalizing the company's process and workflow
 
8. Authentication and authorization system
Single Sign On (SSO) needs to be implemented to reduce the user's experience fluctuation across different websites of the same company. Once logged in, any application within the company is logged in; Likewise, once logged off, any application is logged off. The benefit of SSO:
* better user experience
* user data across different applications is much more communicable and exchangeable
* easier to manage, track and aggregate data to profile a user
* make developers life easier by not developing & maintaining the user authentication and authorization service for each application independently
 
Value:
better user experience; make data more exchangeable among application; reduce the cost of developing the feature independently;
 
9. Use Content Delivery Network (CDN)
Content Delivery Network (CDN) is rather mature. The benefits for frontend application is obvious:
* provides static resources close to a user's geo location so much faster for users
* less bandwidth request load for the backend server
* supports multiple contents such as video, audio, images, files and live broadcasting, etc
* reduce the network bandwidth difference by wireless carriers
* lower the risk of distributed denial-of-service (DDOS) attack
 
10. Load Balance
`Nginx` is a common choice. For a large scale application, load balance and distribution system are must. The benefits of load balance
* distribute request evenly
* much more flexible to scale out
 
 
11. Same APIs for all platforms (mobile, web, etc)
If possible, the same APIs for all platforms - mobile, web or desktop.  
 

# Summary
Basically, this post summarizes one aspect of Alibaba technology, and the principals to build in-house solution. Hopefully, it is helpful for you when make your technology related decision. 
