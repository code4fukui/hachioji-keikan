# Hachioji Landscape Resources Map (hachioji-keikan)

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

This repository is a template for creating interactive, web-based municipal maps. It is demonstrated here as a "Regional Landscape Resources Map" for Hachioji City, Tokyo, but can be adapted for various purposes, including hazard maps.

**Live Demo:** ~~https://sankichi92.github.io/hachioji-keikan/~~ *(unavailable)* *(demo unavailable)*


![A screenshot of the hachioji-keikan map application, showing a map of Hachioji city with colored markers indicating points of interest. A layer control panel is visible in the top-right, and a series of image cards with titles are displayed in a collapsible panel in the bottom-left.](https://user-images.githubusercontent.com/873336/163695254-83959954-215c-4861-9878-3806a6442654.png)


## Features

- **Easy Setup:** Create a new map instance by using this repository as a template.
- **Custom Data Layers:** Automatically generate interactive map layers by adding CSV files with latitude and longitude data to the `/csv` directory.
- **Image Display:** Showcase relevant photos in a collapsible panel on the map by adding image files to the `/images` directory.
- **Configurable Tile Layers:** Add overlay tile layers, such as those from the national Hazard Map Portal, via a simple configuration file.
- **Automated Deployment:** A pre-configured GitHub Action builds and deploys the map to GitHub Pages on every push to the `main` branch.
- **Modern Tech Stack:** Built with React, Leaflet, and TypeScript.

## How It Works

The project uses a set of Node.js scripts at build time to prepare data for the React application:
1.  `hazardmap-config.jsonc` is parsed into a standard JSON file.
2.  The municipal boundary GeoJSON is fetched from the [Shikuchoson Boundaries](https://shikuchoson-boundaries.sankichi.app/) API based on the configured prefecture and city name.
3.  All `.csv` files in the `/csv` directory are converted into a single GeoJSON file.
4.  All images in the `/images` directory are copied into the source tree to be bundled with the application.

This data is then loaded into the React application to render the map, layers, and UI components.

## Getting Started

1.  **Create a new repository:** Click the "Use this template" button at the top of this page.
2.  **Clone your new repository:**
    ```bash
    git clone https://github.com/your-username/your-repository-name.git
    cd your-repository-name
    ```
3.  **Install dependencies:**
    ```bash
    npm install
    ```
4.  **Run the development server:**
    ```bash
    npm start
    ```
    This will start a local server and open the map in your browser.

## Customization & Deployment

1.  **Configure the Municipality:**
    -   Open `hazardmap-config.jsonc`.
    -   Set the `prefecture` and `shikuchoson` fields to match your target municipality. This is used to fetch the correct map boundary.

2.  **Add Custom Data Points:**
    -   Create one or more `.csv` files in the `/csv` directory.
    -   Each CSV must contain `lat` and `lon` columns for coordinates.
    -   Any other columns (e.g., `name`, `description`) will be automatically displayed in the popup when a user clicks on a map marker.

3.  **Add Supporting Images:**
    -   Place `.jpg`, `.png`, or other web-compatible image files in the `/images` directory.
    -   The filename (without the extension) will be used as the image's title on the map.

4.  **Add Hazard Map Layers (Optional):**
    -   In `hazardmap-config.jsonc`, add layer objects to the `tiles` array. You can find available data from the [Hazard Map Portal Site](https://disaportal.gsi.go.jp/hazardmap/copyright/opendata.html).
    -   Example for a flood layer:
        ```jsonc
        "tiles": [
          {
            "name": "洪水浸水想定区域（想定最大規模）",
            "url": "https://disaportal.gsi.go.jp/data/raster/01_flood_l2_shinsuisoutai_data/{z}/{x}/{y}.png",
            "attribution": "<a href='https://disaportal.gsi.go.jp/hazardmap/copyright/opendata.html'>ハザードマップポータルサイト</a>",
            "checked": true
          }
        ]
        ```

5.  **Deploy:**
    -   Commit and push your changes to the `main` branch.
    -   The GitHub Action will automatically build and deploy the site.
    -   In your repository settings, go to "Pages" and ensure the source is set to `gh-pages` branch and the `/ (root)` directory. Your map will be live at the provided URL.

## Data Sources

This project utilizes the following open data sources and APIs:

- **Base Map:** [GSI Japan Pale Tile](https://maps.gsi.go.jp/development/ichiran.html) by the Geospatial Information Authority of Japan.
- **Boundary Data:** [Shikuchoson Boundaries](https://shikuchoson-boundaries.sankichi.app/) for municipal boundary GeoJSON.
- **Hazard Data (Optional):** [Hazard Map Portal Site](https://disaportal.gsi.go.jp/) for official hazard map tile data.

## License

MIT License — see [LICENSE](LICENSE).