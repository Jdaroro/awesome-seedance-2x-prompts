# 🎬 Dreamina Seedance 2.5 Prompt Guide & Skill

> [!NOTE]
> Third-party official documentation snapshot for convenient reading. Copyright remains with the original rights holder; this file is not covered by the repository's MIT License. See the [canonical BytePlus page](https://docs.byteplus.com/en/docs/ModelArk/2607689) for the latest version.

This topic introduces prompting methods and techniques for Dreamina Seedance 2.5 (hereafter referred to as Seedance 2.5), helping you generate high\-quality videos that match your requirements more efficiently.

<span id="skill"></span>
## Get the skill

We strongly recommend using the Seedance 2.5 Skill to optimize your prompts.


1. Install it in your local project with NPX:

   ```Bash
   npx --yes skills@latest add \
     "https://arkdocs-en.tos-ap-southeast-1.volces.com/skills/" \
     --skill sd25-pe \
     --yes
   ```
   
2. In an AI chat box, enter `/sd25-pe + your prompt` to start optimizing the prompt.


<span id="intro"></span>
## Overall introduction

Seedance 2.5 can generate a single video up to **30 seconds** long and accept up to **50 image, audio, and video reference assets** in one request. It provides stronger instruction following, professional video editing and extension controls, and native generation in **more than 10 languages** . These upgrades advance video generation toward production\-ready workflows built around **long\-form storytelling, rich references, precise editing, and multilingual creation** .

Creative quality also improves significantly. More realistic visuals, lighting, performance, and camera movement make results feel closer to live\-action footage. Seedance 2.5 gives professional creators and enterprise teams a faster, more controllable, and more scalable video production workflow.

<span id="multimodal-capabilities"></span>
### Typical multimodal video generation capabilities

> Seedance 2.5 supports flexible combinations of multimodal inputs such as text, images, video and audio. The following table only lists some typical capabilities. More usage methods can be explored based on actual scenarios.



<span aceTableMode="list" aceTableWidth="1,2,3"></span>
| **Task type**             | **R2V tasks supported by Seedance 2.5**                      | **Detailed description of capabilities**                     |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Reference**             | **Subject reference** \- References the subject's appearance identity and/or voice, such as a person, object, scene, or virtual character. | * Subject image reference<br><br>* Subject audio and video reference<br><br>* Subject image + audio reference |
|                           | **Motion reference** \- References motion and dynamic information from videos. | * Action/expression/camera movement/creativity/effects, and more<br><br>* Motion + subject reference |
|                           | **3D clay\-model reference/rendering** \- Uses coarse\-grained or fine\-grained 3D clay\-model videos as motion references and renders them into the target visual style. | * 3D clay\-model reference<br><br>* 3D clay\-model reference + subject reference<br><br>* 3D clay\-model reference + subject reference + scene reference |
|                           | **Style reference** \- References the visual style of images or videos. | * Style image/video reference<br><br>* Style image/video reference + subject reference |
|                           | **Audio reference** \- References audio information such as music, dialogue, voice, tone, or timbre. | * Audio (music/melody/dialogue/voice) reference<br><br>* Audio + subject reference |
|                           | **Storyboard reference** \- References storyboard information such as subjects, composition, actions, plot, and scene progression. | * Storyboard reference<br><br>* Storyboard + subject reference |
|                           | **Keyframe reference** \- Uses one or more images as keyframes to generate a video. | * Multiple keyframes reference<br><br>* First/last keyframes reference |
| **First and last frames** | **First\-frame/first\-and\-last\-frame video generation** \- Generates a video from a single first\-frame image or from two images used as the first and last frames. | Strictly control this through `content.role = first_frame/last_frame`. |
| **Editing**               | **Video instruction editing** \- Uses text instructions to add, remove, or modify visual elements in a video, with support for timestamps to specify when edits should take effect. | * Add: Add subjects, costumes, camera movements, special effects, and more.<br><br>* Modify: Modify the subject, parts of the subject, style, background, color, lighting, material, motion, camera position, and more.<br><br>* Remove: Remove subjects, subtitles, watermarks, and more. |
|                           | **Video editing with reference images** \- Uses text instructions plus reference images to add, remove, or modify visual elements in a video, with support for timestamps to specify when edits should take effect. |                                                              |
|                           | **Audio editing** \- Adds, removes, or modifies audio in video. | * Add: Add vocals, music, sound effects, and more.<br><br>* Modify: Modify vocals, music, sound effects, and more.<br><br>* Remove: Remove vocals, music, sound effects, and more. |
| **Extension**             | **Video extension** \- Continues the input video forward or backward and can require seamless visual and audio continuity. | * Extend forward/extend backward<br><br>* Extend forward/backward + subject reference |
| **Others**                | **One\-click video creation** \- Generates a short video from multiple images and/or videos, with optional text, stickers, transitions, and other elements. | * One\-click video creation from source assets<br><br>* One\-click video creation from source assets + reference video |
|                           | **Seamless video transition** \- Takes two input videos and generates the missing in\-between segment to create a seamless transition. | \-                                                           |
|                           | **Combined capabilities** \- Freely combines the capabilities listed above. | \-                                                           |


<span id="task-usage"></span>
### Task instructions

Seedance 2.5 divides tasks into two categories based on whether the input reference assets lock the properties of the output video. Seedance 2.0 does not make this distinction.


* **Locked:**  The input asset is strictly placed as a segment on the output video timeline. The model output adapts to the input asset, so the output video's aspect ratio, and in some cases its duration, are locked.

* **Unlocked:**  The input asset is used only as a semantic reference, so users can specify the output video's aspect ratio and duration.


<span id="task-locked"></span>
#### Locked: Editing, first and last frames, and extension

Editing, first\-frame or first\-and\-last\-frame generation, and extension automatically lock certain generation parameters based on the input assets and do not support user customization for those parameters. The specific rules are as follows:


