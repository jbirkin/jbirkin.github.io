---
layout: post
title: "Dash Cutout Visualization"
date: 2025-02-11
categories: data-science
---

# Interactive Image Viewer for SPT3G and MeerKAT Cutouts

## Overview
This blog details how to build an interactive image viewer using **Dash**, designed to browse through **SPT3G and MeerKAT cutouts**. The viewer includes features for **navigation, reviewing images, adding notes, caching for performance**, and optimizing UI elements.

Understanding the formation and evolution of high-redshift galaxies is crucial for piecing together the cosmic history of star formation and feedback mechanisms. SPT3G and MeerKAT data provide complementary insights: SPT3G reveals dusty, star-forming galaxies (DSFGs) at high redshifts through sub-millimeter emission, while MeerKAT offers deep radio imaging that can expose AGN activity, starburst-driven winds, and extended emission.

By inspecting and annotating these cutouts side-by-side, we can identify companions, radio structures, and potential outflows, helping to select candidates for ALMA or JWST follow-up observations. This interactive Dash-based viewer facilitates rapid exploration, annotation, and comparison, accelerating scientific insights.

## Project Structure
Ensure your project follows this structure:
```
SPT_HighZ_Analysis/
├── data/
│   ├── processed_cutouts/   # PNG images
│   ├── notes.json           # JSON file to store notes
│
├── scripts/
│   ├── image_viewer_dash.py # Dash app
│
├── assets/                  # Dash auto-served folder
│   ├── (Optional) Store images here for automatic serving
```

## Setting Up Dash
Install Dash if you haven't already:
```bash
pip install dash dash-bootstrap-components flask-caching
```

## Building the Dash Viewer
### 1. Initialize the Dash App
```python
import dash
from dash import dcc, html, Input, Output, State
import dash_bootstrap_components as dbc
import os, json

# Define paths
image_dir = "../data/processed_cutouts"
notes_file = "../data/notes.json"
image_files = sorted([f for f in os.listdir(image_dir) if f.endswith(".png")])

# Load existing notes
if os.path.exists(notes_file):
    with open(notes_file, "r") as f:
        notes = json.load(f)
else:
    notes = {}

# Initialize Dash app
app = dash.Dash(__name__, external_stylesheets=[dbc.themes.SANDSTONE])
```

### 2. Layout Components
```python
app.layout = dbc.Container([
    html.H1("SPT3G & MeerKAT Cutout Viewer", style={"textAlign": "center"}),

    html.Div([
        html.Img(id="cutout-image", style={"width": "100%", "height": "600px", "object-fit": "contain"}),
    ], style={"textAlign": "center"}),

    html.Div([
        dcc.Slider(
            id="image-slider",
            min=0,
            max=len(image_files)-1,
            value=0,
            marks={i: str(i+1) for i in range(0, len(image_files), max(1, len(image_files)//10))},
            tooltip={"placement": "bottom", "always_visible": True},
        )
    ], style={"width": "50%", "margin": "auto"}),

    html.Div([
        html.H5("Review Notes:"),
        dcc.Textarea(id="notes-text", placeholder="Enter your notes...", style={"width": "50%", "height": 150, "margin": "auto"}),
        dbc.Button("Save Notes", id="save-button", color="primary", style={"marginTop": 10}),
        html.Div(id="save-status", style={"marginTop": 10, "color": "green"}),
    ], style={"display": "flex", "flexDirection": "column", "alignItems": "center"}),

    html.Div([
        dbc.ButtonGroup([
            dbc.Button("Previous", id="prev-button", color="secondary", n_clicks=0),
            dbc.Button("Next", id="next-button", color="secondary", n_clicks=0),
        ], style={"marginTop": "20px", "textAlign": "center"}),
    ], style={"display": "flex", "justify-content": "center"}),
], fluid=True)
```

### 3. Callbacks for Image Navigation and Notes
```python
@app.callback(
    Output("image-slider", "value"),
    Input("prev-button", "n_clicks"),
    Input("next-button", "n_clicks"),
    State("image-slider", "value"),
    prevent_initial_call=True
)
def navigate_image(prev_clicks, next_clicks, current_index):
    ctx = dash.callback_context
    if not ctx.triggered:
        return current_index
    button_id = ctx.triggered[0]["prop_id"].split(".")[0]
    
    if button_id == "next-button" and current_index < len(image_files) - 1:
        return current_index + 1
    elif button_id == "prev-button" and current_index > 0:
        return current_index - 1
    return current_index

@app.callback(
    Output("cutout-image", "src"),
    Output("image-label", "children"),
    Output("notes-text", "value"),
    Input("image-slider", "value")
)
def update_image(index):
    image_name = image_files[index]
    current_note = notes.get(image_name, "")
    label = f"Showing: {image_name} ({index+1}/{len(image_files)})"
    return f"/assets/{image_name}", label, current_note

@app.callback(
    Output("save-status", "children"),
    Input("save-button", "n_clicks"),
    State("image-slider", "value"),
    State("notes-text", "value"),
    prevent_initial_call=True
)
def save_notes(n_clicks, index, note_text):
    image_name = image_files[index]
    notes[image_name] = note_text
    with open(notes_file, "w") as f:
        json.dump(notes, f, indent=4)
    return "✅ Notes saved!"
```

## Running the App
```bash
python scripts/image_viewer_dash.py
```
Then open `http://127.0.0.1:8050` in your browser.

## Performance Optimization
1. **Caching Images**
   ```python
   from flask import send_from_directory, make_response
   
   @app.server.route("/images/<path:image_name>")
   def serve_image(image_name):
       response = make_response(send_from_directory(image_dir, image_name))
       response.headers["Cache-Control"] = "public, max-age=86400"
       return response
   ```

2. **Lazy Loading for Faster Browsing**
   ```python
   html.Img(id="cutout-image", loading="lazy", style={"width": "100%", "height": "600px"})
   ```

## Next Steps
- Add a **grid view** for easier browsing.
- Implement **filters for reviewed/unreviewed images**.
- Deploy the app on **AWS or Heroku**.

🚀 This interactive Dash app lets you efficiently browse, review, and annotate your SPT3G and MeerKAT cutouts! Happy analyzing!

