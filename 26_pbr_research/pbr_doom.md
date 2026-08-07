# Takeaway from PBR Material Transform with GPT

In a research project at OTH Regensburg, the textures and materials of the
computer game DOOM from 1993 were remastered for Physically Based Rendering (PBR)
utilizing ChatGPT.
The 3D models of DOOM's scenes and materials were edited and rendered in Blender.

## Folder and file structure
- copy.txt: useful copy strings and prompts
- render_[...].png: example renderings from Blender
- rtgi_example: example rendering from rtgi
- doom_pbr: example script for rtgi
- DOOM-I-without-light-levels
	- presentation.blend: PBR ready room in E2M5 scene
	- material.blend: material tests
	- preview.blend: testing inside the scene
	- maps/3D/E2M5.obj: the reference level
	- maps/3D/E2M5.mtl: the updated material
	- maps/3D/E2M5_Light.obj: an area light for testing rtgi

## Environment for ChatGPT
- GPT-5.5 and Images 2.0
- Plus subscription

## Workflow
1. In the supporting AI/ChatGPT, describe what texture you want to achieve (rather than directly passing the original reference texture)
	- "Can you give me a photorealistic PBR color texture map that represents a metal wall? It should be old, corroded etc. e.g. from a restricted/contaminated site or area. It should have very high quality, resolution and detail"
1. If the texture is sufficient in quality, use it directly or else: Provide the original texture for refinement.
	- "You can take this as inspiration for further refinement"
1. Generate the matching PBR maps sequentially.
	- "Create a matching normal map"
	- "Create a matching roughness map"
	- "Create a matching metalness map"
	- "Create a matching ambient occlusion map"
1. When the material is finalized in Blender, update the according *.mtl file with the new COLOR texture

1. Additional ideas:
	- Push the AI to halucinate
	- Use/Pass reference materials e.g. from https://ambientcg.com/
	- Try using ChatGPT's "Thinking" model instead of "Instant"

## Problems

1. Resolution
	- Quality is restricted to around 1k - 1.5k images
	- 4k only possible upscaled, but without actual quality improvement

1. Tiling
	- It is hard to tailor tiles (e.g. as repeating floor texture).
	- Seams might not line up
	- Different shading on the tile for lower vs. upper or left vs. right bound

1. PBR Maps
	- Plausibility needs to be verified, e.g. check metal areas in metalness map
	- Often switches between resolutions/aspect ratio -> Verify that the same resolution and aspect ratio is generated compared to the color map
	- Metalness map is often inverted -> try to refine or use Color Inversion Node in Blender
	- Big variance in resulting metalness maps for similar surfaces -> partially fixable by tweaking the color ramp in Blender
	- Roughness map sometimes inaccurate, e.g. metal becomes more white (rough) where other elements like cables are more black (specular)

1. Poor Batch Generation
	- ChatGPT only generates one image per prompt. Therefore, multiple textures and the matching PBR textures need to be generated sequentially.

1. Coherance/Continuity
	- It is hard to achieve exact continuity between adjacent textures. For instance, CEMENT3 and CEMENT5 have a slightly varying hazard stripe which is visible when placed next to each other
	-> To solve this, it might be required to copy one of the stripes and place it in the relevant textures by hand

1. Image Rate Limit
	- Error message: "You're generating images too quickly. To ensure the best experience for everyone, we have rate limits in place. Please wait for 4 minutes before generating more images."
	-> occured after around 15 images in 30 Minutes