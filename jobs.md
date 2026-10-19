# Jobs

## Research Software Engineer: open source and open data for cancer imaging

**Brigham and Women's Hospital and Harvard Medical School, Boston, MA. Full time, hybrid. One or two positions.**

We are the team behind [NCI Imaging Data Commons](https://imaging.datacommons.cancer.gov) (IDC) and long-time maintainers of [3D Slicer](https://www.slicer.org). We are hiring a research software engineer to help us maintain and improve the open tools and open data that cancer imaging researchers depend on.

### Why this work matters

Cancer imaging data is stuck between worlds. Clinical images and annotations live in DICOM. Research code wants NIfTI, NRRD, OME-TIFF, or whatever a given AI framework reads. Most public datasets are shared once and never harmonized, so every user rebuilds the same plumbing.

IDC takes the other path: everything is harmonized into standard DICOM, hosted in public cloud buckets, indexed so you can query it, and viewable in the browser. Today IDC holds 179 collections, more than 86,000 patients, over one million image series, and close to 100 TB of data, all free and open. 3D Slicer is one of the most widely used open source platforms for medical image computing, with more than 250 community extensions. We develop and support both, building the converters, Python packages, viewers, cloud workflows and documentation that make them useful to researchers, on their own and together.

This is not a model-training role. We use AI models; we do not develop them. If your main goal is to publish new deep learning architectures, this is not the job for you. If you want to build things thousands of researchers use, and you care that the data stays open and interoperable, read on.

### What you would work on

Tell us where you want to start. The role will be shaped around your strengths and the needs of the projects, and we expect you to work on multiple directions and move between areas over time. Everything is open source and available publicly on GitHub, mostly under the [ImagingDataCommons](https://github.com/ImagingDataCommons) and [Slicer](https://github.com/Slicer) organizations. Ample opportunities to build research connections and support collaborations with the US-based and international partners. Some of the directions include:

**1. Keep IDC running and growing.** Harmonize newly contributed collections into standard DICOM: radiology annotations as DICOM Segmentation and Structured Report objects, whole slide images, AI-derived results. Improve [idc-index](https://github.com/ImagingDataCommons/idc-index) and the [IDC tutorials](https://github.com/ImagingDataCommons/IDC-Tutorials). Identify what users need, support them on the forum, prepare webinars, write documentation.

**2. Make IDC usable by AI agents.** IDC was among the first NCI data commons to ship a [Model Context Protocol server and REST API](https://github.com/ImagingDataCommons/IDC-REST-MCP) and an [agent skill](https://github.com/ImagingDataCommons/imaging-data-commons-skill), so assistants such as Claude can build cohorts and answer questions about the data directly. Evolve these interfaces, help make them more capable and trustworthy, and work with Innolitics on a DICOM-standard MCP server that grounds metadata answers in the text of the standard. This is engineering and evaluation around large language models, not model development.

**3. Make it easier to get data into and out of DICOM.** Under our new NCI U24 award, build the IDC whole-slide-image conversion workflow around [wsidicomizer](https://github.com/imi-bigpicture/wsidicomizer), simplify tools to do conversion to research formats, help build and test a new open source DICOM validator, and assemble the test datasets that make all of this trustworthy. Work with Kitware on cloud-based annotation infrastructure built on 3D Slicer.

**4. Run AI at scale on IDC data.** Apply open source segmentation models such as TotalSegmentator and MONAI to a large multi-institutional cohort using our [CloudSegmentator](https://github.com/ImagingDataCommons/CloudSegmentator) workflows on Terra and JetStream2. Encode results as DICOM. Test and extend the conversion tools in [highdicom](https://github.com/ImagingDataCommons/highdicom) and [dcmqi](https://github.com/QIICR/dcmqi). Write the notebooks and tutorials that let others reuse the results.

**5. Bring microscopy narratives into DICOM.** With the Laboratory of Systems Pharmacology at Harvard Medical School, help map [Minerva](https://github.com/labsyspharm/minerva-story) stories onto the DICOM data model, convert multiplexed tissue images and image-derived data, and support integration of the Minerva viewer with IDC alongside [Slim](https://github.com/ImagingDataCommons/slim).

**6. Help build the next 3D Slicer.** With the Slicer core developers, experiment with the revised architecture: SlicerCore libraries decoupled from the Qt application, a unified extension system, WebAssembly targets. Reimplement existing capabilities such as the DICOM plugins as reusable Python and JavaScript packages, and build the conformance tests that prove the new code behaves like the old.

### Who we are looking for

* You are versatile. You are comfortable picking up a new dataset, library or standard quickly, and you like hopping between topics rather than owning one narrow component.
* You care about supporting the user community, and you are proactive: you learn on your own, spot issues and suggest solutions.
* You write Python fluently. C++ helps for 3D Slicer and dcmqi. JavaScript or TypeScript helps for viewers and the web-facing parts of Slicer. You do not need all three.
* You work in the open: git, GitHub, code review, continuous integration, tests, and documentation are how you normally work, not extras.
* You are curious about DICOM. You do not need to be an expert. You do need to be willing to read the standard and care about getting it right.
* You are interested in how AI agents should interact with scientific data, and skeptical enough to measure whether they get it right.
* Exposure to cloud platforms (Google Cloud, BigQuery, Terra) or high performance computing is a plus.
* Medical imaging or biomedical background is a plus, not a requirement.
* You communicate clearly in writing: issues, forum answers, documentation, technical reports, academic manuscripts.
* You can show us something you have built or contributed to. A GitHub profile or portfolio is worth more to us than a long CV.

Education: bachelor's, master's or PhD in computer science, engineering, physics, biomedical informatics or a related field. Experience: 0 to 5 years. We hire on demonstrated ability.

### What we offer

* Direct mentorship from Andrey Fedorov, technical lead of IDC, and day-to-day work in Boston with Ron Kikinis, Chris Bridge (MGH, maintainer of highdicom) and Steve Pieper (Isomics, 3D Slicer lead architect).
* Collaboration with a distributed open source community: Kitware, Open Connections, David Clunie, Innolitics, the 3D Slicer core developers, and the IDC team across the US.
* Everything you build is public. Upstream contributions to highdicom, dcmqi, idc-index, 3D Slicer and other community projects are part of the job, not a side activity.
* We use AI coding assistants in our daily work and expect you to use them too, with judgment and with tests.
* Co-authorship on the papers that come out of the work.
* Occasional travel to present IDC and 3D Slicer at events such as RSNA, MICCAI and the NA-MIC Project Week hackathons.
* Google Cloud resources provisioned for the project.
* Hybrid schedule and flexible hours.

### Funding

The position is funded across several multi-year projects led or co-led by our group, which gives it more stability than a single grant:

* NCI Imaging Data Commons, part of the NCI Cancer Research Data Commons, funded through a Leidos Biomedical Research contract.
* "National Cancer Institute Imaging Data Commons: improved sustainability and usability", NCI U24 CA306986, 2026 to 2031.
* Curation and enrichment of the APOLLO-5 imaging data, funded by NCI through Leidos Biomedical Research.
* IDC and Minerva collaboration with the Laboratory of Systems Pharmacology, Harvard Medical School.
* "3D Slicer: a unified open source platform for advanced cancer imaging research", NCI R01 CA310962, 2026 to 2031.

### How to apply

Email Andrey Fedorov at fedorov@bwh.harvard.edu with:

* your CV
* a link to your GitHub profile or portfolio
* a few sentences on which of the areas above interest you, and one thing you built or contributed that you are proud of
* optionally, a link to one or two things you have written: documentation, a tutorial, a forum answer, a technical report or a paper
* a few references who can vouch for you

Informal questions are welcome too.

Applications are reviewed on a rolling basis. We would like to fill the position as soon as possible.