<span aceTableMode="list" aceTableWidth="1,2,3,2"></span>
| **Task**                             | **Definition**                                               | **Instructions for output video locking**                    | **Trigger keywords in prompt**                               |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Editing**                          | Edits the visuals or audio of the original video, such as replacing the main subject, adding, removing, or modifying objects, or redrawing and restoring part of the frame. | * **Locks the output video's aspect ratio** , strictly matching the aspect ratio of the video to be edited. The `ratio` parameter must be set to `adaptive`.<br><br>* **Locks the output video's duration** , keeping it *approximately aligned* with the duration of the video to be edited. The `duration` parameter must be set to  **\-1** .<br><br>> If multiple input videos are provided, the model determines which video to edit based on the prompt.<br><br>> Due to the model's frame processing mechanism, the output duration may differ slightly from the input, by up to about 0.3 seconds. This only compresses some transition frames; the output content remains *approximately aligned* with the input and stays complete and unchanged.<br><br>> If a video generated by Seedance 2.5 is used as the editing input, the output duration will not differ from the input duration.<br><br><br>* It is recommended to set `output_format` to `mov`. | 1. Set `content.role` to `reference_image`, `reference_video`, or `reference_audio`.<br><br>2. **Include at least one editing trigger in the prompt:**  **edit video** , **add** , **insert** , **remove** , **delete** , **modify** , **replace** , **change to** , or similar wording.<br><br>> Add small animals to `@video1`; replace the character in `@video1` with `@image1`; remove the background music from `@video1`. |
| **First frame/first and last frame** | Uses one image as the first frame to generate a video, or two images as the first and last frames. | * **Locks the output video's aspect ratio** , strictly matching the aspect ratio of the first\-frame image. The `ratio` parameter must be set to `adaptive`.<br><br>> If the last frame has a different aspect ratio from the first frame, it will be stretched. Use first and last frames with the same aspect ratio.<br><br><br>* **Duration:**  user\-defined. | Set `content.role` to `first_frame` or `last_frame`.         |
| **Extension**                        | Extends the original video forward or backward.              | * **Locks the output video's aspect ratio** , strictly matching the aspect ratio of the video to be extended. The `ratio` parameter must be set to `adaptive`.<br><br>> If multiple input videos are provided, the model determines which video to extend based on the prompt.<br><br><br>* **Duration:**  user\-defined.<br><br>* It is recommended to set `output_format` to `mov`. | 1. Set `content.role` to `reference_image`, `reference_video`, or `reference_audio`.<br><br>2. **Include at least one extension trigger in the prompt:**  **extend forward** , **extend backward** , **continue** , **continue from** , **extend the story** , or similar wording.<br><br>> Extend `@video1` backward: the character from `@image1` falls from the sky...; Continue the first 5 seconds of `@video1`: the woman from `@video2` enters the frame and says... |


<span id="task-unlocked"></span>
#### Unlocked: Reference tasks, storyboards, and keyframes

In general, reference\-based tasks do not lock the output video’s aspect ratio or duration based on the input assets. The following two task types are especially worth noting, as they are also unlocked:


<span aceTableMode="list" aceTableWidth="1,2,3"></span>
| **Task**       | **Output video instructions**                                | **Illustration**                                             |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Storyboard** | * **The generated visuals are not strictly aligned with the storyboard:**  When using a multi\-panel storyboard as input, meaning multiple storyboard frames are combined into a single image, the generated video will not strictly match the storyboard's specific visual details. The storyboard mainly serves as a high\-level plot reference.<br><br>* It is recommended to use relatively simple line\-art storyboards and use the prompt to fill in information not shown in the storyboard, such as actions, camera movement, style, and other basic details. | <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_001_D1WGdoTCUosz6Nxs5cRc9hgyn2b.png) </span> |
| **Keyframes**  | * **Generated visuals align with the keyframes:**  Input multiple independent images as keyframes, which may include first\-frame or last\-frame images. The generated video visuals will be relatively strictly aligned with the input images.<br><br>* **Duration:**  user\-defined. | <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_002_DwRTdAYJhoPCYAxlq1Acypfcn9e.png) </span> <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_003_Ey86dklhnoS8Q6xr1BBcCqUgnwb.png) </span> <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_004_WVY6dMcTIoDWm1xfXZzcYXQdncd.png) </span> <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_005_Y3BsdKO7Vov0Foxl9PPceRkLned.png) </span><br><br><span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_006_FbLvdj4Rao2njbxc0EicyNYFnxc.png) </span> <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_007_P5SAdYqZjom7FSxE5eicG54hnOd.png) </span> <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_008_DIYudkBHno6uuOxX8lgckyoQnuh.png) </span> |


<span id="material-input"></span>
### Reference asset input recommendations

Seedance 2.5 supports up to **50 reference assets** per request, including images, audio, and videos. These assets may refer to the same subject or to different subjects, such as characters, animals, props, locations, and more. To make full use of the model's capabilities, we recommend the following when preparing reference assets:


<span aceTableMode="list" aceTableWidth="1,2"></span>
| **Use case**                                                 | **Input recommendations**                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Total reference asset input limits                           | * **Images:**  Up to 30 images, with resolution up to 4K.<br><br>* **Videos:**  Up to 10 videos, with a combined total duration of no more than 30 seconds.<br><br>* **Audio:**  Up to 10 audio clips, with a combined total duration of no more than 30 seconds. |
| For subject audio/video references, how many subjects are recommended? | **1\-5 subjects** generally produce better results. You may try **6\-10 subjects** , but stability may decrease and multiple attempts may be needed. |
| For subject audio/video references, what input duration is recommended? | **5\-10 seconds** generally works better. Longer inputs may reduce stability, and multiple attempts may be needed. |
| For subject image references, how many subjects are recommended? | **1\-8 subjects** generally produce better results. You may try **9\-12 subjects** , but stability may decrease and multiple attempts may be needed. |
| What is the difference between subject image inputs from different viewpoints? | * For **1\-5 subjects** , both **single\-view** and **multi\-view** inputs are supported.<br><br>* For **more than 5 subjects** , **single\-view** inputs are generally more stable. If multiple viewpoints are needed, it is recommended to split them into separate images from different views, rather than using one image that contains multiple viewpoints. |
| For storyboard references, how many panels are recommended?  | * Multi\-panel storyboards are currently better suited for **15 panels or fewer** .<br><br>* Stick\-figure or line\-art storyboards are recommended. Avoid adding text directly on the storyboard. |
| For 3D clay\-model references, is coarse\-grained or fine\-grained modeling recommended? | Simple, coarse\-grained 3D clay\-model video generally works better as a reference. Use only simple geometric primitives to represent people, objects, animals, and similar subjects. |
| For video editing, what video length is recommended?         | Videos within **20 seconds** generally produce better results. Longer videos may reduce stability, and multiple attempts may be needed. |
| For video editing with reference images, how many images are recommended? | **1\-5 reference images** generally produce better results. You may try **6\-8 reference images** , but stability may decrease and multiple attempts may be needed. |
| For video extension, what format is recommended?             | To achieve the best audio\-visual continuity, use the `mov` format for both the input and output videos. |


<span id="prompt-writing"></span>
### Prompt writing recommendations

> Treat Seedance 2.5 as a visual content producer, and write structured prompts with a visual storytelling mindset.


<span id="prompt-basic"></span>
#### Basic prompting techniques

**`Asset Referencing for R2V`**

Clearly identify each image, video, or audio asset by its upload order and intended purpose, such as which asset represents the subject, voice, action, scene, and so on.

**`One-Sentence Summary`**

Subject + Location + Event + Genre/Style + Camera movement...

**`Detailed Plot Description`**

Shot sequence or timeline: Either format is acceptable. Use timestamps or “Shot N” to divide the video into segments, and describe each segment’s specific visuals, camera movement, actions, dialogue, sound effects, and other details.

