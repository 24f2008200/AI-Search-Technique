AI: Search Methods for Problem Solving · Prof. Deepak Khemani · IIT Madras

# AI Week 1 Exam Notes

Covers all seven Week 1 lectures: Introduction, History and Philosophy, Minds and Machines, Modern Times, Problem Solving, Human Cognitive Architecture, A Decade of Machine Learning. Everything below comes from the transcripts. Boxes marked EXAM are the points most likely to be asked.

[Summary](#summary) [Agents](#agents) [What is AI](#definitions) [Can machines think](#think) [Turing and Winograd](#turing) [Philosophers](#philosophy) [Machines timeline](#machines) [Dartmouth onward](#modern) [Reality and models](#reality) [Symbols](#symbols) [Problem solving](#problem) [Machine learning](#ml) [Who said it](#whosaid) [Self-test](#selftest) [Check these](#caveats)

## The whole week in ten lines

1. The course goal is to build an **intelligent agent**: persistent, autonomous, proactive and goal directed.
2. An agent keeps a **model of the world** in its head. The model is an abstraction. A **self-aware** agent also models itself inside that model.
3. Intelligence needs three things: **remember the past and learn from it**, **understand the present** (knowledge representation and inference), and **imagine the future** (planning and search).
4. The course takes the **information processing view**: sense (signals), turn signals into symbols (neural networks), reason with symbols, act (signals again).
5. Whether machines can think is old and unsettled. Dreyfus, Searle and Penrose argue no. Turing sidesteps it with a behavioural test.
6. The **Turing Test** was weakened by chatbots such as Eliza. Levesque proposed **Winograd schemas** (anaphora resolution) because they need world knowledge and cannot be solved by searching the web.
7. The idea that **thinking is symbol manipulation** runs from Hobbes and Descartes through Leibniz, Babbage and Ada Lovelace to Simon and Newell.
8. **1956, Dartmouth**: John McCarthy named the field. Simon and Newell brought the Logic Theorist and the physical symbol system hypothesis.
9. This course studies **problem solving by search** (first principles, model based) under simplifying assumptions. Knowledge based and learning based methods are the other branch.
10. The last decade belongs to **deep learning**. It is strong at pattern recognition (animal-like abilities) but brittle and lacks general world knowledge.

## Intelligent agents

Core definition

An intelligent agent is a program that is **persistent** (there all the time), **autonomous** (acts on its own), **proactive** (decides which goals to pursue next) and **goal directed** (follows its goals, which may be given by human users). Humans satisfy all of these most of the time, so humans are agents too.

Exam

- Agent properties: persistent, autonomous, proactive, goal directed. An item listing these four is "all of the above".
- An agent must carry a **model of the world** and reason over it. The model is an **abstraction**, since no model can represent reality exactly.
- A **self-aware agent models itself in the world model**. The lecture does not define self-awareness as consciousness of the meaning or purpose of life.

### Three layers of an autonomous agent

| Layer | Role | Technology |
| --- | --- | --- |
| Outer | Signal processing: perceive and act through signals | Vision, speech, touch, motor control |
| Middle | Neuro-fuzzy: turn signals into symbols | Neural networks, deep learning |
| Inner | Symbolic reasoning | Search, planning, logic, constraints (this course) |

The loop is: sense the world, build an internal representation, deliberate on it, act on the world. Marvin Minsky's *Society of the Mind* is cited for the idea that the mind is many parts, so AI is not one monolithic algorithm.

### The three components of intelligence

### Remember the past

Learn from it. Case-based reasoning stores cases directly. Machine learning generalises from experience. Recognising objects, faces and patterns belongs here (deep neural networks).

### Understand the present

Be aware of the world and create a model of it. This is knowledge representation. Make inferences with logic and reasoning.

### Imagine the future

Ask what happens if you take certain actions. Choose a sequence of actions that reaches the goal. This is planning and search, and it is the focus of the course.

Exam

Planning systems compute a **sequence of moves for the future**. They do not recall past experience. A neural network model is a representation of **past experience**, stored in its weights, not of future outcomes and not made of biological neurons.

## What is AI

| Who | Definition or claim |
| --- | --- |
| Herbert Simon | Programs are intelligent if they show behaviour that would be called intelligent in a human. |
| Barr and Feigenbaum | Physicists ask what kind of place the universe is, biologists what it means to be living. AI asks what kind of information processing system can ask such questions. |
| Elaine Rich | AI is the study of techniques for solving exponentially hard problems in polynomial time by exploiting knowledge about the problem domain. This costs solution quality: good, not necessarily optimal, answers. Example: Travelling Salesman. |
| Charniak and McDermott | AI is the study of mental faculties through computational models. This is the cognitive emphasis. |
| John Haugeland | The goal is machines with minds of their own, "the genuine article", not a clever fake. His view: thinking and computing are radically the same. Book: *AI: The Very Idea*. The lecturer's favourite definition. |

Two emphases to remember: writing programs that do useful things, versus understanding thinking and intelligence. A "machine" here means a programmable computer, a general-purpose or universal machine. Anything that operates by rules in a repeatable fashion counts as mechanical.

A student offered another definition: a good AI finds differences between similar situations and similarities between different ones. The lecturer linked the second half to **analogy**.

Two books recommended for history and philosophy: Haugeland's *AI: The Very Idea* and Pamela McCorduck's *Machines Who Think* (note "who", not "which").

## Can machines think

People have asked this for at least 80 years. Haugeland's follow-up: if yes, are we also machines? The DNA argument says we grow from simple cells under instructions written in DNA, so we are machines in some sense.

| Who | Argument against machine intelligence |
| --- | --- |
| Herbert Dreyfus | Intelligence depends on unconscious instincts that cannot be captured in formal rules (tacit knowledge). |
| John Searle | **Chinese Room**: a person who knows no Chinese follows rule slips to turn Chinese questions into Chinese answers. He does not understand Chinese, and computers are likewise pattern matchers. Criticism: such a room would need an infeasible number of rules. |
| Roger Penrose | Something quantum mechanical happens in the brain that current physics cannot explain. |
| Others | Emotion, intuition, consciousness, self-awareness, ethics. |

## Turing Test, Eliza and Winograd schemas

### Turing Test (the Imitation Game)

- Alan Turing said the question "can machines think?" is "too meaningless". His paper was *Computing Machinery and Intelligence* (1950).
- A human judge on a teletype (today, a chat) tries to tell whether the other party is a human or a machine. If a machine convinces the judge often enough, it is intelligent.
- It is a **behavioural evaluation**, not a definition of intelligence. It works through natural language.
- Weakness raised: ask for the product of two 11-digit numbers and the machine's speed gives it away. Shakuntala Devi is the exception.
- The film *The Imitation Game* is about Turing breaking German codes, not about the test.

Exam

The Turing Test evaluates a machine's ability to exhibit intelligent behaviour indistinguishable from a human's. It was not for recruiting human computers, finding double agents, or assessing government workers.

### Chatbots

- **Eliza** (1966, Joseph Weizenbaum, MIT) used simple rules to rework the user's own words. The "Doctor" script imitated a Rogerian psychotherapist. Named after Eliza Doolittle in Shaw's *Pygmalion*. Weizenbaum was disturbed that secretaries confided in it, and wrote *Computer Power and Human Reason*.
- **Loebner Prize**: an open competition run like a Turing test. The 2013 transcript shown was from a program called Izar.
- GPT-3 and Alexa were cited as modern examples of human-like language.

### Winograd schemas (Hector Levesque)

Levesque (book: *Common Sense, the Turing Test, and the Quest for Real AI*) argued that chatbots are impressive but lack intelligence. His alternative test is built on **anaphora resolution**.

- Each schema has two noun phrases of the same class, an ambiguous pronoun, and a **special word** that can be swapped for an alternate word. The swap changes who the pronoun refers to.
- It is a binary choice, and it is **Google-proof**: a large text corpus should not help. Contrast IBM Watson, which won Jeopardy by searching.
- Answering well "requires a certain amount of world knowledge".
- Named after Terry Winograd (1972, early NLP researcher).

| Sentence | Pronoun resolves to |
| --- | --- |
| The city council refused the demonstrators a permit because they *feared* violence. | the council |
| ...because they *advocated* violence. | the demonstrators |
| John took the water bottle out of the backpack so that it would be *lighter*. | the backpack |
| ...so that it would be *handy*. | the water bottle |
| The trophy would not fit in the brown suitcase because it was too *small* / too *big*. | the suitcase / the trophy |
| The lawyer asked the witness a question, but he was reluctant to *repeat* / *answer* it. | the lawyer / the witness |

Exam

Answering such pronoun questions needs **a lot of common-sense knowledge about the world**. It is not about searching the internet, and the lecture frames it as knowledge, not as grammar or data science. "Tyson told Douglas that he won" is genuinely ambiguous, so "cannot say".

## Philosophers: thinking as symbol manipulation

| Who | Idea |
| --- | --- |
| Copernicus | Per Haugeland, put the first wedge between thought and reality. The earth rotates, so the sun's motion is an illusion: what you see is not what is out there. |
| Galileo (1623) | Tastes, odours and colours are "mere names" that reside in consciousness. Remove the living creature and they vanish. The universe is written in the language of mathematics. He used geometry to reason about motion. |
| Thomas Hobbes | "By reasoning I understand computation." Reasoning is adding and subtracting. Called the **grandfather of AI** by Haugeland. In his time "computers" were people who did arithmetic. The comic strip *Calvin and Hobbes* is named after him. |
| René Descartes | Animals are wonderful machines. Thoughts are **symbolic representations**. Everything, even thought, is applied maths (geometry represented by algebra). A symbol and what it symbolises are different things. Also "cogito ergo sum". |
| Leibniz | Much reasoning can be reduced to calculation. Logic should settle disputes: "let us calculate". Also built the stepped drum (see below). |

Problems Descartes could not answer

- **Mind-body problem**: how can thought (symbols) influence the physical body, and the reverse?
- **Paradox of mechanical reason**: reasoning is meant to be both mechanical (automatic rules) and meaningful. Who manipulates the symbols? Opponents joked about a **homunculus** (little man) in the head.

Reading suggested on the self: Douglas Hofstadter, *Gödel, Escher, Bach* (Pulitzer), *The Mind's I* (with Daniel Dennett), *I Am a Strange Loop*.

## Machines through history

### Artificial people in myth and folklore

Talos, the bronze man made by Hephaestus (Homer). Pandora. Pygmalion and Galatea. Daedalus's lifelike statues. Pope Sylvester II's talking head. Paracelsus's homunculus. Rabbi Judah Loew's Golem.

### Real mechanisms

| When | Who or what | Why it matters |
| --- | --- | --- |
| c. 1300 on | Zairja (Arab astrologers); Ramon Lull's Ars Magna | Mechanical idea generators using rotating circles. Lull wanted reason applied to all subjects "without the trouble of thinking". *Al-jabr* gives the word algebra. |
| 14th c. | Clockwork automata in European towns | Strasbourg, Nuremberg, Lübeck. |
| 1642 | Blaise Pascal: Pascaline | Lantern gears. Adds and subtracts directly, multiplies and divides by repetition. About 20 sold, then too costly. |
| 1673 | Leibniz: stepped drum / Leibniz wheel; Stepped Reckoner | Digital mechanical calculator, 8-digit numbers. The drum was used for about three centuries, until the mid-1970s. |
| 18th c. | Vaucanson's duck | Appeared to eat, quack and digest. It could not actually digest. |
| 1770 | Von Kempelen's Chess Playing Turk | A hoax: a small man hid inside. It beat Napoleon and Franklin. The name "Mechanical Turk" (crowdsourcing) comes from it. |
| 19th c. | Thomas de Colmar: Arithmometer | Sturdy design that took calculation from human computers to machines. |
| 19th c. | Charles Babbage: Difference Engine | Computes polynomial values automatically. 25,000 parts, about 13,000 kg, 8 feet tall. A working one is in London's Science Museum. |
| 19th c. | Jacquard loom | Punched cards control the pattern. The idea carried into computer input until the mid-20th century. |
| 19th c. | Babbage: Analytical Engine | Never built. First design of a general-purpose, Turing-complete machine: arithmetic and logic unit, control flow, conditional branching, loops, memory. Fully mechanical. |
| 19th c. | Augusta Ada Byron (Ada Lovelace) | Wrote what is recognised as the first algorithm for a machine, so the first programmer. Realised machines could go beyond numbers, even composing music. The language Ada is named after her. |
| 1940s | ENIAC | First electronic general-purpose computer. 17,000+ vacuum tubes, 7,000+ crystal diodes, 27 tonnes, 150 kW. |

Exam

- **Leibniz** invented the stepped drum (1673).
- **Ada Lovelace (Augusta Ada Byron)** suggested the Analytical Engine might act on things besides numbers and compose music.
- Pascal built the first calculator in this lineage. Babbage designed the Analytical Engine. He did not build it.

## Dartmouth and the symbolic AI era

The **Dartmouth Conference, 1956**, was a two-month, ten-man study of AI based on the conjecture that every aspect of learning or intelligence can in principle be described precisely enough for a machine to simulate it. **John McCarthy** is credited with choosing the name "artificial intelligence". A follow-up conference was held 50 years later.

| Person | Known for |
| --- | --- |
| John McCarthy | Driving force and organiser. Named AI. Designed Lisp. Worked on logic and commonsense reasoning. Co-founded the MIT AI Lab with Minsky. |
| Marvin Minsky | Frame systems (foundation of object-oriented programming), *Society of the Mind*, *The Emotional Machine*. Died 2016. |
| Nathaniel Rochester | IBM engineer who designed the IBM 701 and wrote its first assembler. Supervised Arthur Samuel. |
| Claude Shannon | Father of information theory (entropy), at Bell Labs. Hired McCarthy and Minsky as students. |
| Arthur Samuel | Wrote a checkers program with a **learning component**. It improved as it played and **eventually beat Samuel**. This fed a "Frankenstein" fear of AI. |
| Herbert Simon, Allen Newell, J. C. Shaw | The "show stealers" at Dartmouth (per McCorduck). Built the **Logic Theorist (LT)**, written in Newell's language IPL. It is the first program deliberately engineered to mimic human problem solving, and it proved theorems from Russell and Whitehead's *Principia Mathematica*, sometimes with shorter proofs. Later built the **General Problem Solver** (heuristic search, **means-ends analysis**). Simon won a Nobel Prize in economics. |
| John Laird | SOAR, a cognitive architecture from CMU you can still download. |

Physical symbol system hypothesis (Simon and Newell)

A **physical symbol system has the necessary and sufficient means for general intelligent action**. A symbol system is a pattern of symbols obeying formal laws (words, lists, a musical tune, long division, an abacus). This leads to **symbolic AI, classical AI, or GOFAI** (Good Old-Fashioned AI, Haugeland's term). It contrasts with machine learning, where a neural network keeps knowledge as weights (the **sub-symbolic** level).

Exam

- Logic Theorist: a program that finds proofs, designed by Simon and Newell (with Shaw). It is not a person and not a chess player.
- Samuel's 1952 checkers program improved with play and eventually beat its creator.
- Simon and Newell proposed the physical symbol system hypothesis. They did **not** coin "AI" (McCarthy), write *Machines Who Think* (McCorduck), or invent the Analytical Engine (Babbage).

## Reality, models and ontologies

- Physics says everything is fundamental particles. A human adult has about 1027 atoms, so equations at that level would be useless.
- The meaningful world is **in our minds**. Seeing a tree, a cloud or a chair is our own creation (an extension of Galileo). The film *The Matrix* is the illustration.
- The *Powers of Ten* film zooms from quarks and gluons up to 1026 m, the visible universe. Humans perceive only a narrow band, from roughly a pollen grain (10-4 m) to a mountain range (104 m). Science extends it.
- An agent cannot represent the world as particles. It represents **atoms, molecules, cells, organs, creatures, societies**, depending on its purpose.
- Every discipline defines its own vocabulary, its **ontology**. Representations are dictated by what you want to do with them.

## Symbols, representation and reasoning

- A **symbol is something perceptible that stands for something else**. Its meaning is socially agreed, not intrinsic (road signs, numerals, Shakespeare's rose by any other name).
- **Number versus numeral**: 7, VII and "seven" are different numerals for one concept.
- Languages are semiotic systems. Biosemiotics: complex behaviour from simple parts talking through signs (billions of neurons, ant pheromone trails, which return in ant colony optimisation).
- **Reasoning** means formal methods: manipulating symbols meaningfully (multi-digit multiplication is a worked example).
- Knowledge here is **declarative and explicit** (sentences in a language), not procedural (riding a bicycle) and not tacit.
- Inference types: **deductive** (all men are mortal, Socrates is a man, so Socrates is mortal) and **plausible or probabilistic** (clouds, so likely rain).

### AI, ML, automation, data

AI covers symbolic knowledge representation and problem solving. ML focuses on making sense of data (classification, prediction, recommenders). ML is one component of AI and has been part of it from the start. Automation overlaps with AI (self-driving cars use pattern recognition) but train reservation or online shopping systems are mostly not AI. Data science sits partly in statistics, ML and AI.

## Problem solving

Definition

An **autonomous agent in some world has a goal** (a desired state of affairs) **and a set of actions to choose from**. The problem solver decides which actions to take. The lecture's example is a striker passing to a free teammate.

### Simplifying assumptions for this course

- The world is **static**: nothing changes unless the agent acts.
- The world is **completely known**.
- **One agent** changes the world. The only multi-agent case is two-player games such as chess.
- **Actions never fail**.
- Representation of the world is taken care of. This is why chess was a popular early domain: moves are easy to describe and make, unlike football.

None of these hold in the real world. Real agents see incomplete information, face other agents, and must monitor whether actions succeed.

### Two ways to solve problems

|  | First principles (this course) | Knowledge and experience based |
| --- | --- | --- |
| Also called | Model-based reasoning, search, trial and error | Memory-based reasoning |
| Idea | Reason over a model and try options | Do not reinvent the wheel: reuse stored experience |
| Fields | Search, planning, games, constraints | Case-based reasoning, machine learning (rules, decision trees, neural networks) |
| Example | Solving Rubik's cube without knowing the method | A child who knows the solution matches patterns |

- Rubik's cube: invented 1974 by Arnold Rubik, who took a month to find a first solution. A deep reinforcement learning system later solved it with no human guidance.
- **Supervised** deep learning needs labelled examples. **Reinforcement learning** removes that human input.
- **Sudoku** combines search with reasoning (narrowing options for a square). That combination is captured by **constraint satisfaction**, the last topic of the course.
- **Map colouring** becomes a **constraint graph**: regions are nodes, each with allowed colours, and adjacent nodes get a "not equal" edge. The four-colour theorem was proved by a computer program.
- Logic plus search give deduction. Search, deduction and more are subsumed by constraint processing.

### Course roadmap

State space search, DFS, BFS, iterative deepening, heuristic search, local and stochastic local search, genetic algorithms, ant colony optimisation, A\*, space-saving A\*, sequence alignment, game playing, planning, AO\* (backward search), forward chaining and the Rete algorithm, constraint processing. The recurring enemy is **combinatorial explosion**. Textbook: *A First Course in Artificial Intelligence* by Khemani.

## A decade of machine learning

### Why now

Explosion of data from the internet, more computing power, and better neural network training algorithms. This gives some people the impression that AI is only ML.

### Neural network timeline (as given in the lecture)

- A neuron computes a function of its inputs, typically non-linear (sigmoid). Simple units together do complex things (emergence). The brain is the living proof.
- The Perceptron: a single-layer binary classifier. It is **linear**, so it only separates linearly separable classes (the XOR-style four-point picture).
- **Hidden layers**: Rumelhart, Hinton and Williams showed a multi-layer perceptron (feed-forward network) can learn any non-linear classifier. They popularised **backpropagation**: compare output with the desired output and propagate the error back, adjusting weights.
- Hinton showed around 2012 that deep networks excel at computer vision.
- **Hinton, LeCun and Bengio** received the Turing Award in 2018.
- Everything a network knows is stored in the **weights**. The output "horse" is just a label we gave a node. A symbol has no intrinsic meaning.

### Applications and cautions

- Image labelling, face recognition, speech, disease diagnosis (breast cancer from images, Face2Gene for syndromes). A network can absorb the experience of thousands of doctors who labelled images.
- Surveillance by governments. Big tech treats users as data, for example to target ads.
- These are **animal-like abilities** (Darwiche, 2018; Judea Pearl on eagles and snakes). Humans differ by planning lives, goals, diverse societies and accumulating wealth across generations. The cognitive abilities wanted in an agent are goal directed, autonomous action.
- **Performance versus competence** (Rodney Brooks): a network that labels "people playing Frisbee in a park" cannot answer whether a Frisbee can be eaten. It lacks general world knowledge.
- **Suitcase words** (Minsky): "learning" carries many meanings. Machine learning is **brittle**: each problem needs its own architecture, data and training.

### Games

**IBM Deep Blue beat world champion Kasparov in 1997** in a six-game match. People then expected Go (19 by 19 board, huge search space) to be harder. **AlphaGo** (DeepMind, 2016) won, a triumph of reinforcement learning. AlphaGo Zero learned without human help, and AlphaZero learned several games.

Exam

First AI agent to show machines can beat the very best humans at chess: **Deep Blue** (not Blue Gene, Chess Machine or Deep Thought).

## Who said it

| Quote or fact | Answer |
| --- | --- |
| Tastes, odours, colours reside in consciousness | Galileo Galilei |
| Reasoning is computation | Thomas Hobbes |
| Thoughts are symbolic representations | René Descartes |
| Stepped drum, 1673 | Leibniz |
| Engine might act on things besides numbers; compose music | Ada Lovelace (Augusta Ada Byron) |
| "Can machines think" is too meaningless | Alan Turing |
| Named "artificial intelligence" | John McCarthy |
| Physical symbol system hypothesis | Simon and Newell |
| Chinese Room | John Searle |
| Quantum effects in the brain | Roger Penrose |
| Intelligence rests on unconscious instincts | Hubert Dreyfus (the transcript says "Herbert") |
| Winograd schema challenge | Hector Levesque |
| Eliza | Joseph Weizenbaum, 1966 |
| "Machines with minds of their own"; GOFAI | John Haugeland |
| Suitcase words; Society of the Mind | Marvin Minsky |
| Performance versus competence (Frisbee) | Rodney Brooks |
| Animal-like abilities, 2018 | Adnan Darwiche (transcript: "Darwiche") |

## Self-test

Try to answer before opening each item.

List the four properties of an intelligent agent.

Persistent, autonomous, proactive, goal directed.

What makes an agent self-aware, in this lecture?

It models itself inside its own model of the world. A world model alone is not enough.

Map the three components of intelligence to AI fields.

Past: case-based reasoning, machine learning, neural networks. Present: knowledge representation, logic, inference. Future: planning and search.

Why did Levesque propose Winograd schemas?

Chatbots can pass a Turing-style test without intelligence. Winograd schemas need world knowledge and cannot be solved by searching a corpus.

State the Chinese Room argument and one criticism.

A person following rules can answer Chinese questions without understanding Chinese, so computers merely pattern match. Criticism: it would take an infeasible number of rules.

What are the mind-body problem and the paradox of mechanical reason?

How a thinking mind (symbols) influences the physical body. And how reasoning can be both mechanical and meaningful, with the homunculus joke about who manipulates the symbols.

State the physical symbol system hypothesis.

A physical symbol system has the necessary and sufficient means for general intelligent action.

List the simplifying assumptions in this course.

Static world, completely known, single agent, actions never fail, representation taken care of.

Contrast first-principles and knowledge-based problem solving.

First principles: search over a model, trial and error, no prior solution. Knowledge based: reuse stored experience through case-based reasoning or machine learning.

Why is a neural network's output symbol "meaningless" by itself?

A symbol has no intrinsic meaning. "Horse" is just our label on an output node, and the network's knowledge sits in its weights (sub-symbolic).

What is the difference between performance and competence?

A network can label an image correctly (performance) without the general knowledge a person would have (competence), such as whether a Frisbee can be eaten.

## Check these against your slides

The transcripts are auto-generated and a few names and dates look garbled. Where the lecture and the standard record differ, use whichever your slides and instructor use.

- **Perceptron.** The transcript credits McCulloch and Pitts (1943) with the perceptron and dates Minsky and Papert's linear-separability result to 1958. In the standard record, McCulloch and Pitts proposed the artificial neuron (1943), Rosenblatt built the perceptron (1958), and Minsky and Papert's book came in 1969.
- **Haroun al-Rashid clock.** The transcript says 1802, and the standard account places that gift to Charlemagne in about 807.
- **Names.** "Herbert Dreyfus" is Hubert Dreyfus. The book is *Computer Power and Human Reason*. "Allen Newell" appears as "Alan Newell" in the transcript. The spoken name "Michael Opitz" in the numbers anecdote is unclear.
- **Logic Theorist and Samuel's program.** Quiz options can accept more than one true statement, so read the question's wording on single versus multiple answers.