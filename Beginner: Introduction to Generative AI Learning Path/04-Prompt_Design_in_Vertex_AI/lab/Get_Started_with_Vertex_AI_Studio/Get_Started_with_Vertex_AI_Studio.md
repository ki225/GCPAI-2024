# Get Started with Vertex AI Studio

## learning goal
- Analyze images with Gemini
- Explore Vertex AI Studio Freeform mode
- Design text prompts for zero-shot, one-shot, and few-shot prompting
- Generate conversations with chat prompts




## Task

### Task 1. Analyze images with Gemini in Freeform mode
Enable the Vertex AI API


![](img/img3.png)
![](img/img4.png)
![](img/img5.png)

Analyze images with Gemini

![](img/img6.png)
![](img/img7.png)


The UI contains three main sections:

- Prompt (located in the center): Here, you can create a prompt that utilizes multimodal capabilities.
- Configuration (located on the right): This section allows you to select models, configure parameters, and obtain the corresponding code.
- Response (located at the bottom): This section displays the results of your prompt.

![](img/img8.png)

#### project: Image Analysis
![](img/img9.png)
![](img/img10.png)
![](img/img11.png)
![](img/img12.png)


#### prompt design practice
![](img/img13.png)
![](img/img14.png)
![](img/img15.png)
![](img/img16.png)
![](img/img17.png)
![](img/img18.png)
![](img/img19.png)
![](img/img20.png)


### Task 2. Explore multimodal capabilities
![](img/img21.png)
![](img/img22.png)
![](img/img23.png)
![](img/img24.png)
![](img/img25.png)
![](img/img26.png)
![](img/img27.png)

Replace <Your-Cloud-Storage-Bucket> with the bucket name you copied earlier.
```
gcloud storage cp gs://spls/gsp154/video/train.mp4 gs://<Your-Cloud-Storage-Bucket>
```


![](img/img28.png)
![](img/img29.png)
![](img/img30.png)
![](img/img31.png)
![](img/img32.png)


### Task 3. Design text prompts
![](img/img33.png)


#### Prompt design
There are three main methods to design prompts:

- Zero-shot prompting - This is a method where the LLM is given only a prompt that describes the task and no additional data. For example, if you want the LLM to answer a question, you just prompt "what is prompt design?".
- One-shot prompting - This is a method where the LLM is given a single example of the task that it is being asked to perform. For example, if you want the LLM to write a poem, you might give it a single example poem.
- Few-shot prompting - This is a method where the LLM is given a small number of examples of the task that it is being asked to perform. For example, if you want the LLM to write a news article, you might give it a few news articles to read.

### Parameters
- Temperature
  - controls the randomness in token selection. A lower temperature is good when you expect a true or correct response. A temperature of 0 means the highest probability token is always selected. A higher temperature can lead to diverse, unexpected, or potentially biased results. The gemini-1.5-pro model has a temperature range of 0 - 2 and a default of 1.
- Output token limit
  - determines the maximum amount of text output from one prompt. A token is approximately four characters.


### Zero-shot prompting Lab
The model will respond to a comprehensive definition of the term prompt gallery. For example:
```
A prompt gallery is a collection of pre-written prompts designed to inspire and guide users in various creative activities. These activities can range from writing and art creation to problem-solving and brainstorming. Essentially, it's a curated library of ideas presented as starting points for generating something new.

Here's a breakdown of key features and uses:

Purpose: To overcome creative blocks, explore new ideas, practice specific skills, or simply provide a starting point when you don't know where to begin.

Content: Prompts can take many forms, including:

Questions: "What if animals could talk? What would they say?"
Statements: "Write a story about a lost key."
Images: A picture of a sunset could prompt a poem, story, or painting.
Keywords: Words like "mystery," "adventure," or "joy" can spark ideas.
Combinations: Mixing and matching elements from different prompts.
Target Audience: Prompt galleries can cater to different groups, such as writers, artists, game designers, educators, or anyone looking for creative inspiration.

Platform: Prompt galleries can be found online as websites, social media groups, or within creative software. They can also exist offline as books or card decks.

Benefits:

Accessibility: Provides easy access to a wealth of ideas.
Diversity: Offers a variety of prompts across different genres and styles.
Structure: Can help guide the creative process and provide focus.
Inspiration: Sparks new ideas and helps overcome creative blocks.
Skill Development: Can be used to practice specific techniques or explore new creative avenues.
Examples of where you might find prompt galleries:

Writing websites: Many websites offer writing prompts for fiction, non-fiction, poetry, and screenwriting.
Art communities: Online platforms or forums for artists often share prompts for drawing, painting, or sculpting.
Educational resources: Teachers might use prompt galleries to inspire student writing or classroom discussions.
Creative software: Some software includes built-in prompt generators or galleries to help users get started.
In essence, a prompt gallery is a valuable tool for anyone looking to boost their creativity, explore new ideas, or simply have some fun.
```