Use positive descriptions whenever possible. Negative constraints are supported for subtitles and audio control, such as  **“no subtitles”**  and  **“no BGM.”** 

**`Additional Notes`**

Add any visual details that should remain consistent throughout, such as camera angle, camera movement, environment, scene setting, sound, atmosphere, and other recurring elements.


<columns>
<columnsItem zoneid="P18Dm2qeng">

```Plain
Realistic nature documentary style, natural lighting and shadows. On a warm afternoon, on a grassy slope in the forest, a chubby panda cub rolls down the hill.

The panda has fluffy, realistic black-and-white fur, a small round body, and clumsy, adorable movements. The scene is a green forest slope. The ground is covered with grass, moss, clover, soil, small stones, dry branches, and a few small yellow flowers. Tall tree trunks and dense woods are softly blurred in the background. The camera is a low-angle medium-wide shot with a slight handheld feel. The framing remains mostly stable, keeping the panda in frame at all times.

0s-3s: A panda cub lies on a green grassy slope, its body round and chubby. It begins to slowly roll sideways down the slope with clumsy movements, gently bending the grass beneath its body. A light breeze passes through, and sunlight filters through the trees from the upper left, creating dappled light and shadow.

3s-8s: The panda rolls toward the lower right of the frame and gradually comes to a stop, shifting from lying on its side to lying on its belly. Its round face turns toward the camera, and its front paws press into the grass. The panda lies in the foreground grass, adjusts into a comfortable position, slightly raises and lowers its head, and makes a soft little humming sound.

Low camera position, slight handheld feel, subtly following the panda as it moves toward the lower right. Natural depth of field: the foreground grass is slightly blurred, the panda remains clear, and the background forest is softly out of focus. Natural environmental audio only, including wind, rustling grass, and the soft plop of the panda rolling. The overall mood is warm, realistic, and natural.
```


</columnsItem>
<columnsItem zoneid="rH6yt6nZIW">

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_000_panda-cub-basic.mp4" controls></video>


</columnsItem>
</columns>


<span id="prompt-basic-reference"></span>
##### Reference tasks (multi\-asset mapping)

As the number of reference assets increases, **the mapping and reference relationships between assets become especially important** . The numbering should correspond to the upload order of the assets, such as **Image 1 / Video 1 / Audio 1** , and each asset should be explicitly bound in the text prompt. **It is not recommended to provide mapping information only inside the image itself.**  For example, avoid writing "John" on the protagonist's image and then simply saying "John is at school..." in the prompt, as this can easily cause character confusion or duplication.


* For multiple subjects, list the mapping relationships one by one. When there are many characters, use a list to avoid confusion.

   * Example 1:  *"The knight in Image 1"* 

   * Example 2:  *"Images 1\-2 are Character 1 and correspond to Audio 1; Images 3\-4 are Character 2 and correspond to Audio 2."* 

   * Example 3:  *"Image 1 depicts the protagonist John and uses the voice timbre from Audio 1."* 

* Specify the role of each reference asset clearly, including **what it should be used as a reference for** . If only part of an asset should be referenced, clearly state **which part** should be used.

   * Example 1:  *"Refer to the action of casting the spell in Video 1 and the wrap\-around camera movement in Video 2."* 

   * Example 2:  *"Refer to Image 1 for lighting and filters."* 

* When the reference asset itself is sufficiently accurate, simply state that it should be referenced and avoid repeatedly describing the scene in detail.

   * Example:  *"Strictly refer to the actions and camera movements in Video 1, and keep the sequence consistent with the video."*  There is no need to describe details such as raising a hand, turning around, or having the camera slowly orbit.


<span id="prompt-basic-edit"></span>
##### Editing tasks

Clarify the scope and content to be modified. Timestamps can be used for partial edits. Whenever possible, describe how the content should change from **A to B** .


* Example 1:  *"Only edit the man's dialogue in Video 1: change it to 'Don't come over here,' and adjust the accent to an American English accent..."* 

* Example 2:  *"Change the man's action from drinking coffee to mopping the floor from 4\-6 seconds in Video 1, and leave the rest of the content unchanged."* 

* Example 3:  *"Editing task: Replace the Asian woman on the right in Video 1 with the Latina woman from Image 1."* 


<span id="prompt-basic-first-last-frame"></span>
##### First and last frames


* Prioritize setting the image role through parameters as `first_frame` or `last_frame`. Note that this method locks the output video's aspect ratio, strictly aligning it with the user\-provided first\-frame image.

* The role can also be set as `reference_image`, with the specific images designated in the prompt as the first and last frames. Note that this method does not lock the output video's aspect ratio. The generated video will be similar to the first\-frame and last\-frame reference images, but may not match them exactly.

   * Example 1:  *"Image 1 is the first frame."* 

   * Example 2:  *"Image 3 is the first frame, and Image 5 is the last frame."* 


<span id="prompt-basic-timestamp"></span>
##### Timestamps

Timestamps can help clarify the progression of the story. Use **1\-second intervals** as the basic unit:


* If too little plot is specified within a given time range, the model may improvise more freely.

* If too much content is packed into a given time range, the result may contain excessive cuts or omit parts of the plot. Make sure the duration allocation is reasonable.

* It is not recommended to use timestamps to control high\-frequency actions, such as "shake your head three times per second."


Supported time\-control methods:


* Clear time intervals. Pay attention to timeline continuity and avoid gaps such as "0\-3s... 5\-6s...".

   * Example 1:  *"0\-3 seconds...3\-7 seconds...7\-15 seconds"* 

   * Example 2:  *"[1s\-4s]....[4s\-8s]....[8s\-12s]"* 

* Time\-point control.

   * Example 1:  *"Quick left sideways transition at the 5\-second mark."* 

   * Example 2:  *"At the 2\-second mark, a burst of golden lightning descends from the top of the frame..."* 

* Relative time control.

   * Example 1:  *"John stands there blankly. After 3 seconds, everyone around him shakes their head."* 

   * Example 2:  *"The frame freezes for 1 second after the main character presses the shutter."* 


<span id="prompt-basic-negative"></span>
##### Negative control


* Supports negative control for subtitles.

   * Example 1:  *"Do not add subtitles."* 

   * Example 2:  *"No subtitles."* 

* Supports negative audio control for finer dimensions, including sound effects, background music (BGM), and dialogue.

   * Example 1:  *"No BGM; generate only environmental sounds and action sounds."* 

   * Example 2:  *"No audio."* 


<span id="prompt-advanced"></span>
#### Advanced prompting techniques

<span id="prompt-advanced-camera"></span>
##### Camera language


* Basic camera and shot terms can be written directly, such as shot size (extreme wide shot/wide shot/medium shot/medium close\-up/close\-up), camera movement (push in/pull out/pan/track/follow/orbit/dive/pull back/tilt up/handheld shake), and camera angle (low angle/overhead shot/first\-person perspective).

