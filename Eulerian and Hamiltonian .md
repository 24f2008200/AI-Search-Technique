Yes! Eulerian and Hamiltonian graphs are closely related because both study whether a graph can be traversed in one complete tour. The crucial difference is what the tour must cover.

## 1. The key difference

[PPT - Graphs PowerPoint Presentation, free download - ID:5857327](https://images.openai.com/static-rsc-4/ooM7IA0Uyp-lcrlDdfh-WZ8OHG2DZPL6esvEeAlXoSqWJxXgHaWUB_EjrPn6OiimWq-2BKmcksMwpLU2qteo2T9ROgwCBqwCZLNjjFJb4qiByXvnZ1g4oHTMj7On0yO03qYvjR-VDB75xshQ8gssj_jr1Jmtp2UwdAkNe2ttuGE?purpose=inline)

Eulerian cycle

Every edge is traversed exactly once, and you return to the starting vertex.

[Hamiltonian Cycle - DEV Community](https://images.openai.com/static-rsc-4/TcNxnah-vPuXy2D1fxIQjDC2IQIW-9XTntbq2FRjWJLLOvupizB4DROJejw6gn7eSFDB9BVxt6NurnZtwPfSTofZR4xu87J9ZzSxE4m_0gHR8K4q6Cz32rcV4EC_lJBZuEOLYE33mxIUXRFhYSTd6yynOvYkKdyhl4YVceQssO0?purpose=inline)

Hamiltonian cycle

Every vertex is visited exactly once, and you return to the starting vertex.

A handy memory trick:

- Euler = Edges
- Hamilton = Houses (vertices)

Imagine a delivery person:

- Eulerian: travel along every road exactly once.
- Hamiltonian: visit every house exactly once, returning to the starting house.

## 2. Formal definitions

| Property                        | Eulerian cycle                                                         | Hamiltonian cycle                                     |
| ------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------- |
| What must be covered?           | Every edge                                                             | Every vertex                                          |
| Must each be used exactly once? | Every edge                                                             | Every vertex                                          |
| Must return to start?           | Yes                                                                    | Yes                                                   |
| Can vertices be revisited?      | Yes                                                                    | No, except the starting vertex at the end             |
| Can edges be left unused?       | No                                                                     | Yes                                                   |
| Main characterization           | Connected graph with all vertices of nonzero degree having even degree | No similarly simple general characterization is known |

A small distinction: an Eulerian graph may have vertices of degree zero only if we handle isolated vertices separately. In the standard connected-graph definition, all vertices belong to the connected graph and all degrees are even.

## 3. Are they logically related?

Yes, but neither property generally implies the other.

Eulerian but not Hamiltonian

[Full article: m-Bonacci graceful labeling](https://images.openai.com/static-rsc-4/CPq3Qh3txHPhUYH-AIhWZzcD06JcARIstcFbyckWKAG1z5IJuBR2aAXiUcvX47_aLUKg3_sb3fk2CB-osWDhtGsmdjKJbpdeioCJGlO2sMW29NXIn6HuA2ZCHaHgLI9Bs-GseQfuKR1jfiRG7ONDXYdB4Jc3HyTbnmqyqISOmmQ?purpose=inline)

[tandfonline.com](https://www.tandfonline.com/doi/full/10.1080/09728600.2021.1876505)

Example: two triangles sharing one vertex.

Every edge can be traversed exactly once in a closed tour. But the shared vertex would need to be visited twice during the tour, so there is no Hamiltonian cycle.

Hamiltonian but not Eulerian

[File:Complete graph K4.svg - Wikimedia Commons](https://images.openai.com/static-rsc-4/QLUkM0C4jCzks-Ez78dlQL95XrHhRW4Vs9YUOGPtSoWxVYDocM_f1GvvDRX50zsqdaRa6MkL8ADiQw7tM0N5p7oX879Rn3SoTaYtDMcKNKY693xOwAjJouhlfoz6PNaqNTGgCknCh0EFqpepy0mR6ohwyCyh6r_WAnNBMnvW4e0?purpose=inline)

[commons.wikimedia.org](https://commons.wikimedia.org/wiki/File%3AComplete_graph_K4.svg)

Example: \$K_4\$, the complete graph on four vertices.

A cycle can visit all four vertices exactly once. But every vertex has degree \$3\$, which is odd, so it cannot have an Eulerian cycle.

## 4. The exam-ready theorem

For a finite, connected, undirected graph:

- Eulerian cycle exists if and only if every vertex has even degree.
- Hamiltonian cycle exists if every vertex can be included in a cycle that visits each vertex exactly once. There is no equivalent simple degree-parity test for all graphs.

Some useful sufficient conditions for Hamiltonian cycles do exist, such as Dirac’s theorem: for a simple graph with \$n\geq 3\$ vertices, if every vertex has degree at least \$n/2\$, the graph is Hamiltonian.

Bottom line: Eulerian is about using every edge; Hamiltonian is about visiting every vertex. They are related ideas, but they measure different properties of a graph.
