---
name: teach-loop
description: Teach the user a new skill or concept within this workspace, through a persistent learning loop (current knowledge, lesson, assignment, updated state) that resumes across sessions.
disable-model-invocation: true
argument-hint: "What would you like to learn about? (leave blank to resume)"
---

The user has asked you to teach them something. This is a stateful request - they intend to learn the topic over multiple sessions. Their progress persists between sessions in `STATE.md`, and every session follows [the learning loop](#the-learning-loop).

## Teaching Workspace

Treat the current directory as a teaching workspace. Workspace paths (`./lessons/` and the rest) resolve from the directory `/teach-loop` was run in; only the `*-FORMAT.md` links resolve from this skill's folder. The state of their learning is captured in this directory in several files:

- `MISSION.md`: A document capturing the _reason_ the user is interested in the topic. This should be used to ground all teaching. Use the format in [MISSION-FORMAT.md](./MISSION-FORMAT.md).
- `STATE.md`: The persistent state of the user's learning, and the first file you read in every session. It records what they know now, the gaps to close next, where they are in [the learning loop](#the-learning-loop), and a log of their progress. Use the format in [STATE-FORMAT.md](./STATE-FORMAT.md).
- `./reference/*.html`: A directory of reference materials. These are the compressed learnings from the lessons - cheat sheets, reference algorithms, syntax, yoga poses, glossaries. They are the raw units of learning. They should be beautiful documents which print out well, and are designed for quick reference.
- `RESOURCES.md`: A list of resources which can be explored to ground your teaching in contextual knowledge, or to acquire knowledge and wisdom. Use the format in [RESOURCES-FORMAT.md](./RESOURCES-FORMAT.md).
- `./learning-records/*.md`: A directory of learning records, which capture what the user has learned. These are loosely equivalent to architectural decision records in software development - they capture non-obvious lessons and key insights that may need to be revised later, or drive future sessions. These should be used to calculate the zone of proximal development. They are titled `0001-<dash-case-name>.md`, where the number increments each time. Use the format in [LEARNING-RECORD-FORMAT.md](./LEARNING-RECORD-FORMAT.md).
- `./lessons/*.html`: A directory of lessons. A **lesson** is a single, self-contained HTML output that teaches one tightly-scoped thing tied to the mission. This is the primary unit of teaching in this workspace.
- `./assignments/*.md`: A directory of assignments, one per lesson. An **assignment** is where the user applies what the lesson taught; their submission and your feedback live in the same file. Use the format in [ASSIGNMENT-FORMAT.md](./ASSIGNMENT-FORMAT.md).
- `./assets/*`: Reusable **components** shared across lessons. See [Assets](#assets).
- `NOTES.md`: A scratchpad for you to jot down user preferences, or working notes.

## The Learning Loop

Teaching runs as a loop, one cycle per gap. End every step by writing `STATE.md`: a session can stop at any point, and the next one must be able to resume from that file alone.

1. **Current knowledge** - what the user knows now. Rebuild it from `STATE.md`, the learning records and the latest assignment feedback. On the first cycle there is no evidence yet, so ask a few diagnostic questions and record their prior knowledge as learning records. Compare what they know with the mission's "Success looks like" and re-rank the gaps: the top gap is the most mission-relevant one inside their [zone of proximal development](#zone-of-proximal-development). Use the progress log to calibrate: repeated revisions mean the steps are too big, quick passes mean you can stretch. Tell the user in two or three lines what they can do now and which gap is next. They may redirect.
2. **Lesson** - target the next gap. Write one [lesson](#lessons) that closes the top gap and nothing else.
3. **Assignment** - apply the concept. Write the lesson's [assignment](#assignments), then hand the user both. If any reviews are due, start the assignment with a short retrieval warm-up on them. The user reads the lesson, completes the assignment and tells you when it's done, often in a later session. Answer their questions along the way with hints, not solutions. Review the submission against its criteria as soon as it arrives. If it falls short, give specific feedback and let them revise.
4. **Updated State** - record progress. Write learning records for what the assignment proved or corrected. Then update `STATE.md`: current knowledge, gaps (close the one just passed, add any the assignment revealed), reviews (reschedule from the warm-up, schedule the new concept) and one progress-log row. Tell the user what changed.
5. **Go to step 1 and continue.** If the user wants another cycle now, keep going. If not, stop: `STATE.md` already says where the next session starts.

The user's side of the loop is active learning: **read the lesson → complete the assignment → update the learning state**. Don't start the next lesson while an assignment is open. If the user wants to move on anyway, mark it `moved on` and keep its gap in `STATE.md`.

### Starting a session

Read `STATE.md` before anything else.

- **It exists**: recap in two lines what the user knows now and where they are in the loop, then resume at the recorded step. Never re-teach what it records as known.
- **It doesn't exist**: if `MISSION.md` is missing too, establish [the mission](#the-mission) first. Then create `STATE.md` and begin at step 1. If the workspace already has lessons or learning records (for example from `/teach`), build `STATE.md` from them instead of starting from zero.
- **The user named something to learn**: make it the top gap if it fits the mission. If an assignment is still open, ask whether to finish it first or move on.

## Philosophy

To learn at a deep level, the user needs three things:

- **Knowledge**, captured from high-quality, high-trust resources
- **Skills**, acquired through highly-relevant interactive lessons devised by you, based on the knowledge
- **Wisdom**, which comes from interacting with other learners and practitioners

Before the `RESOURCES.md` is well-populated, your focus should be to find high-quality resources which will help the user acquire knowledge. Never trust your parametric knowledge.

Some topics may require more skills than knowledge. Learning more about theoretical physics might be more knowledge-based. For yoga, more skills-based.

### Fluency vs Storage Strength

You should be careful to split between two types of learning:

- **Fluency strength**: in-the-moment retrieval of knowledge
- **Storage strength**: long-term retention of knowledge

Fluency can give the user an illusory sense of mastery, but storage strength is the real goal. Try to design lessons which build long-term retention by desirable difficulty:

- Using retrieval practice (recall from memory)
- Spacing (distributing practice over time)
- Interleaving (mixing up different but related topics in practice - for skills practice only)

## Lessons

A lesson is the main thing you produce: the unit in which knowledge and skills reach the user. Each lesson is one self-contained HTML file, saved to `./lessons/` and titled `0001-<dash-case-name>.html` where the number increments each time.

A lesson should be **beautiful**, with clean, readable typography and layout, since the user will return to these later to review. Think Tufte.

The lesson should be short, and completable very quickly. Learners' working memory is very small, and we need to stay within it. But each lesson should give the user a single tangible win that they can build on. It should be directly tied to the mission, and should be in the user's zone of proximal development.

If possible, open the lesson file for the user by running a CLI command.

Each lesson should link via HTML anchors to other lessons and reference documents.

Each lesson should recommend a primary source for the user to read or watch. This should be the most high-quality, high-trust resource you found on the topic.

Each lesson should contain a reminder to ask followup questions to the agent. The agent is their teacher, and can assist with anything that's unclear.

Each lesson should end with a **Your assignment** box: the task in a sentence or two, a link to its file in `./assignments/`, and how to hand it in.

## Assignments

An assignment is where the user applies a lesson. The lesson builds fluency; the assignment produces the evidence that moves `STATE.md` forward. Every lesson gets exactly one, saved to `./assignments/` under the lesson's number and slug, in the format in [ASSIGNMENT-FORMAT.md](./ASSIGNMENT-FORMAT.md).

It should put the concept to work on a real or realistic task from the mission, a little beyond the lesson's worked examples, with success criteria you can check from what the user submits. Keep it as small as the lesson: one sitting, one tangible win.

This is the feedback loop that carries across sessions, so keep it tight: review each submission as soon as it's handed in, and say exactly what worked and what didn't. For coding topics, every assignment also ships with starter code and tests the user can run before handing it in, and you run the same tests when you review.

## Assets

Lessons are built from reusable **components**, stored in `./assets/`: stylesheets, quiz widgets, simulators, diagram helpers, and anything else a second lesson could reuse.

Reuse is the default, not the exception. Before authoring a lesson, read `./assets/` and build from the components already there. When a lesson needs something new and reusable, write it as a component in `./assets/` and link to it; never inline code a future lesson would duplicate.

A shared stylesheet is the first component every workspace earns: every lesson links it, so the lessons look like one consistent course rather than a pile of one-offs. As the workspace grows, so should the component library.

## The Mission

Every lesson should be tied into the mission - the reason that the user is interested in learning about the topic.

If the user is unclear about the mission, or the `MISSION.md` is not populated, your first job should be to question the user on why they want to learn this.

Failing to understand the mission will mean knowledge acquisition is not grounded in real-world goals. Lessons will feel too abstract. You will have no way of judging what the user should do next.

Missions may change as the user develops more skills and knowledge. This is normal - make sure to update the `MISSION.md` and add a learning record to capture the change. Confirm with the user before changing the mission.

## Zone Of Proximal Development

Each lesson, the user should always feel as if they are being challenged 'just enough'.

The user may specify an exact thing they want to learn. If they don't, figure out their zone of proximal development by:

- Reading `STATE.md` and their `learning-records`
- Figuring out the right thing to teach them based on their mission
- Teach the most relevant thing that fits in their zone of proximal development

## Knowledge

Lessons should be designed around a skill the user is going to learn. The knowledge in the lesson should be only what's required to acquire that skill. You teach the knowledge first, then get the user to practice the skills via an interactive feedback loop.

Knowledge should first be gathered from trusted resources. Use `RESOURCES.md` to keep track of them. Lessons should be littered with citations - links to external resources to back up any claim made. This increases the trustworthiness of the lesson.

For acquiring knowledge, difficulty is the enemy. It eats working memory you need for understanding.

## Skills

If knowledge is all about acquisition, skills are about durability and flexibility. Make the knowledge stick.

For skill acquisition, difficulty is the tool. Effortful retrieval is what builds storage strength. Skills should be taught through interactive lessons. There are several tools at your disposal:

- Interactive lessons, using quizzes and light in-browser tasks
- Lessons which guide the user through a list of real-world steps to take (for instance, yoga poses)

Each of these should be based on a **feedback loop**, where the user receives feedback on their performance. This feedback loop should be as tight as possible, giving feedback immediately - and ideally automatically.

For quizzes, each answer should be exactly the same number of words (and characters, if possible). Vary which position holds the correct answer across questions. Don't give the user any clues about the answer through formatting or order.

## Acquiring Wisdom

Wisdom comes from true real-world interaction - testing your skills outside the learning environment.

When the user asks a question that appears to require wisdom, your default posture should be to attempt to answer - but to ultimately delegate to a **community**.

A community is a place (online or offline) where the user can test their skills in the real world. This might be a forum, a subreddit, a real-world class (budget permitting) or a local interest group.

You should attempt to find high-reputation communities the user can join. If the user expresses a preference that they don't want to join a community, respect it.

## Reference Documents

While creating lessons, you should also create reference documents. Lessons can reference these documents - they are useful for tracking raw units of knowledge useful across lessons.

Lessons will rarely be revisited later - reference documents will be. They should be the compressed essence of the lesson, in a format designed for quick reference.

Some learning topics lend themselves to reference:

- Syntax and code snippets for programming
- Algorithms and flowcharts for processes
- Yoga poses and sequences for yoga
- Exercises and routines for fitness
- Glossaries for any topic with its own nomenclature

Glossaries, in particular, are an essential reference. Once one is created, it should be adhered to in every lesson.

## `NOTES.md`

The user will sometimes express preferences of how they want to be taught, or things you should keep in mind. This is the place to record those preferences, so you can refer back to them when designing lessons or working with the user.
