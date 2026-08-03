---
name: project-readme-file-generator
description: A simple tool to generate a README.md file for your project, from scratch, with a professional and visually appealing layout. It provides a template that includes sections for project information, badges, and links to documentation, making it easy to create a polished README file that effectively communicates the purpose and features of your project. This skill is designed to fetch interactive inputs from the user to fill up the core sections of the README file. Use this skill whenever the user wants to create a README.md file for their project, or says "generate readme", "create readme", "readme generator", "project readme", "readme template", or any variation of creating a professional and visually appealing README file.  
metadata:
  author: Alex Alcón
  version: 1.0.0
---

# Project README File Generator

This skill generates a general professional and visually appealing README.md file for your project, from scratch. It provides a template that includes sections for project information, badges, and links to documentation, making it easy to create a polished README file that effectively communicates the purpose and features of your project. This skill is designed to fetch interactive inputs from the user to fill up the core sections of the README file and add some related components to the project's root directory.  

The main template for the README file is located in the `assets/main_template.md` file. This template is well structured and defined with three sections: the header, the body, and the footer. Each section has subsections that are clearly defined and labeled as comments in `assets/main_template.md`: 

**The header section contains** 
- **Subsection 1.1 - Project logo or banner** commented as:
  ```html
  <!-- ────────────────────────────── -->
  <!-- header: project logo or banner -->
  <!-- ────────────────────────────── -->
  ```
- **Subsection 1.2 - Project title, one line description, links and dev badges** commented as:
  ```html
  <!-- ───────────────────────────────────────────────────────────────── -->
  <!-- header: project title, one line description, links and dev badges -->
  <!-- ───────────────────────────────────────────────────────────────── -->
  ```
- **Subsection 1.3 - Feature badges** commented as:
  ```html
  <!-- ────────────────────── -->
  <!-- header: feature badges -->
  <!-- ────────────────────── -->
  ```
- **Subsection 1.4 - A detailed synopsis of the project** commented as:
  ```html
  <!-- ──────────────────────────────────────── -->
  <!-- header: detailed synopsis of the project -->
  <!-- ──────────────────────────────────────── -->
  ```
- **Subsection 1.5 - A screenshot that shows how the project works** commented as:
  ```html
  <!-- ─────────────────────────────────────────────────── -->
  <!-- header: screenshot that shows how the project works -->
  <!-- ─────────────────────────────────────────────────── -->
  ```
- **Subsection 1.6 - Project table of contents** commented as:
  ```html
  <!-- ───────────────────────────────── -->  
  <!-- header: project table of contents -->
  <!-- ───────────────────────────────── -->
  ```

- **Subsection 1.7 - Project table of content's sections** commented as:
  ```html
  <!-- ─────────────────────────────────────────── -->
  <!-- body and footer: table of contents sections -->
  <!-- ─────────────────────────────────────────── -->
  ```
  This subsection exlplains clearly the purpose of each section in the table of contents, and what information should be included in each section. It also provides guidance on how to structure the content within each section to ensure that the README file is clear, concise, and easy to navigate.

**The body section contains** 
- The main content of the README file
- It is the detailed descriptions of the table of contents sections.
- This is basically the fill up of the template with the interactive inputs fetched from the user, and the addition of some related components to the project's root directory.

**The footer section contains** 
- Additional information about the project, such as acknowledgements and links to related resources. 
- This subsection exlplains clearly the purpose of each part in the footer section. 

## When to Use This Skill

Use this skill whenever you want to create a README.md file for your project, or say "generate readme", "create readme", "readme generator", "project readme", "readme template", or any variation of creating a professional and visually appealing README file.

## Inputs (Parameters)

This skill can be used either with a new project (starting from scratch) or with an existing project (adding a `README.md` file to an existing project). You must prompt the user for the necessary information to fill up the `./README.md` template from `./assets/main_template.md`. The user inputs will directly define the actual state of the project itself. Thus, fetch interactive inputs from the user to fill up the core sections of the `./README.md` file and add some related components to the project's root directory based from the file `./assets/docs/file_system_structure.md`. Therefore, prompt the user for the following inputs to fill up the `./README.md` template from `./assets/main_template.md`:

### Input 1: GITHUB_USERNAME and REPO_SLUG
> Provide your GitHub username and the repository slug. (e.g. GITHUB_USERNAME: alexalcon, REPO_SLUG: my-awesome-project)

### Question 1: Foundation
> Will a CHANGELOG.md file be applied to this project? (yes/no)

### Question 2: LICENSE
> What is the LICENSE type for this project? (e.g. MIT, Apache 2.0, GPLv3, etc.)

### Question 3: Header (Project title)
> What is the project title? (e.g. My Awesome Project)\

### Input 2: Header (One line description of the project)
>Provide a one line description of the project. Explain what the project does in a concise and clear manner. (e.g. An awesome project that does amazing things)\

### Input 3: Header (Docs link)
> What is the link to the project docs website? (add NaN if there is no website link for the project)
  
### Input 4: Header (Website link)
> What is the link to the project website? (add NaN if there is no website link for the project)

### Input 5: Header (Feature badges)
> Provide the feature badges for the project. (add NaN if there are no feature badges for the project)

### Input 6: Header (Detailed synopsis of the project)
> Provide a detailed synopsis of the project. Explain a brief introduction, the what and the why of the project.

### Input 7: Header (About - the project description; referencial framework)
Take into account the rules from `./assets/images/about_section_rules.png` and `./references/about_section_rules.txt` to generate pertinent questions to fill up the "About" section. Look good, you must cover up all the fields from `./assets/images/about_section_rules.png`. Follow up the rules in `./references/about_section_rules.pdf` and make sure to ask the user for all the necessary information.  

