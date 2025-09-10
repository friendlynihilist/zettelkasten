## Annotations example

Essentially a JSON object with a target (e.g. coordinates) and a body (the actual content of the annotation; e.g. simple text, metadata etc.):

```json
{
  "@context": "http://iiif.io/api/presentation/3/context.json",
  "id": "https://example.com/annotation/1",
  "type": "Annotation",
  "motivation": ["commenting"],
  "body": {
    "type": "TextualBody",
    "value": "This is an important paragraph in the manuscript.",
    "format": "text/plain"
    
  },
  "frbr": "",
  "target": {
    "source": "https://example.com/manifest/canvas/page1",
    "selector": {
      "type": "FragmentSelector",
      "conformsTo": "http://www.w3.org/TR/media-frags/",
      "value": "xywh=100,200,300,150" // Specific region coordinates on the canvas/page
    }
  }
}
```

A IIIF (International Image Interoperability Framework) manifest is a JSON document that describes the structure, metadata, sequences, canvases, annotations, and relationships of digital resources. It provides a standardized way to represent and share information about cultural heritage materials, manuscripts, images, and other media types. Here are the main components of a IIIF manifest:

1. **`@context`:** This specifies the context for interpreting the JSON-LD content and defines the IIIF Presentation API, such as its version and structure.
   - Example: `"@context": "http://iiif.io/api/presentation/3/context.json"`

2. **`id`:** A unique URI that identifies the manifest itself.
   - Example: `"id": "https://example.com/manifests/manifest1.json"`

3. **`type`:** Indicates that the JSON document is a IIIF manifest.
   - Example: `"type": "Manifest"`

4. **`label`:** Descriptive label or title for the resource represented by the manifest.
   - Example: `"label": "Example Manifest"`

5. **`metadata`:** Array containing metadata describing the resource, including information like title, creator, description, date, and other descriptive details.
   - Example:
     ```json
     "metadata": [
       {
         "label": "Title",
         "value": "Example Title"
       },
       {
         "label": "Creator",
         "value": "John Doe"
       },
       // Additional metadata fields...
     ]
     ```

6. **`sequences`:** An array of sequences representing different orderings or views of the canvases (images, pages, or resources). Each sequence contains information about canvases or other content associated with the resource.
   - Example:
     ```json
     "sequences": [
       {
         "id": "https://example.com/manifests/sequence1.json",
         "type": "Sequence",
         // Sequence details, including canvases or content...
       }
       // Additional sequences...
     ]
     ```

7. **`canvases`:** Within a sequence, canvases represent individual images, pages, or resources and contain information about the content, annotations, and relationships.
   - Example:
     ```json
     "canvases": [
       {
         "id": "https://example.com/manifests/canvas1.json",
         "type": "Canvas",
         "label": "Page 1",
         // Canvas details, annotations, and content...
       }
       // Additional canvases...
     ]
     ```

8. **`annotations`:** Annotations associated with the canvases or content, providing additional descriptive, interpretive, or interactive information linked to specific regions or parts of the resources.
   - Example:
     ```json
     "annotations": [
       {
         "id": "https://example.com/annotation/annotation1.json",
         "type": "Annotation",
         // Annotation details...
       }
       // Additional annotations...
     ]
     ```

These components help structure and organize the information within a IIIF manifest, allowing for the representation of complex digital resources, their relationships, metadata, and annotations in a standardized and interoperable format. This structure enables IIIF-compliant viewers and applications to interpret and display the resources in a consistent and user-friendly manner.

Here's a complete example:

