Google Summer of Code 2024 – Final Report

Student: Mohit Marathe
Organization: LibreOffice (The Document Foundation)
Project: Comments in Sidebar
Mentors: Sarper Akdemir, Heiko Tietze

1. Introduction
LibreOffice is a powerful and free open-source office suite, widely known for its feature-rich tools and active community. As part of Google Summer of Code 2024, my project focused on enhancing the user experience in LibreOffice Writer by introducing a Comments Sidebar—an alternative and modern way to interact with comments, designed to improve collaboration, readability, and workflow efficiency.

2. Project Motivation
Currently, comments in Writer are displayed in the document margin. While functional, this layout becomes overwhelming in heavily annotated documents, and it limits interaction. The goal of this project was to create a sidebar view for comments—offering a more intuitive, scalable, and structured interface, similar to what users are familiar with in modern word processors like Microsoft Word or Google Docs.

3. Project Objectives
The primary objectives of the project were:

Develop a sidebar panel for comments using GTK and LibreOffice’s UI framework

Synchronize sidebar comments with the existing margin annotations

Support all standard comment actions (edit, reply, delete, resolve)

Add sorting and filtering options for easier navigation

Ensure real-time updates via event broadcasting

Improve UX with context menus and future-ready design

4. Development Approach
During the Community Bonding period, I created a development roadmap and began work on the UI using Glade, LibreOffice’s preferred interface designer. The sidebar design includes a comments panel to hold multiple thread widgets, with each thread containing one or more comment widgets. GtkBox containers were used for flexible layout handling.

5. Backend & Data Binding
To populate the comments sidebar, I implemented logic that retrieves all annotations from the document. Using internal functions like SwAnnotationWin::GetTopReplyNote (for root comments) and SwPostItMgr::GetNextPostIt (for replies), the system builds comment threads dynamically. A std::unordered_map tracks and organizes comment-thread relationships, enabling fast rendering and future updates.

6. Real-Time Synchronization
To ensure consistency between the traditional margin-based view and the new sidebar, I integrated LibreOffice’s event broadcasting system, using the SfxBroadcaster and SfxListener pattern. This allows the sidebar to respond to changes in real time—comment creation, editing, deletion, and resolution—by implementing a custom Notify method that listens to annotation events and updates the sidebar accordingly.

7. Functional Interactions
All standard comment actions are now fully supported from the sidebar. When a user edits, deletes, replies to, or resolves a comment, the change is performed in the backend, then broadcasted and reflected in both the margin and sidebar interfaces. This bi-directional synchronization ensures a seamless editing experience.

8. Sorting, Filtering & UX Enhancements
To enhance usability, I introduced:

Sorting: By document position or timestamp

Filtering: By author or date range

Context Menus: Right-click options for quick actions like “Edit”, “Reply”, “Resolve Thread”, etc.
These additions provide users with flexibility and better control over collaborative editing, particularly in large documents.

9. Remaining Work & Future Scope
Some features are still in progress or planned for future contributions:

Hover Connector: Display root comment in a balloon tooltip when hovering over the reference text in the document

Custom Author Badges: Add color-coded author indicators for better visual context

Rich Text Formatting: Enable bold, italic, and other text styles inside comment content

These features are designed to bring the comment system closer to modern standards while preserving LibreOffice’s open-source integrity.

10. Conclusion & Acknowledgements
Working with LibreOffice through GSoC has been an incredible learning experience. I gained practical skills in C++, GTK, large-scale software architecture, and open-source collaboration. I'm especially thankful to my mentors, Sarper Akdemir and Heiko Tietze, for their continuous support, code reviews, and invaluable guidance throughout the project.

I look forward to continuing my journey with the LibreOffice community and contributing further to its mission of open-source excellence. Thank you to The Document Foundation and Google Summer of Code for this wonderful opportunity.

Commits Overview

sw: add Comments Panel in sidebar

sort comments by time or position

filter by author & time

display reference text on thread

add context menu for comments & some other changes

sw: make show option work in comments panel

sw: add icons in sidebar's comment widget