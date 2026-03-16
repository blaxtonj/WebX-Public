# Architecture Overview

This document details the architecture of **webx**, highlighting
design principles, folder structure, and key patterns used
to ensure maintainability, performance, and scalability.

## High-Level Structure

```
webx/
├─ app/             
│  ├─ (hero)
│  │  ├─ Hero.tsx          
│  ├─ (services)
│  │  ├─ Audit.tsx         
│  │  ├─ ExploreComps.tsx  
|  |  └─ Services.tsx  
│  ├─ (contact)
|  |  └─ Contact.tsx
│  ├─ components/         # Reusable building blocks organized into (comps) and (sections)
│  │  ├─ (comps)          # general UI components
│  │   ├─ buttons/
│  │   ├─ cards/
│  │   └─ ...etc
|  |  ├─ (sections)       # page-specific components
|  |   ├─ faq/
|  |   ├─ hero/
|  |   └─ ...etc
|  |  
│  └─ api/                 # API route for form submission 
│     └─ submit
|      └─ route.ts         
├─ UI/                     # Reusable UI components
├─ data/                   # Static configuration & JSON data
├─ img/                    # Static images
└─ utils/                  # Utility functions
```


## Routing & Route Groups

Route Groups: Folders wrapped in parentheses, like (hero), (services), and (contact), are used purely for organization and do not affect the URL path.

Pages inside groups:

- (hero)/Hero.tsx → /

- (services)/services.tsx → /

- (contact)/Contact.tsx → /

All three pages are currently rendered at the root path /, while the folders (services) and (contact) exist only to keep the project organized.

Components Routing: The subfolders under components/ are organized similarly:

- (comps)/buttons → /components/buttons

- (sections)/faq → /components/faq

This avoids unnecessary URL nesting like /components/sections/faq and keeps paths clean

## Purpose of ```components/```

The ```components/```section functions as a public component library and interactive playground. It showcases multiple variants of commonly used UI patterns in modern web development, ranging from simple elements such as buttons to more complex components like forms, navigation systems, and interactive widgets.

Each component includes:

-  live interactive demo that users can experiment with

- syntax-highlighted code example using Highlight.js

- ability to copy and reuse the implementation

In addition to atomic UI elements, the section also includes complete page sections (such as hero sections, FAQs, and other layout blocks). These examples focus heavily on animation systems and interactive UI patterns, demonstrating how modern motion libraries and component composition can be used to create engaging user interfaces.

- Components are organized into two primary groups:

- (comps) — reusable UI components (buttons, cards, forms, etc.)

- (sections) — larger page sections composed of multiple components

During development, I initially considered using Next.js parallel routes to structure this section. However, I ultimately chose route groups, which provided a simpler and more maintainable routing structure while still allowing components to be organized logically.

The goal of this section is to provide ready-to-use UI patterns while also serving as a technical sandbox for experimenting with animations, layout systems, and component architecture.
