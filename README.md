```rust
use std::fmt;

fn main() {
    let mut dijith = Dev::default()
        .with_skill(Skill::Language("Rust"))
        .with_skill(Skill::Framework("Axum"))
        .with_skill(Skill::Language("TypeScript"))
        .with_skill(Skill::Language("Lua"))
        .with_skill(Skill::Framework("Next.js"))
        .with_skill(Skill::Framework("Angular"))
        .with_skill(Skill::Paradigm("Functional"))
        .with_interest_in(&["system design", "operating systems", "networking"]);

    dijith.about = "Computer engineering student interested in system design".to_string();

    dijith.push_project("rusty-vim", "Modal text editor built from scratch.");
    dijith.push_project("markweavia", "Markdown Slides creator");
    dijith.push_project("project-mosaic","Collaborative pixel canvas with a Rust/Axum backend.",);

    dijith.render();
}

struct Dev {
    handle: &'static str,
    about: String,
    skills: Vec<Skill>,
    projects: Vec<(&'static str, &'static str)>,
    interests: Vec<&'static str>,
    matrix_id: &'static str,
}

impl Default for Dev {
    fn default() -> Self {
        Self {
            handle: "dijith-481",
            about: String::new(),
            skills: Vec::from([Skill::Tool("Neovim"), Skill::Tool("Arch Btw")]),
            projects: Vec::new(),
            interests: Vec::new(),
            matrix_id: "@dijith:matrix.org",
        }
    }
}

impl Dev {
    fn push_project(&mut self, name: &'static str, description: &'static str) {
        self.projects.push((name, description));
    }

    fn with_skill(mut self, skill: Skill) -> Self {
        self.skills.push(skill);
        self
    }

    fn with_interest_in(mut self, interests: &'static [&'static str]) -> Self {
        self.interests.extend(interests);
        self
    }

    fn render(&self) {
        println!("\n\n[DEV] {} :: {}", self.handle, self.about);
        println!(
            "\n[CORE] {}",
            self.skills
                .iter()
                .map(ToString::to_string)
                .collect::<Vec<_>>()
                .join(" | ")
        );
        println!("\ninterested_in:: {}", self.interests.join(", "));
        println!("\n[ARTIFACTS]");
        self.projects.iter().for_each(|&(name, desc)| {
            println!("- {}: {}", name, desc);
            println!("  └─ https://github.com/{}/{}", self.handle, name);
        });
        println!("\n[MATRIX] - matrix:: {}", self.matrix_id);
    }
}

#[derive(Debug)]
enum Skill {
    Language(&'static str),
    Framework(&'static str),
    Tool(&'static str),
    Paradigm(&'static str),
}

impl fmt::Display for Skill {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            Self::Language(s) => write!(f, "lang::{}", s),
            Self::Framework(s) => write!(f, "framework::{}", s),
            Self::Tool(s) => write!(f, "tool::{}", s),
            Self::Paradigm(s) => write!(f, "paradigm::{}", s),
        }
    }
}
```

```bash
rustc dijith-481.rs
./dijith-481
```
```bash
[DEV] dijith-481 :: Computer engineering student interested in system design

[CORE] tool::Neovim | tool::Arch Btw | lang::Rust | framework::Axum | lang::TypeScript | lang::Lua | framework::Next.js | framework::Angular | paradigm::Functional

interested_in:: system design, operating systems, networking

[ARTIFACTS]
- rusty-vim: Modal text editor built from scratch.
  └─ https://github.com/dijith-481/rusty-vim
- markweavia: Markdown Slides creator
  └─ https://github.com/dijith-481/markweavia
- project-mosaic: Collaborative pixel canvas with a Rust/Axum backend.
  └─ https://github.com/dijith-481/project-mosaic

[MATRIX] - matrix:: @dijith:matrix.org
```