* Common camera techniques can also be written directly, such as one\-shot/long take, Hitchcock zoom/dolly zoom, aerial perspective, FPV, bullet time, handheld shot, and speed ramp.

* For overly niche or technical terms, convert them into [term + descriptive explanation].

   * Example:  *"Rack focus: the focus shifts smoothly; the trees that were originally clear in the foreground become blurred, while the character in the background gradually becomes clear."* 

* For transition shots, clearly specify both the trigger point and the transition method. Whenever possible, include both the transition timing and method.

   * Example:  *"At the 5\-second mark, the camera quickly transitions leftward using a left wipe combined with a natural dissolve."* 


<span id="prompt-advanced-action"></span>
##### Action and expression descriptions


* **Actions:**  Give priority to general descriptions, such as "doing several sets of high\-knee raises and somersaults" or "both sides engaging in close combat." Only write specific details for a few memorable actions, and avoid repeating the same actions.

* **Expressions:**  Use descriptive sentences and reduce the use of idioms.


<span id="prompt-advanced-whitemodel"></span>
##### 3D clay\-model reference/rendering


* In the prompt, clearly state which elements of the 3D clay\-model video should be referenced.

   * Example 1: If the video does not contain lighting changes and you only want to reference camera movement and motion, write:  *"Refer to the camera movement and motion in [Video 1]..."* 

   * Example 2: If the video includes lighting changes that should also be referenced, write:  *"Refer to the lighting changes, camera movement, and motion in [Video 1]..."* 

* If reference images are also provided, clearly specify the mapping between the reference images and the 3D clay\-model video.

   * Example:  *"Map the man in gray clothing from [Image 1] to the red model in [Video 1], and replace the green model 2 in [Video 1] with the red\-haired girl from [Video 2]."* 

* Even when a 3D clay\-model video is provided, describe the desired generated video content in detail for better results. Make sure the text description is consistent with the 3D clay\-model video. For subjects without additional image or video references, describe the subject's appearance and key features in detail.


Example prompt:

*Refer to [Video 1] for the lighting direction and lighting changes, camera movement, character positions, music, sound effects, and visual rhythm to generate an animated scene.* 

*Replace the pink model in [Video 1] with Hina Amano from [Image 3], and replace the gray model in [Video 1] with Hodaka Morishima from [Image 2]. Use the rooftop and sky from [Image 1] as the full background scene.* 

*First, Hina Amano clasps her hands together and closes her eyes in prayer. Sunlight gradually illuminates her face and the distant buildings from the upper left of the frame. As the shot changes, Hodaka Morishima says "あっ?" with slight surprise. He turns toward the right side of the frame, leans back in surprise, and looks at the sunlight shining from the upper right. The light spreads from the lower left of the ground toward the upper right, illuminating the boy's clothing. Then the shot switches to a wide view referencing [Image 1]. The two protagonists stand with their backs to the camera. The boy spreads his arms and says "Ah~" in surprise, while the girl maintains her praying pose.* 

*Japanese animation style inspired by Makoto Shinkai. The lighting should evoke sunlight breaking through clouds, and the overall atmosphere should feel hopeful and emotional. The character appearances must strictly reference [Image 2] and [Image 3], remain consistent throughout the video, and avoid face changes. Keep the original audio unchanged. High image quality, rich details, stable motion, and smooth visuals.* 

<span id="prompt-advanced-storyboard"></span>
##### Multi\-panel storyboards


* **Avoid using too many panels:**  Multi\-panel storyboards are currently better suited for **15 panels or fewer** . Too many panels in a single input, such as an 18\-panel storyboard, can lead to still frames or incorrect sequence order. Storyboards also constrain the model's creative output, so make sure the storyboard is accurate and logically structured.

* **Avoid noisy or over\-sharpened storyboards:**  Do not use cluttered, over\-sharpened AI\-generated storyboards directly, and avoid adding too much text to the storyboard image.

   Examples of unsuitable multi\-panel storyboards:

   
   <columns>
   <columnsItem zoneid="yelJTENaRO">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_009_XAnedIcrLoYcTOxTbvccxrKznfh.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="MapshbSVxt">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_010_P4uodRd1VoGtlKxa3rccsEHonTd.png) </span>
   
   Not recommended
   
   </columnsItem>
   <columnsItem zoneid="JA9DT9fP8L">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_011_FLdHd8tNgoJaW2x2woccLQ67nnc.png) </span>
   
   Not recommended
   
   </columnsItem>
   </columns>
   
* **Avoid inconsistencies in the prompt:**  Make sure the prompt does not contain contradictions or unreasonable camera movement and motion design.

* **Storyboard panels are not strictly aligned with the final video:**  A multi\-panel storyboard will not be followed exactly frame by frame, and the generated video retains a degree of autonomy. If strict alignment is required, use the multi\-keyframe reference method.

* **Use stick\-figure or line\-art storyboards [recommended]:**  Use relatively simple line\-art storyboards and control generation through the prompt:

   * Step 1: Clearly state the mapping relationships of the reference assets.

   * Step 2: Write an overall story summary.

   * Step 3: Fully describe the plot according to the storyboard, and at minimum fill in information not shown in the storyboard. You may use timestamps to clarify the story logic.

   Line\-art storyboard example:

   
   <columns>
   <columnsItem zoneid="RnHtDE66Z5">
   
   ```Plain
   Asset binding: Storyboard - [Image 1]; bedroom - [Image 2]; Li Tian - [Image 3]; Li Qian - [Image 4]; the book Happiness - [Image 5].
   Shot 1:
   [Wide shot, locked-off camera, rule-of-thirds composition] On a snowy winter night, inside a quiet bedroom, a man stands sideways in front of a floor-to-ceiling window with his hands in his trouser pockets, looking out at the falling snow. The girl stands beside him, quietly looking at him. The atmosphere is calm and restrained, with snowflakes continuously falling against the glass window.
   Shot 2:
   [Medium over-the-shoulder shot] The girl's back is in the foreground. The man turns his head and looks at her gently. The girl lowers her head slightly and remains silent. Snow continues to fall outside the window.
   Shot 3:
   [Medium close-up, diagonal composition] The man holds the book Happiness and slowly hands it to the girl. The girl raises her hand to take the book.
   Shot 4:
   [Close-up of the girl's face, centered composition] The girl holds the book tightly against her chest. Her eyes are red, tears slowly fall, and her expression is filled with sadness.
   Shot 5:
   [Close-up of the man's face, diagonal composition] The man smiles gently and quietly looks at the tearful girl, with sadness in his eyes.
   Shot 6:
   [Wide shot, locked-off camera] The girl turns around and slowly walks out of frame. Only the man remains, standing alone in front of the window with his hands in his pockets, looking out at the snowy, windy night sky. The room feels empty and silent.
   ```
   
   
   </columnsItem>
   <columnsItem zoneid="K7hZJ7CAJd">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_012_HOJNdQNXEoEsjox9uTtcCVP3nFc.png) </span>
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_013_A8ZfdshHnorwKHxxn9vcJZ36n5P.png) </span>
   
   </columnsItem>
   </columns>
   
