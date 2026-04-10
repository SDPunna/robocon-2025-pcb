# GitHub Repository Setup Guide

## Quick Start: Creating Your GitHub Repository

### Step 1: Initialize the Repository

In your project folder (where your Altium files are located):

```bash
# Initialize git repository
git init

# Add all files
git add .

# Create your first commit
git commit -m "Initial commit: Robocon 2025 PCB design"
```

### Step 2: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `robocon-2025-pcb` (or your choice)
3. Description: "Multi-ESP communication and mechanism drive PCB for Robocon 2025"
4. Choose **Public** (for portfolio) or **Private**
5. **DO NOT** initialize with README (you already have one)
6. Click "Create repository"

### Step 3: Connect Local to GitHub

Copy the commands shown on GitHub after creating the repo:

```bash
# Add GitHub as remote origin
git remote add origin https://github.com/YOUR_USERNAME/robocon-2025-pcb.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 4: Organize Your Repository

Suggested folder structure:

```
robocon-2025-pcb/
├── .gitignore                    # ✓ Already created
├── README.md                     # ✓ Already created
├── LICENSE                       # ✓ Already created
├── Hardware/
│   ├── Altium_Project/          # Your .PrjPcb, .SchDoc, .PcbDoc files
│   │   ├── R2_Circuit.PrjPcb
│   │   ├── Schematic1.SchDoc
│   │   ├── PCB1.PcbDoc
│   │   └── Libs/                # Custom ESP32 libraries
│   └── Outputs/
│       ├── Gerbers/             # Manufacturing files
│       ├── BOM/                 # Bill of Materials
│       └── PDFs/                # Documentation
├── Firmware/                    # ESP32 firmware code
│   ├── Master_ESP32U/
│   ├── Locomotion_ESP32S3/
│   └── Shooter_ESP32S3/
├── Documentation/
│   └── Images/                  # Screenshots, photos
│       ├── PCB_Top_Layer.png
│       ├── PCB_Bottom_Layer.png
│       └── Schematic_Overview.png
└── SETUP.md                     # This file
```

### Step 5: Add Project Files

```bash
# Create folder structure
mkdir -p Hardware/Altium_Project/Libs
mkdir -p Hardware/Outputs/{Gerbers,BOM,PDFs}
mkdir -p Firmware/{Master_ESP32U,Locomotion_ESP32S3,Shooter_ESP32S3}
mkdir -p Documentation/Images

# Copy your Altium files to Hardware/Altium_Project/
# Copy generated Gerbers, BOM to Hardware/Outputs/
# Copy PCB screenshots to Documentation/Images/

# Add and commit
git add .
git commit -m "Add Altium project files and outputs"
git push
```

### Step 6: Export Images from Altium

For your README and documentation:

**PCB Screenshots:**
1. In Altium, open `PCB1.PcbDoc`
2. View → 2D Layout Mode
3. Switch to Top Layer view → File → Export → Image → PNG
4. Switch to Bottom Layer view → File → Export → Image → PNG
5. Switch to 3D view → File → Export → Image → PNG
6. Save to `Documentation/Images/`

**Schematic Screenshots:**
1. Open `Schematic1.SchDoc`
2. File → Smart PDF → Print to image or screenshot
3. Save to `Documentation/Images/`

**Add to repository:**
```bash
git add Documentation/Images/
git commit -m "Add PCB and schematic images"
git push
```

### Step 7: Generate Manufacturing Files

From Altium:
1. File → Fabrication Outputs → Gerber Files
2. File → Fabrication Outputs → NC Drill Files
3. File → Assembly Outputs → Bill of Materials
4. File → Assembly Outputs → Pick and Place Files

Save all to `Hardware/Outputs/`

```bash
git add Hardware/Outputs/
git commit -m "Add manufacturing files (Gerbers, BOM, assembly)"
git push
```

## Making Your Repository Professional

### Add Repository Badges

Edit `README.md` and customize the badges at the top:
- Replace `yourusername` with your actual GitHub username
- Add build status badges if you automate anything
- Add shields from https://shields.io

### Create Releases

When you complete a version:

```bash
# Tag your version
git tag -a v1.0 -m "Version 1.0 - Initial working design"
git push origin v1.0
```

Then on GitHub:
1. Go to "Releases"
2. Click "Create a new release"
3. Choose your tag (v1.0)
4. Add release notes
5. Attach ZIP of Gerbers and BOM

### Enable GitHub Pages (Optional)

For a project website:
1. Go to repository Settings
2. Pages → Source → main branch
3. Your README will become a website

## Best Practices

### Commit Messages

Good commit messages:
```bash
git commit -m "Fix trace width on power lines to 50 mils"
git commit -m "Add I2C pull-up resistors to master ESP32"
git commit -m "Update custom ESP32-S3 footprint library"
```

Bad commit messages:
```bash
git commit -m "updates"
git commit -m "fix"
git commit -m "changes"
```

### When to Commit

- After completing a feature (e.g., "Finished motor driver section")
- Before making major changes (so you can revert)
- After fixing a bug
- At the end of each work session

### What to Include

✅ **DO include:**
- All Altium source files (.PrjPcb, .SchDoc, .PcbDoc)
- Custom libraries (.SchLib, .PcbLib)
- Generated outputs (Gerbers, BOM)
- Firmware source code
- Documentation and images
- README, LICENSE, .gitignore

❌ **DON'T include:**
- History/ folder (temporary Altium files)
- __Previews/ folder (auto-generated)
- Large binary files that can be regenerated
- Your personal settings or passwords

## Sharing Your Repository

### For Portfolio

Add this to your resume/LinkedIn:
```
GitHub: github.com/YOUR_USERNAME/robocon-2025-pcb
Multi-ESP communication PCB for robotics competition
• Designed 2-layer PCB with I2C inter-processor communication
• Created custom ESP32 component libraries
• Implemented strategic power distribution for motor drivers
```

### For Collaboration

Share the repository link with:
- Your team members
- Professors/mentors for review
- Other Robocon teams
- Open-source hardware community

### For Job Applications

Employers love seeing:
- Clean, documented code/designs
- Professional README files
- Version control usage
- Thoughtful commit messages
- Manufacturing-ready outputs

## Troubleshooting

**Problem:** Git is not tracking my Altium files
- **Solution:** Check if the file extensions are in .gitignore. Remove if needed.

**Problem:** Repository is too large
- **Solution:** Don't commit History/ and __Previews/ folders. Add them to .gitignore.

**Problem:** I made a mistake in my last commit
- **Solution:** `git commit --amend` to edit the last commit message
- Or: `git reset HEAD~1` to undo the last commit (keeps changes)

**Problem:** I want to collaborate with teammates
- **Solution:** Add them as collaborators in repository Settings → Collaborators

## Next Steps

1. ✅ Create GitHub repository
2. ✅ Upload all files
3. ✅ Add images and screenshots
4. ✅ Generate and upload manufacturing files
5. ✅ Write good commit messages
6. ✅ Share with your team
7. ✅ Add to your portfolio

---

**Need help?** Open an issue on GitHub or contact your team!

**Resources:**
- GitHub Guides: https://guides.github.com
- Git Cheat Sheet: https://education.github.com/git-cheat-sheet-education.pdf
- Markdown Guide: https://www.markdownguide.org
