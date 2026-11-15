Reflection


Across my predictions, what did I get right/wrong? Was there a problem I kept missing?

Across my predictions, I got folder stats right about halfway, it made the decisions on how it works on its own, but it made a really good directory tree drawing. I thought it would not make the tree correctly, or would mess up on it. I got the shopping website also about halfway, it decided to use HTML, CSS, JS and somehow packaged them all in one single file so it was really convenient. I thought it would make them in different files. I got software test reader completely wrong, I thought it would just not work since I never gave it information on what a CK testing output file is, so it had to make the program based on its own general knowledge. Claude blew my expectations completely away in software test reader project, and it genuinely impressed me. There was no universal problem except one odd thing, and it was that Claude asked me permission in each project to view outside of the working directory when I started the change in a new branch in each project.

2. In which project did the diff review find something real? In which did it find nothing, and does that make you trust that project's code?

In folder stats, the diff review just showed some few lines being updated. In shopping website, there were a lot of new additions to lines. In software reader, it just added about a couple lines and changed existing ones. I would say that folder stats and software test reader had kind of nothing found in the diff review, but shopping website was very interesting. The project's code got almost 30 more lines for a relatively simple feature in shopping website. I trust the project code on the folder stats and software reader projects, since the assistant had a very easy time making changes to the code, but I would not trust shopping website. I still don't know if I fully trust the project's code, everything works very well but in real situations I feel like every piece of code has many security vulnerabilities.

3. Choose a function/code section in a project that I did not read while building, without an assistant read it and write what I think it does then ask an assistant, and say what I got right/wrong

I choose the folder_stats.py file in my folder stats project. I choose this piece of code:
def describe(node):
    files = len(node["files"])
    subs = len(node["children"])
    text = f"{node['name']}/  ({files} file{'' if files == 1 else 's'}"
    text += f", {subs} subdirector{'y' if subs == 1 else 'ies'})"
    if node["error"]:
        text += f"  [could not read: {node['error']}]"
    return text
I think what this does is take the length of directories and other files using len(). It then prints the summary of the details it found.
After asking the assistant, it said:
----------
This function takes a dictionary representing a directory and returns a one-line human-readable summary of it.

What it expects as input: a node dict with these keys:

name: the directory’s name
files: a list of files in the directory
children: a list of subdirectories
error: an error message (or a falsy value like None or "" if there was no problem)

What it does, step by step:

Counts the files and subdirectories using len().
Builds a string starting with the directory name and a trailing slash, followed by the file count in parentheses, with correct pluralization (“1 file” vs. “3 files”, “0 files”).
Appends the subdirectory count, again handling pluralization, including the irregular form (“1 subdirectory” vs. “2 subdirectories”).
If node["error"] is truthy, appends a note like [could not read: Permission denied].
----------

What I got right was that the function does take the statistics using len, but I personally did not know how the code displayed the summary. It arranging the statistics into a one line readable summary is interesting.

4. Was there something I put in a CLAUDE.md that belonged in a handoff file/other way around? If I built these again, what would I change in every file?

All my CLAUDE.md files were the same, I just gave it a basic set of instructions for each project. One thing put in CLAUDE.md was what programming language to use, and if claude is allowed to modify the original files inside of the working directory or not. I think these should be listed in handoff files as well, since it is useful to know what language/files were used to make the original program. In the other case, the handoff files had the main goal of the project in a somewhat detailed explanation, I would put this inside of spec.md. In future projects, I will make a neat and organized spec.md file which has a main project goal section, then other sections that explain complicated features in detail. I would put in CLAUDE.md the same things I put in the file for this assignment, but I would add extra specific rules for each project, such as any language constraints or if claude should ask me permission or not for the changes it wants to make.
