# Backend Technical Specs


# Business Goal: 
A language learning school wants to build a prototype of learning portal which will act as three things:
- Inventory of possible vocabulary that can be learned
- Act as a  Learning record store (LRS), providing correct and wrong score on practice vocabulary
- A unified launchpad to launch different learning apps

## Technical Requirements

- The backend is to be built using FastAPI
- The frontend is to be built using 
- The database is to be built using PostgreSQL
- The API will allways return JSON

Here is the **database schema** with precise **foreign key (FK) references**:

---

## **Database Schema**

We have the following tables:

### **words**  
Stores information about words in different formats.  
- `id` (int, PK) - Unique identifier for the word.  
- `kanji` (string) - Kanji representation.  
- `romaji` (string) - Romaji representation.  
- `english` (string) - English translation.  
- `parts` (json) - Additional information about the word.  

---

### **groups**  
Represents a group of words.  
- `id` (int, PK) - Unique identifier for the group.  
- `name` (string) - Name of the group.  
- `words_count` (int) - Number of words in the group.  

---

### **word_groups** (join table)  
Intermediate table for the **many-to-many** relationship between `words` and `groups`.  
- `word_id` (int, FK) - References `words.id`.  
- `group_id` (int, FK) - References `groups.id`.  

---

### **study_activities**  
List of available study activities.  
- `id` (int, PK) - Unique identifier for the study activity.  
- `name` (string) - Name of the activity.  
- `url` (string) - URL associated with the activity.  

---

### **study_sessions**  
Represents a study session linked to a group and a study activity.  
- `id` (int, PK) - Unique identifier for the session.  
- `group_id` (int, FK) - References `groups.id` (group being studied in the session).  
- `study_activity_id` (int, FK) - References `study_activities.id` (study activity performed).  
- `created_at` (timestamp) - Date and time the session was created.  

---

### **word_review_items**  
Records word reviews within a study session.  
- `id` (int, PK) - Unique identifier for the review item.  
- `word_id` (int, FK) - References `words.id` (word being reviewed).  
- `study_session_id` (int, FK) - References `study_sessions.id` (session in which the word was reviewed).  
- `correct` (boolean) - Indicates whether the response was correct.  
- `created_at` (timestamp) - Date and time of the review.  

---

### **Key Relationships**  
1. `words` and `groups` have a **many-to-many** relationship through `word_groups`.  
2. `study_sessions` is linked to a `group` and a `study_activity`.  
3. `word_review_items` connects `words` with `study_sessions` to track reviews.  


### API Endpoints

- `GET /words` - Retrieve a list of all words
- `GET /words/{word_id}` - Retrieve details of a specific word
- `GET /groups` - Retrieve a list of all groups
- `GET /groups/{group_id}` - Retrieve details of a specific group
- `GET /study-activities` - Retrieve a list of all study activities
- `GET /study-activities/{study_activity_id}` - Retrieve details of a specific study activity
- `POST /study-sessions` - Create a new study session
- `GET /study-sessions/{study_session_id}` - Retrieve details of a specific study session
- `POST /word-reviews` - Create a new word review
- `GET /word-reviews/{word_review_id}` - Retrieve details of a specific word review