* **Concept storyboard usage:**  If the storyboard is a concept storyboard or keyframe design, the prompt can be simplified.

   * Example:  *"Construct a complete story plot according to the storyboard sequence, and use the shots in a reasonable and coherent way."* 

   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_014_To54dlVwsoe0grxwLOxcoOKWnbG.png) </span>


<span id="prompt-advanced-keyframe"></span>
##### Keyframe reference

When the video must strictly follow the storyboard, use **keyframe references** . Input each storyboard as an independent reference image in order, and state in the first sentence of the prompt:  **"Use Images X to X in order as keyframes."** 


* Example:  **"Use Images 1 to 7 in order as keyframes.**  In a sea of clouds and mountains, blue\-and\-pink long\-tailed spirit fish soar through the air. The camera slowly moves toward an ancient town built into the mountainside, focusing on the ancient pagoda at the top of the mountain. The scene then enters an elegant Chinese\-style hall, where the spirit fish flies in through the window, lands in the round pool at the center of the hall, and swims leisurely. Finally, the perspective cuts to a dark ancient temple, where an old monk with a white beard stands with his back to the camera, quietly gazing at a huge framed painting. Inside the painting are the hall and the spirit fish swimming in the pond. The overall style is a new Chinese Ukiyo\-e illustration."

  
   <columns>
   <columnsItem zoneid="fLQ5ArUhUw">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_015_LmUsdFzCfoUVGsx1uoXcS51TnUg.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="p1RRMfzvpR">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_016_O6GfdOchEokX1WxachycYQO6n2c.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="HToqK5Wyxl">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_017_VaV5dOFvGouMVhxrL75cZ8DLncb.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="nM4uIBSrDp">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_018_RX5ydLzxmonwNrx0u8CcZ1GLnDg.png) </span>
   
   </columnsItem>
   </columns>
   
   
   
   <columns>
   <columnsItem zoneid="UjN0dQBvdu">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_019_KO88dDOFsoPElXxQd6tcoUdXnKf.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="Vp0KNC1lDl">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_020_V606dSWxgoGbp0x0brxcIgBsnic.png) </span>
   
   </columnsItem>
   <columnsItem zoneid="Giy8PrJJFo">
   
   <span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_021_Z5mBdYZqGoVRYpxMcA4c4whvnVa.png) </span>
   
   </columnsItem>
   </columns>
   


<span id="diff-from-2-0"></span>
#### Differences from Seedance 2.0


1. **Timestamp support:**  Seedance 2.0 does not respond to timestamps and only responds to shot numbers, while Seedance 2.5 supports integer\-second timestamps.

2. **Multi\-view image support:**  Seedance 2.0 does not recommend using multi\-view images as subject references, while Seedance 2.5 supports them.

3. **Flexible aspect ratios:**  Seedance 2.0 only supports six fixed output aspect ratios, while Seedance 2.5 can support any output aspect ratio between  **[0.4, 2.5]**  by controlling the input assets.

4. **Improved V2V quality:**  Seedance 2.5 supports `MOV` output, which better preserves color consistency, brightness consistency, and audio\-visual consistency in extension and editing tasks.


<span id="capability-examples"></span>
## Prompt examples

<span id="reference-examples"></span>
### Reference\-to\-video generation

The usage of subject, motion, audio, and style references is consistent with Seedance 2.0. For details, see [Appendix: Prompt examples](https://docs.byteplus.com/en/docs/ModelArk/2222480#ff5fb3e6). The following sections introduce the reference capabilities added in Seedance 2.5.

<span id="whitemodel-reference"></span>
#### 3D clay\-model video reference and rendering

<span id="coarse-whitemodel"></span>
##### Coarse\-grained 3D clay\-model video


* Supports rendering from 3D clay\-model videos that contain dynamic or temporal information such as motion, camera movement, movement paths, and lighting changes. It also supports adding reference images for subjects, scenes, props, and other elements to control the rendering result.

* The current version performs better with simple modeling. It is recommended to use only simple geometric primitives to represent people, objects, animals, and similar subjects.


3D clay\-model type: Includes cuts, camera movement, and lighting


<columns>
<columnsItem zoneid="DzU7Z9dNpZ">

**Input: text**

Prompt: Use the 3D clay\-model reference video `<video1>` as the only reference for the entire video's camera movement, shot rhythm, shot\-size changes, subject motion trajectory, and camera blocking. Strictly preserve the shot order, camera position changes, movement patterns, and pacing of the 3D clay\-model video. Do not change the shot structure, add new shots, or alter the subject's motion logic.

Using the keyframe reference images for each stage, generate a 30\-second high\-quality 3D animated short film. The overall style should be dreamy, fairytale\-like, warm, and full of childlike fantasy. The character's appearance should remain consistent with the keyframes for each stage. Do not change the character design. The character's facial expressions and emotions should change naturally with the scene.


* 0\-3s (first\-frame reference: `<2pic>`): The shot starts from an overhead wide view and slowly pushes in toward a little girl on the floor. The girl sits on the carpet in her room, playing with a toy airplane. She stands up, turns left, and forcefully swings her right hand to launch the airplane. The toy airplane flies in an arc from left to right into the foreground. The sound gradually transitions from the sound of throwing a paper airplane into the engine sound of a real animated airplane, accompanied by gentle, soothing, cheerful background music.

* 3\-5s (reference: `<3pic>`): The airplane flies from left to right through hanging star decorations in the room. The girl rides the airplane into a fantasy sky. A flock of birds flies across the foreground, creating a natural transition. The camera continues side\-following and rotating.

* 5\-8s (reference: `<4pic>`): The camera continues side\-following and orbiting around the little girl. Throughout this segment, the girl keeps piloting the small airplane through a sea of sunset clouds. Around her, a flock of strange birds and giant mythic birds fly alongside her. The white dragon from the reference image swims forward through the air, a winged horse spreads its wings and flies, and a flying whale calls out. Floating islands appear in the background.

* 8\-10s (reference: `<5pic>`): The camera orbits to the back of the airplane. The airplane slowly dives toward the sea surface. The girl falls into the water, creating many bubbles in the frame. She swims toward the deep sea, now wearing a bubble\-shaped oxygen helmet.

* 10\-19s (references: `<6pic>`, `<7pic>`): The girl continues swimming deeper into the ocean. Suddenly, a manta ray swims into frame and carries the girl forward. The camera continues following the manta ray and the girl as they travel through a dazzling underwater world. The girl looks amazed by the beautiful underwater scenery. The camera keeps pushing forward, revealing a huge space\-time rift ahead. The area around the rift looks like broken mirrors, while inside the rift is a brilliant cosmic galaxy. The girl feels a little frightened, but is eventually pulled into the space\-time rift and arrives in a fantasy universe.

* 19\-23s (reference: `<8pic>`): The girl bursts out of the space\-time rift into the fantasy universe, and her outfit changes into the spacesuit shown in the keyframe. Wearing the spacesuit, she jumps from one planet to another. She reaches out, leaps forward, and catches a glowing star. The frame freezes.

* 23\-24s (reference: `<9pic>`): In the foreground, the girl and the planets begin to flip forward, gradually transforming and disappearing. In the background, the overhead view of the bedroom from the opening scene (reference: `<1pic>`) slowly fades in.

* 24\-28s (reference: `<9pic>`): The overhead camera continues pushing in. The girl lies asleep on the carpet, still holding the star\-catching pose with her hand. A toy airplane and a space\-themed picture book lie beside her. Her Asian father enters the frame from the lower left and gently covers her with a blanket. The lighting slowly shifts from warm dusk light to cool moonlight at night.

* 28\-30s (reference: `<10pic>`): The camera continues pushing in toward the picture book. The father enters the frame and gently closes the picture book on the floor with his right hand. The final frame freezes on the picture book.


Overall requirements: All visuals should reference the corresponding keyframes. The 3D clay\-model video should only be used as a reference for camera movement, camera motion, shot rhythm, camera blocking, and character animation. Do not reference its visual content. The long\-take transitions should feel natural and smooth. Actions should remain continuous, and character proportions and movement should remain consistent. Generate a 30\-second video in a 16:9 widescreen format.

</columnsItem>
<columnsItem zoneid="VarHPBFwyN">

**Input: video and images**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_001_video-1.mp4" controls></video>


&nbsp;

Video 1

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_022_AjpddkViSolsPfxJTv7c9uDEnCe.png) </span>

