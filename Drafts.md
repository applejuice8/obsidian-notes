My First Hackathon
Last Saturday, I participated in a Mini Hackathon organized by `Sunway Tech Club`, `Logitech` and `GDG Cloud KL`, with `AMD` sponsoring laptops and lucky draw prizes. We also had the opportunity to use the new Logitech MX Master 4 mouse, equipped with haptic feedback, it was definitely worth the hype.

I had the honour to be a part of a dedicated and collaborative team alongside `Tan Wen Zhe`, `Joe`, `Adam` and `Pei Jia`. Together, we built a fully local LLM using `Ollama`, `Gemma3`, `ChromaDB` and `Streamlit`. No API calls, no internet delays, completely offline and private. To enhance user experience, we also created a UI using `React`, which provided more flexibility and control compared to `Streamlit`.

Much appreciation goes to `Spyros Kyriazatis` and `Leong Lai Fong` for their active interactions with participants, guiding us throughout the hackathon.

Overall, it was a very exciting and fun event. Definitely looking forward to participating in more hackathons and continue learning from the community.

#Hackathon #Logitech #GDGCloudKL 

---

I want to contribute to the CS50 community by developing a website to display final project ideas. Right now CS50 website has a "Gallery of Final Projects" (https://cs50.harvard.edu/x/gallery/) but here are some issues. I propose to develop a website to improve the displays of final projects. By doing so, future participants can be inspired to develop more interesting software. If it doesn't violate CS50 rules, I hope to publish this as an open source project, recruiting volunteers from various social platforms like LinkedIn and Facebook.

Problem Statement
1. No sort function. If i am looking for Python projects, I have to manually check the tags for each project.
2. No rankings of popularity. Some projects have caught the eyes of many participants, but due to the lack of rankings, it will be overlooked by many others.
3. No pagination. Every project available are loaded instantly. If a user accidentally refreshed the page, it will be exhausting to scroll back to where they stopped.
4. Not all projects are displayed. For instance, my final project was not displayed on the website. And im sure many other people's too

Proposed Solution to Each Problem
1. Implement 2 separate filter buttons, 1 for programs (CS50x, CS50P, etc.), and another for tags (programming languages, libraries, frameworks)
2. Allow users to upvote / downvote each project to allow popular projects to be more apparent.
3. Limit the results of each page to 15, sorted by popularity.
4. Allow CS50 participants to upload their completed final projects to the website. To prevent spams, we can either manually approve projects or implement an authentication that only allow CS50 participants / alumni to post their project.

This project not only enhances the current gallery of final projects, it is also a way for us to contribute to the cs50 community, fostering collaboration.

---

Subject: Proposal to Build an Improved CS50 Final Project Gallery Platform

Dear CS50 Team,

I hope this message finds you well. I am writing to propose a CS50 community-driven project aimed at improving how CS50 final projects are showcased and explored.

The current “Gallery of Final Projects” is a valuable resource, but several limitations reduce its usefulness for both learners and alumni. To address these issues, I would like to develop an independent, open-source website that improves discoverability, usability, and community engagement around CS50 final projects. I will proceed only if this does not violate CS50’s policies regarding branding, content use, or student submissions.

Key issues with the current gallery:
1. Lack of filtering options
	- Users cannot easily browse by language, topic, framework, or course variant.
2. No popularity indicators
	- Projects that receive significant attention are not surfaced to new users.
3. No pagination
	- All projects load at once, reducing usability and causing loss of progress if the page refreshes.
4. Incomplete coverage
	- Many valid projects, including my own, do not appear in the gallery

Proposed solution for each problem:
1. Implement structured filtering for course (CS50x, CS50P, etc.) and for tags (languages, libraries, frameworks)
2. Support optional upvotes/downvotes to highlight noteworthy work
3. Add pagination (e.g., 15 items per page) with sorting by popularity or recent activity
4. Provide a submission workflow allowing CS50 participants and alumni to add their projects. To prevent spam, the platform could use manual review, lightweight verification, or authentication tied to CS50 identity systems if permitted.

My goal is to support and strengthen the CS50 community by making it easier for learners to find inspiration, compare approaches, and share their work. If approved, the project will be fully open source, and I intend to invite volunteers from CS50 groups on platforms such as LinkedIn and Facebook to contribute to development and maintenance. I have also attached a screenshot to better illustrate my proposal.

I would appreciate your guidance on:  
• Whether such a project aligns with CS50’s policies
• Any restrictions on branding, naming, API use, or use of links to student submissions

Thank you for considering this proposal. I would be happy to provide a more detailed project plan if useful.

Sincerely,  
Colin Leong
Computer Science Student and Former CS50 Participant