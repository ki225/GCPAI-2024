```
git clone --depth=1 https://github.com/GoogleCloudPlatform/training-data-analyst
```
###  helper functions to complete the tasks
- For Inspect the processed text metadata:
  - text: the original text from the page
  - text_embedding_page: the embedding of the original text from the page
  - chunk_text: the original text divided into smaller chunks
  - chunk_number: the index of each text chunk
  - text_embedding_chunk: the embedding of each text chunk
- For Inspect the processed image metadata:
  - img_desc: Gemini-generated textual description of the image.
  - mm_embedding_from_text_desc_and_img: Combined embedding of image and its description, capturing both visual and textual information.
  - mm_embedding_from_img_only: Image embedding without description, for comparison with description-based analysis.
  - text_embedding_from_image_description: Separate text embedding of the generated description, enabling textual analysis and comparison.
- For Import the helper functions to implement RAG:
  - get_similar_text_from_query(): Given a text query, finds text from the document which are relevant, using cosine similarity algorithm. It uses text embeddings from the metadata to compute and the results can be filtered by top score, page/chunk number, or embedding size.
  - print_text_to_text_citation(): Prints the source (citation) and details of the retrieved text from the get_similar_text_from_query() function.
  - get_similar_image_from_query(): Given an image path or an image, finds images from the document which are relevant. It uses image embeddings from the metadata.
  - print_text_to_image_citation(): Prints the source (citation) and the details of retrieved images from the `get_similar_image_from_query()`` function.
  - get_gemini_response(): Interacts with a Gemini model to answer questions based on a combination of text and image inputs.
  - display_images(): Displays a series of images provided as paths or PIL Image objects.
