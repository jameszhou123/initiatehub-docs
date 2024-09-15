# Content Models for Strapi

Our encyclopedia is targeted at a K-12 audience, aiming to provide educational content that is both engaging and age-appropriate. We use Strapi to organize and manage our content, which will then be exported in JSON format and packaged within our Flutter app. This approach ensures that our content is accessible offline without the need for an online database.

To align with the best practices seen in popular children's encyclopedias and optimize for offline use, we have designed a series of content models in Strapi that include all necessary data types. This structure ensures that the content is well-organized, easily navigable, and engaging for young readers. Below are the detailed content models with their respective data types, suitable for Strapi collections.

## Content Models with Data Types for Strapi

### Category Model

- ID: UUID (Unique identifier)
- Name: String (Name of the category, e.g., "Science", "History")
- Description: Text (Brief description of the category)
- Icon/Image: Media (Image file for the category icon)
- Color Theme: String (Hex color code to visually distinguish categories)
- Slug: String (URL-friendly identifier, useful for linking and organization)

### Topic Model

- ID: UUID (Unique identifier)
- Title: String (Title of the topic, e.g., "Planets")
- Category: Relation (Reference to the Category Model)
- Summary: Text (Brief overview or introduction to the topic)
- Cover Image: Media (Main image for the topic)
- Recommended Age: Integer (Suggested age range for the content)
- Featured Media: Relation (Reference to Media Asset Model, such as a primary image or video)
- Tags: Array of String (Keywords for searchability and related content suggestions)
- Slug: String (URL-friendly identifier)

### Article Model

- ID: UUID (Unique identifier)
- Title: String (Article title)
- Topic: Relation (Reference to the Topic Model)
- Content: Rich Text (Main body text, broken into sections with headings for easy reading)
- Subtopics: Array of Relation (References to other Article Models for deeper dives)
- Images/Media: Array of Relation (References to Media Asset Model for supporting visuals)
- Fact Boxes: Array of Relation (References to Fact Box Model for quick facts)
- Did You Know?: Text (Fun trivia or interesting facts to keep readers engaged)
- Audio Narration: Media (Optional audio file to support listening and accessibility)
- Interactive Elements: JSON (Links or embeds for animations or other interactive content)
- Slug: String (URL-friendly identifier)

### Media Asset Model

- ID: UUID (Unique identifier)
- Type: Enum (Image, Video, Infographic, Audio)
- File URL: Media (Path to the media file)
- Alt Text: String (Description for accessibility purposes)
- Caption: Text (Brief description or caption for the media)
- Associated Articles: Array of Relation (References to Article Models where the media is used)

### Fact Box Model

- ID: UUID (Unique identifier)
- Title: String (Title of the fact box, e.g., "Fastest Animal")
- Content: Text (Quick fact content)
- Associated Topic: Relation (Reference to the Topic Model)
- Icon: Media (Optional icon or visual aid)

### Glossary Model

- ID: UUID (Unique identifier)
- Term: String (Glossary term)
- Definition: Text (Simple, child-friendly definition)
- Related Topics: Array of Relation (References to related Topic Models)
- Pronunciation Audio: Media (Optional audio file for pronunciation to aid in learning)

### Quiz Model

- ID: UUID (Unique identifier)
- Title: String (Quiz title)
- Associated Topic: Relation (Reference to the Topic Model)
- Questions: Array of JSON (Question objects with options, correct answers, and explanations)
- Difficulty Level: Enum (Easy, Medium, Hard)
- Feedback: Text (Optional feedback for correct/incorrect answers)
- Time Limit: Integer (Optional time limit per question in seconds)

## Additional Features for Offline Usability

- Version Control: Include version fields in Article and Topic models to manage updates.
- Publish Date: DateTime field for content management and updates.
- Localization: JSON or additional fields for multi-language support.

These models are designed to provide a comprehensive, user-friendly experience for children, with offline accessibility and interactive features to enhance learning. If further customization or additional features are needed, feel free to let me know!
