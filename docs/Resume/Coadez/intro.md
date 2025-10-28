# Codeaz - Competitive Programming Analytics Platform

## Project Summary

In our hackathon project **Codeaz**, we wanted to solve the problem of
tracking competitive programming progress across multiple platforms.
Normally, a student has to open LeetCode, Codeforces, and CodeChef
separately to check ratings, problems solved, and contests. We built a
single dashboard where all this data comes together. Users enter their
handles, and our backend fetches their data from all three sites. For
**Codeforces**, we used their public API; for **LeetCode**, we used
their internal GraphQL API; and for **CodeChef**, since no proper API
exists, we scraped the profile page using **JSDOM**. We saved this data
into MongoDB and displayed it on a React frontend. To keep everything
updated, we automated refresh using **GitHub Actions**. We also made the
frontend a **Progressive Web App (PWA)**, so users could install it on
their phones and access it like a native app. The result was a platform
that 35+ users tested, and it saved them time by giving all stats in one
place.

------------------------------------------------------------------------

## What is GraphQL?

GraphQL is a query language for APIs that lets us request only the exact
data we need, instead of receiving everything like in normal REST APIs.
For example, in my project I used GraphQL with LeetCode to fetch only
the user's contest history, rating, and problems solved in a structured
way, which made it efficient.

------------------------------------------------------------------------

## What is JSDOM?

JSDOM is a JavaScript library that allows us to work with HTML pages in
Node.js as if we are inside a browser. It creates a virtual DOM from the
webpage, so we can select and extract elements like ratings or stars. I
used JSDOM (library) for CodeChef because it does not provide a proper
public API, so scraping the HTML was the only way to get user stats.

------------------------------------------------------------------------

## Refreshing Data

In our project, we didn't want users to click refresh every time to get
updated data. So, we built a function called **refreshData**. This
function goes through all the users stored in our database, fetches
fresh stats from Codeforces, LeetCode, and CodeChef, and then updates
the records in bulk. To avoid hitting API limits, we added a small delay
between each request. We also automated this process using **GitHub
Actions**, which runs the refresh function automatically on a fixed
schedule (like every few hours). This way, the data in our app always
stays updated without any manual work from the user.