```json
{
  "@context": "http://iiif.io/api/presentation/3/context.json",
  "id": "https://example.com/manifests/manifest1.json",
  "type": "Manifest",
  "label": "Example Manuscript Manifest",
  "metadata": [
    {
      "label": "Title",
      "value": "Example Manuscript"
    },
    {
      "label": "Creator",
      "value": "John Doe"
    }
  ],
  "sequences": [
    {
      "id": "https://example.com/manifests/sequence1.json",
      "type": "Sequence",
      "label": "Default Sequence",
      "canvases": [
        {
          "id": "https://example.com/manifests/canvas1.json",
          "type": "Canvas",
          "label": "Page 1",
          "width": 800,
          "height": 600,
          "annotations": [
            {
              "id": "https://example.com/annotations/annotation1.json",
              "type": "Annotation",
              "motivation": "commenting",
              "body": {
                "type": "TextualBody",
                "value": "This is an annotation on Page 1",
                "format": "text/plain"
              },
              "target":
              {
                "source": "https://example.com/manifests/canvas1.json",
                "hasLevel": "https://example.com/annotations/annotation1.json"
                "selector": {
                  "type": "FragmentSelector",
                  "value": "xywh=100,100,200,200"
                }
              }
            }
          ]
        },
        {
          "id": "https://example.com/manifests/canvas2.json",
          "type": "Canvas",
          "label": "Page 2",
          // Canvas details for Page 2...
        }
      ]
    }
  ]
}
```

## Annotation workflow

In many IIIF implementations, annotations are often created or generated within the front-end viewer applications (such as Mirador) by users or scholars interacting with the displayed digital resources. These annotations are then stored or persisted in a separate annotation store or service and linked to the IIIF manifests.

Here's a typical workflow:

1. **Annotation Creation in the Viewer:** Users interact with IIIF-compliant viewers like Mirador, where they have tools or functionalities allowing them to create annotations. These annotations can include comments, transcriptions, tags, links to related resources, or other types of descriptive or interpretive content linked to specific regions or parts of the displayed digital resources (such as images or manuscripts).

2. **Storage in Annotation Service:** The annotations created within the viewer are sent to an annotation service or server, which manages and stores these annotations as separate resources in a structured manner. The annotation service adheres to IIIF specifications and APIs, enabling the creation, retrieval, and manipulation of annotations.

3. **Linkage to Manifest:** Once the annotations are stored in the annotation service, they are associated or linked to the relevant canvases or parts of the digital resources described in the IIIF manifests. This linkage is achieved through the inclusion of annotation URIs or references within the manifest's annotation lists or structures.

4. **Access via IIIF Manifest:** IIIF-compliant viewers or applications, when rendering the digital resources using the manifests, can retrieve and display annotations associated with those resources by querying the annotation service using the annotation URIs or references specified within the manifest.

So, while the creation of annotations often happens in the front-end viewer, the actual storage, management, and linking of annotations to the IIIF manifests are handled by separate annotation services or servers, ensuring that annotations can be accessed, shared, and presented in a standardized and interoperable manner across different IIIF-compliant viewers or applications.

## Annotations storage

In the context of IIIF, annotations are typically stored separately from the digital resources they annotate. IIIF-compliant servers or annotation services manage and store annotations as independent objects or datasets associated with the digital resources.

The storage location or method for annotations can vary depending on the implementation and the specific IIIF-compliant platform or service being used. However, here are some common approaches to annotation storage:

1. **External Annotation Servers or Services:** Annotations might be stored in dedicated annotation servers or services that support IIIF specifications. These servers manage annotations as separate resources and provide APIs to create, retrieve, update, and delete annotations associated with digital resources.

2. **Embedded within IIIF Manifests:** IIIF manifests, which describe the structure and metadata of digital resources, can include annotations as part of their structure. Annotations might be embedded within the manifest itself, enabling a self-contained representation of the resource along with its associated annotations.

3. **Annotation Stores or Databases:** Some implementations use databases or stores specifically designed to manage annotations related to IIIF resources. These stores organize annotations based on the targets (regions or parts of the digital resources) they annotate and provide querying capabilities to retrieve annotations as needed.

4. **Linked Data and Web Services:** IIIF encourages the use of linked data principles, allowing annotations to be stored as linked resources, connected via URIs and accessed through HTTP requests. Annotations might be stored as separate JSON-LD documents or resources with unique identifiers accessible via URLs.

In general, IIIF-compliant systems maintain a clear separation between the digital resources (images, manuscripts, etc.) and the annotations associated with them. Annotations are stored in a way that allows them to be queried, retrieved, shared, and linked to the relevant parts of the digital resources they annotate. The specific storage mechanism and infrastructure used to manage annotations can vary based on the implementation and requirements of the IIIF-enabled platform or service.