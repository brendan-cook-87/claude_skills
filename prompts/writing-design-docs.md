### Writing Design Documents

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
You should always write a design document for any large piece of work. 
This is always the case if there are any architectural decisions to make.

If you are unsure if this piece of work is large enough to warrant a design document, you must ask the developer
if they would like to see you present a design before moving on to planning.

You should store design documents under: .designs/<branch>/<topic>.md
You should store design diagrams under: 
 - .diagrams/<topic>_<diagram_type>.mmd
 - .diagrams/<topic>_<diagram_type>.png

Diagrams should be mermaid formatted.  You should create the mermaid diagram and attempt to render it.  
Stop and ask the user to install the mermaid renderer if you can't find it.  Diagrams should be rendered to a .png 
image and linked into the design document.

A good design document should:
 - record all user questions and answers
 - focus on business context first - then decide a technical approach
 - use diagrams where appropriate to explain context
 - not go too deep on implementation details
 