# My GSoC 2024 Report

**Student:** [Mohit Marathe](https://gerrit.libreoffice.org/q/owner:mohitmarathe@proton.me)

**Organization:** [LibreOffice](https://www.libreoffice.org/)

**Project:** [Comments in Sidebar](https://summerofcode.withgoogle.com/programs/2024/projects/Hht1NBGx)

**Mentor:** [Sarper Akdemir](https://de.linkedin.com/in/sarper-akdemir), [Heiko Tietze](https://de.linkedin.com/in/heiko-tietze-4204aa30)

---

## Overview

LibreOffice is a free and powerful office suite, and a successor to OpenOffice.org (commonly known as OpenOffice).
Its clean interface and feature-rich tools help you unleash your creativity and enhance your productivity.
Check out the [official site](https://www.libreoffice.org/) for more information.

## Project

The goal of this project is to enhance the LibreOffice Writer by implementing a comments sidebar feature.
Currently, comments are displayed at document margin, which can be cumbersome and disrupt the user’s workflow.
By adding the comment section in the sidebar, the aim is to improve usability, streamline the commenting process, and enhance collaboration within the application.

## Work

Since I made a rough plan of how I would implement the feature during the proposal phase, I started working on the project right from the _Community Bonding_ period.
I started with creating the UI of the comments panel, the thread widget and the comment widget using **Glade**. In the comments panel, the Gtk Box container will accomodate
the threads and the Gtk Box inside the thread widget will accomodate the comments.

After that, I worked on the method to populate the comments in the comments panel. It will first collect all the existing comments in the doc. Then to display comments in 
thread, it will iterate through all the comments to get its root comment using `SwAnnotationWin::GetTopReplyNote` and then create the thread by adding its replies using 
`SwPostItMgr::GetNextPostIt`. I used a hashmap (`std::unordered_map`) to keep track of all the threads and comments and to optimize this process.

There is no plan the remove the annotation margin, at least not anytime soon. So I needed to syncronize the comments in comments panel with that of annotation margin.
While working on this, I was introduced to event handling mechanism of LibreOffice. 
There are two main classes: `SfxBroadcaster` and `SfxListener`. The broadcaster has a list of listeners to which it broadcasts the message if an event happens.
Using these, I created a method `Notify` which reacts to different events, like comments being added, edited, resolved and deleted, by reflecting the same changes in the
comments panel.

Next thing to do was to add the ability to do all the actions like edit, delete, reply, resolve from the comments panel itself and to sync the comments in annotation margin.
It works basically like this: `Do some action -> Find the corresponding annotation window -> Do that action there -> Notify the comments panel -> Reflect the changes`.

Now that the comments panel was able to do the basic things, it was time to add some new features like sort and filter. For sorting, I modified the `populateComments` (which 
was responsible was collecting the existing comments and displaying it in threads) to first empty the comments panel, sort the collection of existing comments based on sorting
option selected i.e. By Position and by time) and then display it. For filter (by date or author), it was as simple as setting the visibility of widget based on the selected 
option.

Then I added context menu for the comment widget, which contains options like: Edit, Reply, Delete, Resolve, Delete Thread and Resolve Thread.

## What's left to do

- Adding connectors: While hovering over the reference text in the doc, it should show the root comment in a balloon tip.
- Creating custom widget for displaying author name with customizable colors
- Rich Text Formatting

## Conclusion

It was really fun working on this project and I got to learn a lot from it! I got to know some amazing people like my mentors Sarper and Heiko.
I'm immensely grateful to LibreOffice for giving me the opportunity to work on this project. A very big thanks to my mentor Sarper Akdemir for
guiding me on this project and for being so understanding & supportive! The comments panel would not have been possible without you and Heiko.

I wish to continue being part of the LibreOffice community and contribute to it!


## Commits

- [sw: add Comments Panel in sidebar](https://gerrit.libreoffice.org/c/core/+/167840)
- [sort comments by time or position](https://gerrit.libreoffice.org/c/core/+/170492)
- [filter by author & time](https://gerrit.libreoffice.org/c/core/+/170497)
- [display reference text on thread](https://gerrit.libreoffice.org/c/core/+/171233)
- [add context menu for comments & some other changes](https://gerrit.libreoffice.org/c/core/+/170996)
- [sw: make show option work in comments panel](https://gerrit.libreoffice.org/c/core/+/172379)
- [sw: add icons in sidebar's comment widget](https://gerrit.libreoffice.org/c/core/+/172380)
