# Designing the User Interface

# Part 1: From Requirements to Design

<aside>
🙀 Going to build a web application to create, list, search and manage events that are going on in Manchester

R1: Browse events by topic/keyword/venue
R2: Locate an event on the map

</aside>

After we get the requirements:

- What? Functional requirements
- How? Non-functional requirements

User stories, persona stories, use cases … are elaborated into scenarios and visual designs ahead of implementation

- Leaving details out and capturing the main idea and scope of the application
- Useful to expose flaws, misunderstandings

Same functional requirements can end up in different designs.

Visual designs are used to initiate a dialogue between the customers and the software engineering team. Visual designs are useful because it allows you to capture the main idea of the scope of the application. It allows you to know that the engineering team and the customers are on the same page whether the expectations

## From Requirements to Design

Different fidelity levels: sketches, wireframes, mockups and prototypes

Rough designs → More fine-grained designs

### Sketches

Designs on paper, paper prototypes

### Wireframes

Designing layout on the website

![Wireframes](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled.png)

Wireframes

### Mockups

Has user interface, proper layout, final touches to the last product. Can also have interactive mock-ups - click to interact etc

‘balsamic’ tool / ‘mockflow’ tool

![Untitled](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled%201.png)

### Prototypes

Custom hard-coded solutions, almost final product

![Untitled](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled%202.png)

### Time to build vs Fidelity

![Untitled](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled%203.png)

Higher the fidelity, more the time it is going to take for the final product

It is often very hard to predict how long the prototype will take or how long will it take to build the real code

## Why Mockups

- Stimulates a dialogue with the customer
    - Confirm requirements capture
    - Showing different choices
    - Exchange of ideas
- Save time
    - Prevent misunderstandings
    - Remove bugs early on
- Tips
    - Extract the tasks from requirements
    - Follow a top-bottom approach

# Guidelines for User Interface Design

## Ben Shneiderman’s 8 Golden Rules

[Ben Shneiderman](https://www.cs.umd.edu/users/ben/goldenrules.html)

1. Strive for consistency
    
    You need to make the user interface consistent and if you have different screens in the application, they should have their content located in the same regions. They should use the same fonts, colour palette and so on.
    
2. Seek universal usability
    
    Users can have different ages or different abilities (disabled), and we should recognise the needs of such diverse users. Add features for novices, and features for experts.
    
3. Offer informative feedback
    1. Say what happened and why (Error feedback)
    2. Suggest a next step
    3. Find the right tone
4. Design dialogs to yield closure
    
    If a task is finished or the workflow is done, say it or give a notification
    
5. Prevent errors
    
    Data checks and validation
    
6. Permit easy reversal of actions
    
    **Universal undo**
    
    Allow users to reverse their performed actions
    
7. Keep users in control
    
    The user interface should show the information that is requested by users, no more and no less.
    
8. Reduce short-term memory load
    
    Try to declutter, try to minimize the number of stimuli of number of controls on the window (screen)
    

### Information Overload

![Untitled](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled%204.png)

Overcrowding of icons and data, so many excessive icons etc

To fix this, we can group similar functionalities so content is disclosed on demand

![Untitled](Designing%20the%20User%20Interface%200a32dd8f94a54c3e82f81aeb098f965d/Untitled%205.png)

To solve this, split the form in smaller chunks and removing those fields that are not compulsory