Image 2

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_023_KTDddO41Vod4W1xxY2icesjzn0f.png) </span>

Image 3

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_024_A8AcdIHcQolmt7xFxTTcAhHonsb.png) </span>

Image 4

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_025_FLzjd3zABo9HmqxDVEIcRwKZnLf.jpg) </span>

Image 5

</columnsItem>
<columnsItem zoneid="xUZLy4r2E2">

**Input: video and images (continued)** 

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_026_HFQid7k5AovyMhxXJcHcZc96nOc.png) </span>

Image 6

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_027_J6vWd5GW0oH8LYxYXhYceCvxnVj.png) </span>

Image 7

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_028_F0pkdLxQVok07Ax1Ge9cbDP5nxc.jpg) </span>

Image 8

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_029_Ol7PdZ0choM5KRxCNaZcJBrDn2g.png) </span>

Image 9

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_030_S0XidqVTDoTVw0xFaiIcsvY4nAe.png) </span>

Image 10

</columnsItem>
<columnsItem zoneid="NpIkNlMOCR">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_002_10.mp4" controls></video>


Synchronized comparison video:

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_003_8月3日(1).mp4" controls></video>


</columnsItem>
</columns>


<span id="fine-whitemodel"></span>
##### Fine\-grained 3D clay\-model video


* Designed for scenarios with complete modeling, with a focus on re\-rendering. It helps achieve richer and higher\-quality rendering results by “coloring” the 3D clay model.

* Try to provide a complete and clear fine\-grained 3D clay\-model video, without distracting elements such as trajectory lines, coordinate lines, camera cones, or similar visual interference.



<columns>
<columnsItem zoneid="uSMkj0dvsa">

**Input: text**

Render Video 1. No BGM; generate only environmental sounds and action sounds.

Rendering requirements: The background is a nighttime cyberpunk city in deep blue and purple tones, filled with dense skyscrapers. Huge holographic billboards and neon lights glow between the buildings. Several flying vehicles move through the sky, flashing faint lights and producing subtle mechanical sounds. The character is a small raccoon dressed in a black stealth suit, appearing mostly as a silhouette. Its footsteps are cautious and quiet. The character moves across the rooftop of one of the skyscrapers.

</columnsItem>
<columnsItem zoneid="NgRcT5IRf7">

**Input: video**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_004_偷感很重.mp4" controls></video>


</columnsItem>
<columnsItem zoneid="HgBLyYhDil">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_005_seedance25-cyberpunk-rooftop-raccoon-render-6s-720p-20260804-baseline-run01_cgt-20260804112511-lpq4w.mp4" controls></video>


</columnsItem>
</columns>


**High\-difficulty 3D clay\-model video previsualization**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_006_飞船-594音乐.mov" controls></video>


<span id="storyboard-reference"></span>
#### Multi\-panel storyboard reference


<columns>
<columnsItem zoneid="IRpuKaOVJv">

**Input: text**

Image 1: Nine\-panel storyboard reference, used for the overall shot structure, shot sizes, and camera\-movement rhythm.

Image 2: Live\-action reference of a rocket launch site on a dusk grassland, used as the benchmark for environmental composition, warm golden sunset light, cool twilight blue tones, and realistic color live\-action texture.

Image 3: Subject 1 (guardian robot) character appearance reference.

Image 4: Subject 2 (elderly grandmother) character appearance reference.

[Subject settings]

Subject 1 (guardian robot): Refer to Image 3. A near\-future weathered retro robot with an aged blue\-green metal body, mottled rust, a domed head, two glowing red circular camera eyes, thin antennas, and slender articulated limbs. It is very tall, about twice the height of a human.

Subject 2 (elderly grandmother): Refer to Image 4. A frail elderly woman with silver hair tied into a low bun, deep wrinkles, wearing a bright golden floor\-length dress with gold\-and\-blue embroidered details on the chest. Her expression is full of reluctance and sorrow. Her height only reaches the robot's chest.

Environment (dusk grassland launch site): Refer to Image 2. A near\-future grassland at dusk, with the sky gradually shifting from warm gold to cool blue. On the distant horizon, a launch tower stands with a white rocket, steam rising around it. Knee\-high wild grass sways in the wind across a vast, open landscape.

[Overall style]

Live\-action color cinematic film, realistic photoreal texture, full\-color visuals throughout. Color 35mm film look, fine realistic film grain, rich cinematic color grading, IMAX large\-format feel. Handheld cinematography with breathing\-like camera shake, shallow depth of field, wide aperture, continuous drifting foreground grass, sparks, and ash. Slight Dutch angle. Strong contrast between warm golden sunset light, cool twilight blue, and explosive warm orange. 16:9 horizontal frame. Near\-future emotional disaster\-film atmosphere: quiet, tragic, protective, and filled with reluctance.

[Strictly exclude]

Black\-and\-white, monochrome, grayscale, desaturated visuals; hand\-drawn, sketch, line art, illustration, comics, animation; storyboard frames, rough sketches; tilt\-shift miniature look, toy\-like appearance, plastic CG, glossy overexposed CG.

[Shot list] (9 shots, approximately 30 seconds)