Adjust the Output Token limit parameter to 1024 and click the SUBMIT button.
```
A prompt gallery is a collection of pre-written prompts, often categorized, designed to inspire or assist users in various creative activities. These activities could include:

AI art generation: Prompt galleries provide various textual descriptions to guide AI image generators like Midjourney, DALL-E 2, Stable Diffusion, etc., towards generating specific visuals. These prompts can range from simple object descriptions to complex scenes with specific art styles, lighting, and composition.
Writing: Prompt galleries can offer story starters, character ideas, dialogue prompts, poetry prompts, or even blog post ideas for writers experiencing writer's block or seeking fresh inspiration.
Roleplaying: For tabletop RPGs or online roleplaying, prompt galleries can provide scenario hooks, character backgrounds, quest ideas, or random encounter generators.
Brainstorming/Ideation: Prompt galleries can offer a structured way to brainstorm new ideas for projects, products, or problem-solving, offering different perspectives and angles to consider.
Learning/Education: Prompt galleries can be used by educators to spark discussions, encourage critical thinking, or provide starting points for research projects.
Essentially, a prompt gallery serves as a resource for jumpstarting creativity or providing direction when tackling a task that requires imaginative input. They can be found online on dedicated websites, within AI art generator communities, or even as personal collections built by individuals.
```

Adjust the Temperature parameter to 0.5 and click the SUBMIT button.
```
A prompt gallery is a collection of prompts, typically for generative AI models like large language models (LLMs) or image generation AI. They serve as inspiration and starting points for users who want to create content but might be struggling with what to ask the AI to generate.

Here's a breakdown of key aspects:

Purpose: To provide examples of effective prompts, showcase the capabilities of the AI, and help users understand how different phrasing can influence the output.
Content: Prompts can range from simple to complex, covering various topics and styles. They might include keywords, specific instructions, or even full paragraphs describing the desired output. For image generation, they might include art styles, subject matter, and composition details.
Platform: Prompt galleries can exist on various platforms, including:
AI tool websites: Many AI tools host their own galleries to help users get started.
Community forums and websites: Users often share and discuss effective prompts in online communities.
Personal collections: Individuals might maintain their own collections of prompts for future use.
Benefits:
Overcome creative blocks: Provide inspiration when users are stuck.
Learn effective prompting techniques: By studying existing prompts, users can learn how to structure their own requests for better results.
Discover new possibilities: Explore different styles and applications of the AI.
Save time and effort: Start with a pre-made prompt and modify it as needed, rather than starting from scratch.
Essentially, prompt galleries are valuable resources for anyone working with generative AI, offering a convenient way to explore, learn, and create.
```

Adjust the Temperature parameter to 2.0 and click the SUBMIT button.
```
A prompt gallery is a collection of pre-written prompts, often categorized, designed to inspire or assist users in various creative activities. These activities could include:

Writing: Story ideas, character sketches, poetry prompts, script outlines, etc.
Art: Drawing ideas, painting prompts, sculpting suggestions, digital art concepts, etc.
Brainstorming and Ideation: Prompts to stimulate thinking for problem-solving, innovation, or project development.
Journaling: Self-reflection questions, daily writing prompts, gratitude prompts, etc.
Gaming: Ideas for game scenarios, character backstories, world-building, etc.
AI Art Generation: Specific and descriptive phrases used to guide AI image generators like Midjourney, DALL-E 2, Stable Diffusion, etc., to create desired outputs.
Prompt galleries can be found:

Online: Websites, blogs, social media groups, and within AI art generation platforms often feature extensive libraries of prompts.
In Books: Creative writing and art instruction books sometimes include prompt sections.
Software: Some writing or art software may include built-in prompt generators or galleries.
The purpose of a prompt gallery is to overcome creative blocks, explore new ideas, offer different perspectives, and provide a starting point for a project. They can be particularly useful for beginners or anyone looking for a spark of inspiration.
```

### One-shot prompting Lab
design prompts in more organized ways, e.g. providing Context and Examples in their respective input fields

![](img/img35.png)
![](img/img36.png)
![](img/img37.png)

![](img/img39.png)


![](img/img40.png)
![](img/img41.png)

![](img/img42.png)
![](img/img43.png)


### Few-shot prompting Lab
use the model to perform sentiment analysis on a sentence, such as determining whether a movie review is positive or negative using few-shot prompting.

![](img/img44.png)
![](img/img45.png)


## Task 4. Generate conversations
add context to the chat and let the model respond based on the context provided.

![](img/img47.png)
![](img/img48.png)
![](img/img49.png)
![](img/img50.png)
![](img/img51.png)
![](img/img52.png)


