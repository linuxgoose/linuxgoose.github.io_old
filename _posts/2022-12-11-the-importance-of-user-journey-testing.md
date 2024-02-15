---
layout: post
title: The Importance of User Journey Testing
subtitle: Over the past few years I’ve been involved in numerous development projects. The size of the projects have varied from small, to medium, to enterprise level. As many would expect, the smaller projects were less structured in their testing when compared to the larger projects. Unit testing was more sporadic and random and code reviews were infrequent whereas the medium-to-large projects were far more organized and robust with their implementation of unit testing and code reviews. Use cases were well thought-out and included every bit of detail for new features. However, all projects shared one thing in common. Bugs. A lot of them.
tags: [ux-research,ux,user-journey,product-design]
comments: true
author: Jordan Robinson
---

{: .box-note}
**Note:** A User Journey Test involves walking through the exact steps that your users will take day-to-day as they interact with multiple “modules” on your platform.

Over the past few years I’ve been involved in numerous development projects. The size of the projects have varied from small, to medium, to enterprise level. As many would expect, the smaller projects were less structured in their testing when compared to the larger projects. Unit testing was more sporadic and random and code reviews were infrequent whereas the medium-to-large projects were far more organized and robust with their implementation of unit testing and code reviews. Use cases were well thought-out and included every bit of detail for new features. However, all projects shared one thing in common. Bugs. A lot of them.

When one hears the term “User Interface Testing” it appears that far too much emphasis is put on the word interface and not enough on user. As developers we often fall victim to a code-first view of their work and become hyper-focused on pumping out the best possible code that we forget that the core audience of software writing — users — also require the same level of attention.

This was a common theme amongst each and every project.

Users are not focused on each individual module in a platform like developers usually are. Generally, they utilize numerous modules with each visit. They do so by having a specific task or goal in mind that they are trying to accomplish.

One project member asked me in the past, so then how do we ensure that the user can get to their end goal? This is accomplished through User Journey Testing.

## What is User Journey Testing?

At its core, User Journey Testing is the act of writing a test case for a user’s journey when they perform a particular task on your website or web application.

### User Journey

Thus, a user journey can be defined simply as the specific steps a user performs when accomplishing something on your platform.

This could be as simple as intending to contact your business or something more complex as configuring a product to their needs and submitting an order to your business.

At the core in the above examples, the user’s journey could be landing on the homepage, clicking the appropriate link, filling out the forms, entering payment information (and maybe creating an account), and sending the applicable information your business.

As such, a User Journey Test involves walking through the exact steps that your users will take day-to-day as they interact with multiple “modules” on your platform.

## Why Is User Journey Testing Important?

Now that you understand what a User Journey is and what User Journey Testing is, you may be asking why is it important?

Far too often development teams are assigned individual modules to code (creating, enhancing, or fixing). More than some would like to admit, these teams may not be professionals at documentation or communication with the other team.

### Example — An Online Store

For illustration sake, let’s assume there is one team (Team A) that is working on a product configuration module and the other team (Team B) is creating the cart and payment module.

After developing each modules separately they test their work. Team A performs their unit testing to ensure that the user is able to configure their ideal product to their preferences and Team B ensure that their system is able to capture payment via PayPal as expected. In isolation, this seems great! Both modules are working — we’re ready to launch!

Wrong.

As Team A and Team B put their respective modules into production they begin receiving some complaints from customers. In some instances customers are reporting that when they add their configured product to their cart, certain configuration options they had selected are not accurate. Say for one customer they had initially selected for their product to be red, but upon being added to their cart it reverted back to the default blue. The user closed all of their tabs and revisited the store. This time the customer did not experience an issue during the ordering process, but upon receiving their item it was incorrect. They had actually received the item in purple! However, the business is certain they fulfilled the order as per the order they had received from the customer — for it to be purple, not red. Upon further investigation, they found that upon the submission of the order, the system swapped the color when inserting the order into the database.

This is where unit testing can fall short in performing sufficient verification of functionality. Individually each tam would have seen that their modules worked correctly. Team A (product configuration module) would have seen that the customer was able to configure the product as per their needs on their end and Team B (cart and payments module) would have seen that the customer was able to place an order and pay through PayPal). However, the user’s intention was not met.

The user’s journey to accomplish their intent of purchasing a red product was not fulfilled.

Had the development team conducted User Journey Testing they would have found that during the communication (passing of values) between modules been tested there was a translation error.

## Conclusion

It is easy to fall victim to our own confirmation bias and focus on testing what is important to us, but in the bigger picture we must consider the end-user’s experience.

User Journey Testing is exactly how we can accomplish this. By following the footsteps of our user’s from the beginning we will not only be able to ship out a far superior product (bug-wise) we will also be able to better understand how to make the product better functionality wise.