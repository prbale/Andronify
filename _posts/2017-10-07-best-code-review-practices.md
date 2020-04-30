---
layout: post
title:  "Best practices for Code Review"
author: shraddha
featured: true
image: assets/images/code_review.jpg
categories: [ coding, development ]
tags: featured
---

Benefits of Code Review

Multiple eyes on a piece of code. Finding bugs early.
Coding standards compliance
Consistent design and implementation. Better Structured code.
Team cohesion
Learn how to write better code
Teaching and Sharing Knowledge. Mentoring tool.

 Guidelines for Authors
 Before raising PR:
 - Code is "complete as possible"
 -  Small Pull Request.
 -  In case of Long Request, "Point out important parts"
 -  Explain changes in PR Description along with JIRA story links
 -  Read the checklist and mark the things which you have considered for this PR
 -  Pull Request Title : Use JIRA ticket and useful title description

 After Raising PR:
 -  Be grateful for the reviewer's suggestions.
 e.g. Good call. I'll make that change., Whoops. Good catch, thanks. Fixed in a4994ec.
 - A common axiom is "Don't take it personally. The review is of the code, not you."
 - Try to respond to every comment.

 Guidelines for Reviewers
 -  Don't nitpick - Focus on the correctness of code. Less focus whitespaces/variable names.
 -  Don't be a jerk - No harsh comments plz
 -  Ask good questions; don't make demands.
 - Avoid selective ownership of code. ("mine", "not mine", "yours")
 - Provide Examples  ( Small code snippets will do )
 -  Seek first to understand

Tools and General Advise:
 -  Schedule time for PR's
 -  Automate with code linting
 - Use a checklist

 Pull Request Template:

 To add this template to GitHub project, You just have to create "pull-request-template.md" file in root directory.
 Below is the sample format for the PR template.

 ```

# Description

Please include a explaination of the change. Please also include relevant motivation and context. List any dependencies that are required for this change.

JIRA Ticket link:

Please delete options that are not relevant.

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] This change requires a documentation update

# How Has This Been Tested?

Please describe the tests that you ran to verify your changes. Provide instructions so we can reproduce. Please also list any relevant details for your test configuration

- [ ] Test A
- [ ] Test B

# Checklist:

- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] Unit test cases added for this change.
- [ ] Considered accessibility for UI change                                                                                                                                                                                                      - [ ] There are no merge conflicts                                                                                                                                                                                                                                                              .                                                                                                                                                                                      # Impacted Areas in Application                                                         List general components of the application that this PR will affect.
