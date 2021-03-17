# 1. NEd Documentation

<!-- TOC -->

- [1. NEd Documentation](#1-ned-documentation)
- [2. General Overview](#2-general-overview)
    - [2.1. “NEd 101”: A Beginner’s Guide](#21-ned-101-a-beginners-guide)
        - [2.1.1. Introduction / Main Areas](#211-introduction--main-areas)
    - [2.2. Mapping Objects](#22-mapping-objects)
        - [2.2.1. Programs](#221-programs)
        - [2.2.2. Types and subtypes](#222-types-and-subtypes)
        - [2.2.3. Tutorials:](#223-tutorials)
        - [2.2.4. How-to Guides:](#224-how-to-guides)
        - [2.2.5. Reference:](#225-reference)
        - [2.2.6. Explanation:](#226-explanation)

<!-- /TOC -->

# 2. General Overview
Giving a general overview of various topics for the purpose of broad understanding.


## 2.1. “NEd 101”: A Beginner’s Guide
### 2.1.1. Introduction / Main Areas 
NEd is the primary tool used for editing video data within Gracenote. Broadly speaking, it encompasses four main areas:

| Programs | People | Schedules | Mappings |
| -------- | -------- | -------- | -------- |

These function in slightly different ways.

- **Programs**
    - This stores information about the assets themselves (movies, episodes, series, etc.) For example, information about a movie would include all of the different versions that it has (English, Italian, director’s cut, etc), its genre, its release date, and so on.
    - Images are also stored here.
    - This is accessed within SQL by using `programs.program`.
- **People**
    - Stores information about people, which can include actors and actresses, characters, cast, and crew.
- **Schedules**
    - fsgsdfg
- **Mappings**


## 2.2. Mapping Objects
NEd

- **programs**			
    - stores all program information including images	
- **common**		
    - stores information that is common between records ie. Language, country 
- **showtimes**		
    - stores VOD mapping schemes and data	
- **sources**			
    - stores all source information (channel)	
- **interface_db**		
    - stores schedule information (channel day)




### 2.2.1. Programs

### 2.2.2. Types and subtypes
Within the Video department, we have different categories of data, which are referred to as Types. These can primarily be categorised as the following:

- Movies
- Series
- Episodes
- Sports

Within each of these categories, there are further Subtypes. For instance, Movies could be subtyped as one of the following:

- Feature Film
- Short Movie
- TV Movie




- Different types (Movies, Series etc)
- Different image types
- Different IDs: program IDs, how they relate to TMS IDs, etc
- Program structure (what fields are used by Content and which are commonly requested)
- Remote metadata vs TMS data (Mapping scheme)
- Link between programs and schedule data 
- Common Content dictionary (what they talk about when they talk about ‘mapping scheme’?) 
- Typical reports (OVD, Linear, VOD),  metadata + image, different templates


> How should online reference material be structured?
> We could use some of the principles outlined here, and break it principally into four broad areas (note that we may not need all three):

### 2.2.3. Tutorials:
- Learning-oriented - emphasis is on _how_ to do something.
- It is a lesson which allows newcomers to get started and **accomplish something specific from start to finish** (no matter how small).

### 2.2.4. How-to Guides:
- Problem-oriented.
- **A specific series of steps which shows how to solve a particular problem**. The person using the how-to guide has a specific goal in mind (such as writing SQL that will fetch program genres) and the guide will take them clearly and linearly through that process, much like a recipe in a cookery book.

### 2.2.5. Reference:
- Information-oriented
- This is purely to convey information on an as-needed basis, and is intended as a reference guide. For example, if we frequently use lambdas in Python for specific purposes, we might want to document this so that we can quickly and efficiently locate that information when we need it (and not before).

### 2.2.6. Explanation:
- Understanding-oriented.
- It is broadly educational and the emphasis is on developing a general understanding; for example, a general explanatory overview of NEd as a whole.

general concepts
ID types
data objects
