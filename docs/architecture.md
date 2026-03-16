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
