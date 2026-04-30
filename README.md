# CS 178 — Lab 20: Data Visualization with Plotly + Flask

A starter Flask app for CS 178 at Drake University. Students use this repo to complete Lab 20, which covers building and improving interactive data visualizations using Plotly Express, served via Flask.

---

## What's in This Repo

```
cs178-lab20/
├── app.py                        # Flask app — three routes: /bad, /good, /my-chart
├── requirements.txt              # Python dependencies
├── templates/
│   ├── index.html                # Landing page linking to all three charts
│   └── chart.html                # Shared template that renders a Plotly chart
```

---

## Quickstart

**1. Fork this repo** to your own GitHub account, then clone your fork:

```bash
git clone https://github.com/YOUR-USERNAME/cs178-lab20.git
cd cs178-lab20
```

**2. Install dependencies:**

```bash
pip3 install -r requirements.txt
```

**3. Run the app:**

```bash
python3 app.py
```

**4. Open your browser to `http://localhost:8888`**

You'll see three links:

- ❌ **Bad Chart** — the chart you'll diagnose and fix in Part A
- ✅ **Improved Chart** — your fixed version (edit the `/good` route in `app.py`)
- 📊 **My Chart** — your original visualization (edit the `/my-chart` route in `app.py`)

---

## The Dataset

This lab uses the **Gapminder dataset**, which ships directly with Plotly — no CSV download needed.

```python
import plotly.express as px
df = px.data.gapminder()
```

| Column      | Description                      |
| ----------- | -------------------------------- |
| `country`   | Country name                     |
| `continent` | Continent                        |
| `year`      | Year (1952–2007, every 5 years)  |
| `lifeExp`   | Life expectancy at birth (years) |
| `pop`       | Population                       |
| `gdpPercap` | GDP per capita (USD)             |

---

## Lab Overview

### Part A — Fix the Bad Chart

The `/bad` route serves a deliberately broken bar chart. Your job is to:

1. Identify what's wrong with it (written response)
2. Replace it in the `/good` route with an improved scatter plot
3. Apply five specific fixes: title, axis labels, colorblind-safe palette, log scale, hover labels
4. Run your improved chart through a color blindness simulator

### Part B — Build Your Own Chart

The `/my-chart` route is a blank placeholder. Replace it with an original visualization that answers a question you find interesting about the Gapminder data. Requirements: different chart type than Part A, title, axis labels, colorblind-safe palette, interactive tooltips.

Full instructions are in the [Lab 20 document on HackMD](https://hackmd.io/@profmoore/).

---

## Deployment

Push your changes to GitHub, SSH into your EC2 instance, and pull them down:

```bash
# On your local machine
git add .
git commit -m "Lab 20: my changes"
git push origin main

# SSH into EC2
ssh -i your-key.pem ec2-user@YOUR-EC2-IP

# On EC2
cd cs178-lab20
git pull origin main
pip3 install -r requirements.txt
python3 app.py &
```

Open `http://YOUR-EC2-IP:8888` in your browser.

Make sure port 8888 is open in your EC2 security group:

> AWS Console → EC2 → Your Instance → Security → Security Groups → Edit Inbound Rules → Add Rule → Custom TCP, Port 8888, Source 0.0.0.0/0

---

## Quick Plotly Express Reference

```python
# Scatter plot
px.scatter(df, x="col1", y="col2", color="col3", size="col4",
           hover_name="country", title="My Title",
           labels={"col1": "Human Label"}, log_x=True,
           color_discrete_sequence=px.colors.qualitative.Safe)

# Horizontal bar chart (sorted)
px.bar(df.sort_values("col", ascending=True), x="col", y="country",
       orientation="h", title="My Title",
       color_discrete_sequence=["#4C78A8"])

# Line chart
px.line(df, x="year", y="lifeExp", color="country", title="My Title")

# Box plot
px.box(df, x="continent", y="gdpPercap", color="continent",
       title="My Title", log_y=True)

# Choropleth world map
px.choropleth(df_2007, locations="iso_alpha", color="lifeExp",
              hover_name="country", color_continuous_scale="Viridis",
              title="Life Expectancy 2007")

# Convert a Plotly figure to an HTML string for Flask
chart_html = fig.to_html(full_html=False, include_plotlyjs="cdn")
```

---

## Course Info

**CS 178 — Cloud Computing and Database Systems**
Drake University — Spring 2026
Instructor: Prof. Moore — [@profmoore](https://hackmd.io/@profmoore/)
