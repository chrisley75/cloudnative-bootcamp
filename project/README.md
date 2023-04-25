# CI/CD From First Principles: Project

## Description

In the last two days of this course, you will work in Squads teams to complete a project. The goal of the project is to apply the techniques and concepts learned in this course to deliver an application to the cloud.

Each Squad has at least one Cloud Engineer on it, so you can rely on that person's development background to determine which stack you will operate in.

This project has no definitive end and is meant to simulate the types of challenges you may face on a client site. Your goal is to get as far as possible, as a Squad, over the next two days:

1. Pick an application stack that at least one person on your Squad is familiar with. Create an application in that stack using the appropriate generator or other project creation mechanism. For example, if you wanted to write a Java/Spring Boot application you would start at [Spring initializr](https://start.spring.io/).
1. Create a [Trello Board](https://trello.com/) for your Squad. Make sure the board has at least these columns:

   - Ice Box
   - Backlog
   - In progress
   - Done
   - Accepted

   Before beginning any challenge after this one, first work as a Squad to decompose the challenge into its component steps. Elect one person to write down what is required. One easy way to know "what is an appropriately sized chore?" is to think of the smallest thing that will have some type of observable outcome. And feel free to create a few Stories that, once implemented, provide real value to the end-users of your product.

1. Deploy your application to the cloud using the principles of this course (i.e. a CI/CD pipeline). (_Reminder:_ Break down the work into deliverable units ("chores") first)
1. Create another application and CI/CD pipeline for it. If your application from step 1. was a frontend, add a backend, and vice versa.
1. Modify the code of one application to use the API of the other and make sure they are both deployed.
1. Ensure that at least one of your applications uses a database.
1. How would you consider enhancing your pipeline? In what ways do your current pipelines seem insufficient? As a developer using the pipeline, do you have a way to keep working locally and find out if something went wrong? Is your development flow pull or push based? What should it be? Implement at least one change to your pipeline that improves the developer experience.
1. Test Driven Development (TDD) is a key practice in building high-quality, large-scale, distributed systems. Imagine you had to design a system, deployed to the cloud, that needs to send internationally supported Text Messages to 50 countries across a few hundred carriers. Further, depending on the user's settings (if they exist) the user receives a message in the language of their choosing, otherwise they receive a message in the default national language of the country code.

   Your Product Owner is scared to deploy this feature without enlisting a few hundred QA people, all across the world to manually send and receive messages for every country/carrier combination and user language setting as well. How does your team test this? Is what you have deployed sufficient? How would you change it? See how many of those changes you can implement.

1. What else can you think of? How else can your pipeline or application grow in complexity?

> Hint: Consider keeping track of the [DORA Metrics](https://www.cloudbees.com/blog/dora-devops-metrics-bandwagon) for your project.