Shot 1 (0\-3s): Extreme wide shot, ultra\-low camera position close to the ground, looking upward, handheld camera slowly tilting downward. Refer to the grassland composition in Image 2. The dusk grassland feels vast and empty. Knee\-high wild grass in the foreground sways out of focus, and warm golden lens flare sweeps across the frame.

Shot 2 (3\-6s): Medium front shot with a handheld camera. The robot supports the elderly woman.

Shot 3 (6\-10s): Facial close\-up. The elderly woman looks reluctant to part. Dialogue (elderly woman): "Fly safe, my child. Come back to me."

Shot 4 (10\-14s): Extreme wide shot tilting upward. The rocket rises with a thick white smoke trail. Dialogue (elderly woman): "There he goes... there he goes."

Shot 5 (14\-18s): Extreme wide shot. The rocket explodes and breaks apart in midair. Dialogue (elderly woman): "No... no, no—"

Shot 6 (18\-22s): Extreme facial close\-up. The elderly woman's pupils contract and tears fall. Dialogue (elderly woman): "...he was almost there."

Shot 7 (22\-25s): Close\-up transitioning to a medium close\-up. The elderly woman breaks down in tears. Dialogue (elderly woman): "Bring him back! Please—bring him back!"

Shot 8 (25\-28s): Ultra\-low\-angle, nearly vertical upward shot. The robot embraces the elderly woman, forming a protective dome around her. Dialogue (robot): "Don't look up. I've got you."

Shot 9 (28\-30s): Extreme wide rear shot. The two figures embrace tightly in silhouette. Dialogue (robot): "I'm still here. I'll stay... as long as you need."

</columnsItem>
<columnsItem zoneid="eY9O02PlLd">

**Input: images**

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_031_GDwhdxBqTooHB9xSGIpcQNz2nDb.png) </span>

Image 1

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_032_RkIdd31hTo7VtBxs5ZacIQY4nhH.png) </span>

Image 2

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_033_SAkwdVijHoYqmzxxvtncFt9pnsg.png) </span>

Image 3

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_034_ZdDydqSsIoiTbbx0na3cTYLanPg.png) </span>

Image 4

</columnsItem>
<columnsItem zoneid="lJG4v4AoMu">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_007_守护机器人_火箭发射_30s.mp4" controls></video>


</columnsItem>
</columns>


<span id="keyframe-reference"></span>
### Keyframe reference

**Input: text**

Create a one\-shot vertical pixel\-art wuxia\-themed video based on @Image 1 to @Image 6. Use Chinese\-style 8\-bit wuxia background music. The entire video should use a unified light\-blue background, consistent pixel\-art style, and a clean, bright, transparent visual look.

Shot 1:

Hold on the ink\-wash\-style "江湖风云" logo from @Image 1. The background is the unified light\-blue color. Keep the frame still for about 1 second.

Shot 2:

After the text area from @Image 1 disappears, the pixel\-art close\-up face of the male wuxia character from @Image 2 slides in from the bottom of the frame. The character blinks and looks toward the camera, then quickly moves downward and exits the frame. After the character exits, the original logo area transforms into the blue pixel\-art "武功秘籍" martial arts manual from @Image 3.

Shot 3:

Immediately transition to @Image 4. A small pixel\-art wuxia character jumps forcefully upward from the bottom of the frame and hits the blue diamond\-shaped question mark icon above. Bold dark\-blue text "今日闯江湖!" pops out above the question mark icon. After landing, the character strikes the standing pose from @Image 4, then raises a hand to greet the viewer. Next, the character prepares to run, turns toward the right side of the frame, and runs to the right, with the running pose referencing @Image 5. The camera follows the character smoothly to the right, and the character jumps out of frame from the right side.

Shot 4:

The UI interface from @Image 6 slides into the frame from the right. The pixel\-art wuxia character jumps in from the upper\-right corner and lands at the lower\-right side of the large "三月廿七日" text. The character opens both arms in an enthusiastic presentation pose and freezes. The final frame holds on this composition.

Overall requirements:

Pixel\-art wuxia visual style throughout, with a unified light\-blue background tone. The camera movement should be continuous and smooth, presenting a one\-shot flow with seamless position shifts and follow movement. Element transitions should feel natural, and character actions should connect smoothly. No stuttering, no flickering. Text and UI must remain clear and stable.


<columns>
<columnsItem zoneid="yKa9H2GwvF">

**Input: images**

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_035_F1jNda7EvoU6s8xZOu6c3OW6ngg.png) </span>

Image 1

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_036_N5M0du2QZoBTRTxltFYcmgnCnsb.png) </span>

Image 2

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_037_F38XdqrFkokBhRxHyA0caAnCnmc.png) </span>

Image 3

</columnsItem>
<columnsItem zoneid="f5zGjhOhyd">

**Input: images (continued)** 

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_038_Oj26dVH9Po2wSzxHmp2cGRjcnTf.png) </span>

Image 4

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_039_NJ6rdj0RcoiL8ExidO7cWlLznvj.png) </span>

Image 5

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_040_FKETdT9WroMFs6xgJa1cl4lWnEf.png) </span>

Image 6

</columnsItem>
<columnsItem zoneid="hpAIEuevfr">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_008_pixel_cgt-20260805221932-s8jkr.mp4" controls></video>


</columnsItem>
</columns>


<span id="edit-examples"></span>
### Edit videos

<span id="video-instruction-edit"></span>
#### Video instruction editing

Use a prompt to add, remove, or modify visual elements in a video.


<columns>
<columnsItem zoneid="ntPSPdDlV1">

**Input: text**

Preserve the composition, camera position, lighting, and performance rhythm of @Video 1. Only modify the female lead's appearance and expression: let her naturally age from her twenties to around sixty. The restraint in her eyes gradually softens, tears slide past the corners of her eyes, and the corners of her mouth slowly lift until she finally smiles through her tears. The entire video should be a continuous one\-shot, with no jump cuts and no flickering. Her facial features should gradually age without drifting or changing identity.

</columnsItem>
<columnsItem zoneid="vRNxEizc3x">

**Input: video**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_009_对镜含泪凝视.mp4" controls></video>


</columnsItem>
<columnsItem zoneid="DQX77aMo0m">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_010_20260803T215035_2614ad0b51c3_表情变化.mp4" controls></video>


</columnsItem>
</columns>


<span id="video-refimage-edit"></span>
#### Video editing with reference images

Use prompts to add, remove, or modify video content, with support for additional reference images to guide the editing results.


<columns>
<columnsItem zoneid="OhfKRQznsu">

**Input: text**

Replace the two\-person fight in @Video 1 with an empty\-handed probing exchange before a cold\-weapon duel.

Replace the scene with a medieval stone castle platform, an ancient courtyard, an outer platform of a mountain fortress, or a simple stone\-brick duel arena. The background should include castle walls, wind, fog, distant mountain ridges, and a flat stone ground. Refer to @Image 1 for the environment.

