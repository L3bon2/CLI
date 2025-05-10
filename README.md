# CLI
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Linux CLI Roadmap for Frontend Developers</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f4f4f4;
      color: #333;
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #222;
      color: white;
      padding: 20px;
      text-align: center;
    }
    .progress-wrapper {
      padding: 10px;
    }
    .progress-bar {
      height: 25px;
      width: 100%;
      background-color: #ccc;
      border-radius: 5px;
      overflow: hidden;
      display: flex;
      position: relative;
    }
    .progress-subsection {
      flex: 1;
      border-right: 1px solid white;
    }
    .progress-fill {
      background-color: #4caf50;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      transition: width 0.3s;
    }
    .section {
      padding: 20px;
      margin: 20px;
      border-radius: 10px;
    }
    .section h2 {
      margin-top: 0;
    }
    .checkbox-item {
      margin: 10px 0;
    }
    .checkbox-item input {
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Linux CLI Roadmap for Frontend Developers</h1>
  </header>
  <div class="progress-wrapper">
    <div class="progress-bar">
      <div class="progress-fill" id="progressFill"></div>
      <!-- 12 static subsections -->
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection"></div>
      <div class="progress-subsection" style="border-right: none;"></div>
    </div>
    <div id="progressText" style="text-align:center; margin-top: 5px; font-weight: bold;"></div>
  </div>

  <div id="roadmap"></div>

  <script>
    const roadmapSections = [
      {
        title: "Essentials: Navigation & Files",
        color: "#ffadad",
        items: [
          "pwd, ls, cd – navigating directories",
          "mkdir, rmdir, touch, rm, cp, mv – managing files and directories",
          "Path concepts: absolute vs relative",
          "File types (file, stat)",
          "Wildcards (*, ?, []) for pattern matching"
        ]
      },
      {
        title: "Viewing & Searching Files",
        color: "#ffd6a5",
        items: [
          "cat, less, more – viewing files",
          "head, tail, nl – file beginnings/endings",
          "grep, find, locate – searching files and content",
          "wc, sort, uniq, cut, xargs – content analysis and manipulation",
          "diff, cmp, comm – comparing files"
        ]
      },
      {
        title: "File Permissions & Ownership",
        color: "#fdffb6",
        items: [
          "ls -l, chmod, chown, chgrp",
          "Numeric (e.g., 755) vs symbolic (e.g., u+x) permissions",
          "umask basics",
          "Understanding sudo use"
        ]
      },
      {
        title: "Working with Archives and Compression",
        color: "#caffbf",
        items: [
          "tar, gzip, gunzip, zip, unzip",
          "Understanding .tar.gz, .zip usage for project bundling"
        ]
      },
      {
        title: "Package Management",
        color: "#9bf6ff",
        items: [
          "apt, dpkg (Debian)",
          "yum, dnf, rpm (Red Hat)",
          "pacman (Arch)",
          "nvm – Node version manager",
          "npm, yarn, pnpm – usage via CLI"
        ]
      },
      {
        title: "Process Management & Monitoring",
        color: "#a0c4ff",
        items: [
          "ps, top, htop, kill, killall, pkill",
          "jobs, fg, bg, & for background jobs",
          "watch, time – repetitive monitoring and performance"
        ]
      },
      {
        title: "Networking Basics",
        color: "#bdb2ff",
        items: [
          "curl, wget – making HTTP requests",
          "ping, traceroute, netstat, ss, telnet, dig",
          "lsof -i, nc – testing local ports"
        ]
      },
      {
        title: "Text Processing & Pipelines",
        color: "#ffc6ff",
        items: [
          "Pipes (|), Redirection (>, >>, <)",
          "tee – output to file and console",
          "awk, sed – stream editing",
          "jq – parsing and formatting JSON"
        ]
      },
      {
        title: "Version Control with Git (CLI-based)",
        color: "#fffffc",
        items: [
          "git init, clone, add, commit, status, log, diff",
          "branch, checkout, merge, rebase",
          "stash, reset, revert",
          "push, pull, fetch, remote"
        ]
      },
      {
        title: "Front-End Build & Dev Tools via CLI",
        color: "#e0fbfc",
        items: [
          "webpack, vite, parcel",
          "eslint, prettier, stylelint",
          "jest, vitest, cypress",
          "CLI flags, config files, npx usage"
        ]
      },
      {
        title: "Dotfiles & Shell Customization",
        color: "#d0f4de",
        items: [
          "Editing ~/.bashrc, ~/.zshrc, ~/.profile",
          "Custom aliases (alias)",
          "export, env – environment variables",
          "PS1 prompt customization"
        ]
      },
      {
        title: "Scripting Basics",
        color: "#f1c0e8",
        items: [
          "Bash scripting: #!/bin/bash, variables, conditionals, loops",
          "Using CLI tools in scripts",
          "cron, at – job scheduling",
          "Automating builds, testing, cleanup, etc."
        ]
      }
    ];

    const roadmap = document.getElementById("roadmap");
    const progressFill = document.getElementById("progressFill");
    const progressText = document.getElementById("progressText");

    roadmapSections.forEach((section, idx) => {
      const container = document.createElement("div");
      container.className = "section";
      container.style.backgroundColor = section.color;

      const title = document.createElement("h2");
      title.textContent = section.title;
      container.appendChild(title);

      section.items.forEach((item, i) => {
        const div = document.createElement("div");
        div.className = "checkbox-item";
        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.dataset.section = idx;
        checkbox.dataset.item = i;

        const saved = localStorage.getItem(`checkbox-${idx}-${i}`);
        if (saved === "true") checkbox.checked = true;

        checkbox.addEventListener("change", () => {
          localStorage.setItem(`checkbox-${idx}-${i}`, checkbox.checked);
          updateProgress();
        });

        const label = document.createElement("label");
        label.textContent = item;

        div.appendChild(checkbox);
        div.appendChild(label);
        container.appendChild(div);
      });

      roadmap.appendChild(container);
    });

    function updateProgress() {
      const allCheckboxes = document.querySelectorAll("input[type='checkbox']");
      const total = allCheckboxes.length;
      const checked = Array.from(allCheckboxes).filter(c => c.checked).length;
      const percent = Math.round((checked / total) * 100);

      progressFill.style.width = percent + "%";
      progressText.textContent = checked > 0 ? percent + "% complete" : "";
    }

    updateProgress();
  </script>
</body>
</html>