### Question 4: Header (Support section)
> Would you like to provide additional ways to contact the project maintainer/maintainers? (yes/no)

If the user inputs "yes" for Question 4, then prompt the user for the necessary information to fill up the Support section. The user will be prompted to input the anserws in this format: "link1 | link2 | ... | linkN".

### Question 5: Footer (Acknowledgements section)
> Would you like to provide acknowledgements for the project? (yes/no)


## Instructions (Steps by Step Instructions)

Take into reference always the file `./assets/main_template.md` to fill up the `./README.md` file. The template is well structured and defined with three sections: the header, the body, and the footer. Each section has subsections that are clearly defined and labeled as comments in `assets/main_template.md`.

### First step
Follow and copy all the contents from the structure defined in `./assets/docs/file_system_structure.md` to create the necessary files and directories. Check which components are already present in the project and which ones are missing. Create the missing components as defined in `./assets/docs/file_system_structure.md`. Therefore, copy all the contents from the `./assets/.github` directory also.

### Second step
From **Input 1: GITHUB_USERNAME and REPO_SLUG** input, fetch this data and fill them up in all the required sections from each component of the `./assets/docs/file_system_structure.md`.


### Third step
Fill up the `./LICENSE` file from **Question 2: LICENSE** input.

### Fourth step
Fill up the project name from **Question 3: Header (Project title)** input.

### Fifth step
Based on the user input from input 2, fill up the corresponding section regarding the "one line description of the project". Make your best approach to enhance the description and make it more appealing and professional.

### Sixth step
Fill up the project docs and website links from **Input 3: Header (Docs link)** and **Input 4: Header (Website link)** inputs. If the user inputs "NaN" for any of these links, then remove the corresponding section from the `./README.md` file.

### Seventh step
Fill up the project feature badges from **Input 5: Header (Feature badges)** input. If the user inputs "NaN" for this input, then add some default recomended badges according to the current context until here.

### Eighth step
Fill up the detailed synopsis of the project from **Input 6: Header (Detailed synopsis of the project)** input. Make your best approach to enhance the synopsis and make it more appealing and professional. Fill up the brief introduction, the what and the why of the project in the three separate paragraphs as defined in the `./assets/main_template.md` file.

### Ninth step
Fill up the "About" section from **Input 7: Header (About - the project description; referencial framework)** input. Make your best approach to enhance the content and make it more appealing and professional. Take into account the rules from `./assets/images/about_section_rules.png` and `./references/about_section_rules.txt` to generate a professional about section. Do not add the titles sections from `./assets/images/about_section_rules.png`, just fill up the content in paragraphs so that in must be clearly identified all these fields, just as it is explained int the corresponding part of the `./assets/main_template.md` file.

### Tenth step
For the resting parts (from the Built With section to Roadmap section), do not add any content because these sections are left for the user creativity or conventions to put on to them. This is thought as the fact that only the owner or the maintainer of the project knows better how his or her project works. Thus, just add the titles of these sections and leave them empty for the user to fill up later. Make sure to add a comment in the `./README.md` file to indicate that these sections are left for the user creativity or conventions to put on to them.

### Eleventh step
For the Support section, if the user inputs "yes" in **Question 4: Header (Support section)**, then fill up the input links and identify where the links come from (e.g. GitHub, LinkedIn, Twitter, etc.). If the user inputs "no" in **Question 4: Header (Support section)**, then just leave the default content in the Support section as it is defined in the `./assets/main_template.md` file. Also in the yes case, leave the default content in the Support section as it is defined in the `./assets/main_template.md` file, and just add the user input links continuing the bullet list of the default content in the Support section.

### Twelfth step
For the Contributing to the License sections, just fill up the default content in these sections as it is defined in the `./assets/main_template.md` file.

### Thirteenth step
For the Acknowledgements section, if the user inputs "yes" in **Question 5: Footer (Acknowledgements section)**, then fill up the input acknowledgements. If the user inputs "no" in **Question 5: Footer (Acknowledgements section)**, then do not add this section.

## Rules and Constraints

- Always follow the structure defined in `./assets/main_template.md` to fill up the `./README.md` file. The template is well structured and defined with three sections: the header, the body, and the footer. Each section has subsections that are clearly defined and labeled as comments in `assets/main_template.md`.
- Always follow the structure defined in `./assets/docs/file_system_structure.md` to create the necessary files and directories. Check which components are already present in the project and which ones are missing. Create the missing components as defined in `./assets/docs/file_system_structure.md`.
- Copy all the contents from the `./assets/.github` directory.
- Make sure to add the paths to images and the like to the `./assets/readme/` directory, and not to any other directory. Just as it is defined in `./assets/docs/file_system_structure.md`.
- The about section is the most important section of the README file. Take into account the rules from `./assets/images/about_section_rules.png` and `./references/about_section_rules.txt` to generate a professional about section. Do not add the titles sections from `./assets/images/about_section_rules.png`, just fill up the content in paragraphs so that in must be clearly identified all these fields, just as it is explained int the corresponding part of the `./assets/main_template.md` file.

## Outputs Format (Result(s))

Just follow up the structure defined in `./assets/main_template.md`. This is the source of truth of this skill.

## Examples (Sample Inputs and Outputs)

Look in the `./assets/examples` directory for sample README files. These are referencial examples only, so do not copy them directly. Use them as a reference to create your own README file based on the inputs and the structure of `./assets/main_template.md`.