Replace the man in dark clothing in the video with @Image 2, and replace the man in light\-colored clothing with @Image 3. Keep the original actions and rhythm unchanged.

AI effects should only enhance the environment and texture: wind\-blown clothing, light fog, a small amount of dust at contact points, cool metallic reflections, subtle film grain, and an epic color palette. The overall style should be restrained, realistic, and evoke a classic hardcore duel atmosphere. Keep the background music synchronized with the action beats.

</columnsItem>
<columnsItem zoneid="dDAINEawyZ">

**Input: video and images**

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_041_TEd5dsNe8oQ8zAxxygRcgnqAnug.png) </span>

Image 1

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_042_AEsxdxoL2oCC5CxHuvMcSnoLnab.png) </span>

Image 2

</columnsItem>
<columnsItem zoneid="iaULwSBYDj">

**Input: video and images (continued)** 

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_043_Z0shdkGCbo0aTFxxJrfcN1h7n4d.png) </span>

Image 3

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_011_reference1.mp4" controls></video>


&nbsp;

Video 1

</columnsItem>
<columnsItem zoneid="bOmwNQJdTs">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_012_output.mp4" controls></video>


</columnsItem>
</columns>


<span id="video-audio-edit"></span>
#### Video audio editing


<columns>
<columnsItem zoneid="Z12COL0JzG">

**Input: text**

Translate the spoken dialogue in the video into Chinese, with no subtitles. Precisely adjust the lip movements to match the translated speech, while keeping everything else unchanged.

</columnsItem>
<columnsItem zoneid="XjQL88nw1l">

**Input: video**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_013_30秒动漫独白.mp4" controls></video>


</columnsItem>
<columnsItem zoneid="ve1rOCyE7l">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_014_动漫中文输出.mp4" controls></video>


</columnsItem>
</columns>


<span id="other-examples"></span>
### Other examples

<span id="video-extend"></span>
#### Video extension


* For video extension tasks, the volume of the generated video may differ slightly from the input video. When extending a video originally generated by Seedance 2.5, the volume difference is usually smaller, resulting in better seamless continuity.



<columns>
<columnsItem zoneid="XK06U30xXw">

**Input: text**

Extend @Video 1 by 5 seconds. A bee flies in and lands on the flower. Then, in a macro close\-up, its legs and abdomen are covered with golden pollen particles. The bee flaps its wings and takes off, and the camera follows it as it flies toward another flower of the same species. In slow motion, pollen shakes loose from the bee's fine hairs and falls precisely into the flower's stamen, magnifying the moment of pollination.

<div data-tips="true" data-tips-type="warning" data-tips-is-title="true">warning</div>


<div data-tips="true" data-tips-type="warning">Select <code>mov</code> as the output format.</div>


</columnsItem>
<columnsItem zoneid="iiOtLTD3nO">

**Input: video**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_015_0615_发芽.mp4" controls></video>


</columnsItem>
<columnsItem zoneid="UIJxfZwYQU">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_016_seedance25-extend-bee-pollination-5s-720p-mov-20260803-baseline-run01_cgt-20260803215229-28wbd.mov" controls></video>


Stitched video:

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_017_8月3日.mp4" controls></video>


> Pay close attention around the 15\-second mark: there should be no visible stitching or transition artifacts.

</columnsItem>
</columns>


<span id="one-click-video"></span>
#### One\-click video creation


<columns>
<columnsItem zoneid="B7WzHo0ce0">

**Input: text**

Turn all images into a one\-click video. The image order can be freely arranged. Generate a coffee shop vlog in a hand\-drawn animated doodle cutout style, documenting the fun daily moments of a puppy wearing different cute outfits and taking photos at the coffee shop. Generate trendy, internet\-style playful audio or BGM.

The images may move slightly, creating a live\-photo effect, but do not alter the original images. Keep the visuals highly consistent with the original images.

</columnsItem>
<columnsItem zoneid="WIBqoQO6Y5">

**Input: images**

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_044_YmwkduFMtopY2zxd5WxcgISrnCd.jpg) </span>

Image 1

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_045_JRY2dVlgMohjLWxPTsUcosBbneb.jpg) </span>

Image 2

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_046_HJDQdOr6IoxSIdxqHItc1NvKn8t.jpg) </span>

Image 3

</columnsItem>
<columnsItem zoneid="T7Q0A5z6Nk">

**Input: images (continued)** 

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_047_MEbKdxSqxoCRuSxcC5QcBOyrnWf.jpg) </span>

Image 4

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_048_VhAtdRoM5osPffxjKuZcrpHInde.jpg) </span>

Image 5

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_049_Rp2Sdg6OUosVCKxsnjMcGC3Anhh.jpg) </span>

Image 6

</columnsItem>
<columnsItem zoneid="tLAgR5amsp">

**Input: images (continued)** 

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_050_ScYudGz7So5oHoxJp8Icsk94nRc.jpg) </span>

Image 7

<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/img_051_MLVNdsa65oMMAYxdVouc7pysndb.jpg) </span>

Image 8

</columnsItem>
<columnsItem zoneid="R3e0dMe5T6">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_018_8282ec97-0d00-4a2f-9896-9c1d88c600bc.mp4" controls></video>


</columnsItem>
</columns>


<span id="video-transition"></span>
#### Seamless video transition


<columns>
<columnsItem zoneid="zJg7nbuCPf">

**Input: text**

Seamlessly connect [Video 1] and [Video 2]. At the end of [Video 1], the camera should quickly fly upward to the top, rapidly turn back, and then dive vertically downward, creating a natural seamless transition into [Video 2]. During the transition, the mahjong tiles gradually transform into high\-rise buildings, and the entire scene changes accordingly. Do not alter the two uploaded videos themselves.

</columnsItem>
<columnsItem zoneid="ipOuq1J7nn">

**Input: videos**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_019_1.mp4" controls></video>


&nbsp;

Video 1

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_020_2.mp4" controls></video>


&nbsp;

Video 2

</columnsItem>
<columnsItem zoneid="xYwq2U1fft">

**Output**

<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/videos/video-generation/sd25-pe/vid_021_积木转场视频.mov" controls></video>


</columnsItem>
</columns>


<span id="summary"></span>
## Summary

Compared with Seedance 2.0, Seedance 2.5 is not a cross\-generational leap in the same way that 2.0 was compared with 1.5. Seedance 2.0 already achieved a major breakthrough in core generation capabilities, while Seedance 2.5 builds on that foundation with systematic enhancements for real production scenarios. These include longer video generation, richer multimodal references, more stable editing and extension capabilities, and improvements in aspect ratio control, audio\-visual continuity, controllability, and workflow adaptability.

Therefore, the value of Seedance 2.5 is not simply that “a single video looks better,” but that it advances the model further in key areas such as reusability, deliverability, and scalable production. It pushes Seedance from an impressive creative generation model toward a scalable, end\-to\-end video creation workflow.
