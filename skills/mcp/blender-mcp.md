# Blender MCP Skill

Use this skill when the user wants to control Blender, create 3D scenes, manipulate objects, or generate/import 3D assets.

## Prerequisites
- Blender must be running with the blender-mcp addon enabled
- The MCP server must be configured in your MCP settings

## Important: All commands require `user_prompt` parameter
Every blender-mcp tool requires a `user_prompt` parameter describing the action.

## Core Commands

### Get Scene Info
```bash
npx mcporter call blender.get_scene_info user_prompt:"get scene info"
```

### Get Object Info
```bash
npx mcporter call blender.get_object_info user_prompt:"get cube info" object_name:Cube
```

### Capture Viewport Screenshot
```bash
npx mcporter call blender.get_viewport_screenshot user_prompt:"capture viewport" max_size:800
```

### Execute Python Code in Blender
For simple one-liners:
```bash
npx mcporter call blender.execute_blender_code user_prompt:"add cube" code:"bpy.ops.mesh.primitive_cube_add(size=2, location=(0, 0, 0))"
```

For complex multi-line code, write to a temp file first, then:
```bash
npx mcporter call blender.execute_blender_code user_prompt:"run script" code:"exec(open(r'C:/path/to/script.py').read())"
```

## Polyhaven Assets (Free HDRIs, Textures, Models)

**Note:** PolyHaven must be enabled in BlenderMCP panel (press N in 3D Viewport → check "Use assets from Poly Haven")

### Check Polyhaven Status
```bash
npx mcporter call blender.get_polyhaven_status user_prompt:"check polyhaven"
```

### Get Categories
```bash
npx mcporter call blender.get_polyhaven_categories user_prompt:"get categories" asset_type:hdris
# asset_type: hdris, textures, models, all
```

### Search Assets
```bash
npx mcporter call blender.search_polyhaven_assets user_prompt:"search wood textures" asset_type:textures categories:wood
```

### Download Asset
```bash
npx mcporter call blender.download_polyhaven_asset user_prompt:"download hdri" asset_id:forest_slope asset_type:hdris resolution:2k
# resolution: 1k, 2k, 4k
# file_format: hdr/exr (HDRIs), jpg/png (textures), gltf/fbx (models)
```

### Apply Texture to Object
```bash
npx mcporter call blender.set_texture user_prompt:"apply wood texture" object_name:Cube texture_id:wood_planks
```

## Sketchfab Models

### Check Sketchfab Status
```bash
npx mcporter call blender.get_sketchfab_status user_prompt:"check sketchfab"
```

### Search Models
```bash
npx mcporter call blender.search_sketchfab_models user_prompt:"search cars" query:car categories:vehicles count:10 downloadable:true
```

### Preview Model
```bash
npx mcporter call blender.get_sketchfab_model_preview user_prompt:"preview model" uid:abc123xyz
```

### Download and Import Model
```bash
npx mcporter call blender.download_sketchfab_model user_prompt:"download car model" uid:abc123xyz target_size:2.0
```

## Hyper3D Rodin (AI 3D Generation)

### Check Hyper3D Status
```bash
npx mcporter call blender.get_hyper3d_status user_prompt:"check hyper3d"
```

### Generate from Text
```bash
npx mcporter call blender.generate_hyper3d_model_via_text user_prompt:"generate chair" text_prompt:"a wooden chair"
# Optional: bbox_condition for aspect ratio control
```

### Generate from Images
```bash
npx mcporter call blender.generate_hyper3d_model_via_images user_prompt:"generate from image" input_image_paths:"C:/path/to/image.png"
# Or with URLs: input_image_urls:"https://example.com/image.png"
```

### Poll Generation Status
```bash
npx mcporter call blender.poll_rodin_job_status user_prompt:"check job" subscription_key:your_key
# Or for FAL_AI mode: request_id:your_request_id
```

### Import Generated Asset
```bash
npx mcporter call blender.import_generated_asset user_prompt:"import chair" name:MyChair task_uuid:your_uuid
```

## Hunyuan3D (AI 3D Generation)

### Check Hunyuan3D Status
```bash
npx mcporter call blender.get_hunyuan3d_status user_prompt:"check hunyuan3d"
```

### Generate Model
```bash
npx mcporter call blender.generate_hunyuan3d_model user_prompt:"generate car" text_prompt:"a red sports car"
# Or with image: input_image_url:C:/path/to/image.png
```

### Poll Job Status
```bash
npx mcporter call blender.poll_hunyuan_job_status user_prompt:"check job" job_id:job_xxx
```

### Import Generated Asset
```bash
npx mcporter call blender.import_generated_asset_hunyuan user_prompt:"import car" name:SportsCar zip_file_url:path_from_poll
```

## Common Workflows

### Create a Simple Scene
1. Get current scene info
2. Execute Python code to add primitives
3. Apply textures from Polyhaven
4. Set up lighting with HDRI

### Import External Model
1. Search Sketchfab for desired model
2. Preview to confirm it's correct
3. Download with appropriate scale

### AI-Generate Custom Asset
1. Check if Hyper3D or Hunyuan3D is available
2. Generate from text or image
3. Poll until complete
4. Import into scene

## Quirks & Gotchas (Discovered During Testing)

### 1. Hidden `user_prompt` parameter
Every tool requires `user_prompt` but it's not always shown in `--all-parameters`. Without it:
```
Error: 1 validation error... user_prompt Field required
```

### 2. Multi-line code doesn't work directly
MCPorter argument parsing breaks on semicolons and newlines. **Workaround**: Write to temp file, then:
```bash
code:"exec(open(r'C:/path/to/script.py').read())"
```

### 3. `set_texture` has Blender 4.0+ compatibility bug
Errors with `ShaderNodeSeparateRGB undefined`. **Workaround**: Assign materials via Python code instead.

### 4. Boolean modifier solver names changed
Blender 4.0+ uses `'EXACT'` not `'FAST'`. Adjust Python scripts accordingly.

### 5. PolyHaven must be manually enabled
Won't work until user enables it in BlenderMCP panel (N key → sidebar).

## When to Use This Skill
- User mentions Blender, 3D modeling, or scene creation
- User wants to add objects, textures, or lighting
- User needs to import models from Sketchfab or Polyhaven
- User wants to generate 3D assets with AI
- User needs to execute Blender Python scripts
