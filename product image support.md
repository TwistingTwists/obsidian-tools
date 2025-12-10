- bugs / corner cuts  
	- avatar image and background image have special handling on the hydrate template page 
	- upload assets for a product -- save the against product_id so that they can be fetched later. 
	- ```
	  **Workflow:** 1) Hydrate Template (first frames + ElevenLabs audio) → 2) Generate Scene Videos (Veo3 clips) → 3) Cut & Combine (final video) 
	  
	  update this 
	  ```
- hydrate button disabled untilt he product image is uploaded 
- regenerate first frame -- fix that to regenerate with the PRODUCT IMAGE 
- if hydration fails for one image 
	- [ ]  collect that error and return back as a part of api response
	- [ ] retry for the hydrate part 
- coin comparison asset possible? -- for the sizing issue in the template
- 
- does hydrate part and the 'generate first frame' share same method implemented???
- [x] why is the model gemini 2.5 pro being used on prod?? -- we are using constant , so should not be happening
- [ ] new experiemtns 
	- [ ] kling o1 - image edit -  first frame gen model 
		- [ ] api integration fal 
		- [ ] ugc_dashboard integration 
	- [ ] kling 2.6 - video generation model 
		- [ ] api integration fal 
- [ ] SCENE DEPENDENCY DIAGRAMS
	- [ ] for first frame generation -- choose the model from the dropdown -- and pass that to the 
- [ ] scene system prompt -- remove dialogue part that user is speaking-- for the user not speaking and background tasks. 
 - [ ] sceneflow update to 0.2.4 
 - [ ] 

----

MORE CUSTOMISATIONS:
1. first frame and last frame support -- in the config -- via the scene dependency system -
	1. to generate consistent end and first frames 
	2. to use the same first frame from scene 1 and change camera angles.
2. Existing remotion templates -- use those to 


```
   Current State Summary

   Backend (`template_hydration.py`)

   Already supports product_image in dependencies:
   1. DependencyConfigInput has a type field (e.g., "product_image", "product_video", "logo")
   2. resolve_scene_assets() includes type in the resolved assets payload
   3. generate_for_group() extracts product_image_url from dependencies with type == "product_image" and passes it to _generate_combined_avatar_background_frame()
   4. _generate_combined_avatar_background_frame() already accepts and uses product_image_url parameter

   Frontend - Hydrate Tab (`page.tsx`)

   Already implemented:
   •  Dependencies upload UI grouped by type
   •  File upload handler (handleDependencyFileUpload)
   •  Passes runtimeAssets.dependencies to hydration API

   Frontend - Make Template Tab (`page.tsx`)

   NOT YET IMPLEMENTED:
   •  No UI to define/edit the dependencies array at collection level
   •  No UI to specify which scenes use which dependencies (scene-level dependencies: string[])

   ──────────────────────────────────────────

   Implementation Plan for Make Template Tab

   What's Missing

   1. Collection-Level Dependencies Editor

   Add UI in TemplateInfoCard (or separate card) to define dependency slots:

   typescript
     dependencies: [
       { type: "product_image", assetKey: "main_product", min: 1, max: 1 },
       { type: "logo", assetKey: "brand_logo", min: 0, max: 1 },
     ]

   2. Scene-Level Dependency Selection

   In SceneConfigCard, add ability to select which dependencies a scene uses:

   typescript
     scene.dependencies: ["main_product", "brand_logo"]  // assetKeys

   ──────────────────────────────────────────

   Frontend Changes Required

   A. Add Dependencies Editor Component (new)

   typescript
     // New component: DependencyConfigCard.tsx
     // - Add/remove dependency slots
     // - Set type (dropdown: product_image, product_video, logo, etc.)
     // - Set assetKey (unique identifier)
     // - Set min/max requirements (0 or 1 typically)

   B. Update TemplateInfoCard

   Add section after Avatars/Backgrounds:

   tsx
     <DependencyConfigCard
       dependencies={formData.dependencies}
       onChange={(deps) => setFormData(p => ({ ...p, dependencies: deps }))}
     />

   C. Update SceneConfigCard

   Add multi-select for dependencies:

   tsx
     // In scene editor, add:
     <label>Dependencies (Product Images, etc.)</label>
     <MultiSelect
       options={formData.dependencies.map(d => ({ value: d.assetKey, label: `${d.assetKey} (${d.type})` }))}
       value={scene.dependencies}
       onChange={(deps) => onUpdate({ ...scene, dependencies: deps })}
     />

   D. Update `handleGenerateSceneFirstFrame`

   Currently only uses avatar/background images. Update to include product image:

   typescript
     // In handleGenerateSceneFirstFrame:
     // 1. Find dependency with type="product_image" in scene.dependencies
     // 2. Look up generated/uploaded image for that dependency
     // 3. Pass as referenceImages to generateFirstFrame API

   ──────────────────────────────────────────

   Backend Changes (Minor)

   `POST /video-template-collections/scenes/generate-first-frame`

   The API already accepts referenceImages, but for explicit product image support:
   •  Option A: Use existing referenceImages (works today)
   •  Option B: Add explicit productImage?: S3Reference field to GenerateFirstFrameRequest

   Currently the backend hydration flow uses product_image_url explicitly - the stateless "Make Template" endpoint could be updated similarly.



   Type Definitions (already exists)

   typescript
     // In videoTemplateCollectionApi.ts - already defined:
     export interface DependencyConfigInput {
       type: string;        // "product_image", "product_video", "logo"
       assetKey: string;    // unique identifier like "main_product"
       min: number;         // 0 = optional, 1 = required
       max: number;         // typically 1
     }

   ──────────────────────────────────────────

   Summary

   Area                                            │ Status      │ Work Needed
   ------------------------------------------------+-------------+----------------------------------------------------------------
   Backend hydration                               │ **Done**    │ Already extracts `product_image` from dependencies
   Backend generate-first-frame                    │ **Partial** │ Can use `referenceImages`, or add explicit `productImage` param
   Frontend Hydrate Tab                            │ **Done**    │ Uploads dependencies and passes to API
   Frontend Make Template - Dependencies Editor    │ **Missing** │ Need UI to define `dependencies[]`
   Frontend Make Template - Scene Dependencies     │ **Missing** │ Need UI to select which deps a scene uses
   Frontend Make Template - First Frame Generation │ **Missing** │ Need to include product image in `referenceImages`

   Would you like me to proceed with implementing these frontend changes?

>  go ahead and do t
```



```
[{"max": 1, "min": 1, "type": "product_image", "asset_key": "product_1"}]
```