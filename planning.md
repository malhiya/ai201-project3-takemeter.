# TakeMeter

This project focuses on r/TrueFilm, an online community dedicated to deep, text-heavy cinematic discourse that acts as an intellectual alternative to casual movie forums. The dataset classifies posts into three distinct labels: AUDIENCE_EXPERIENCE (personal emotions and viewing habits), THEMATIC_INTERPRETATION (decoding abstract meaning and subtext), and CRAFT_EVALUATION (analyzing technical execution and industry context). Separating these categories matters deeply to regulars because the community’s internal prestige is built entirely on moving past raw, consumer-level reactions and into rigorous textual or technical analysis.


## Definitions

### 1. AUDIENCE_EXPERIENCE

**Definition:** The text focuses primarily on the viewer's personal, emotional, or physical response to the movie, using first-person perspectives or consumer-grade value judgments.

> **Clear Example 1:** "I watched 'Aftersun' last night and it completely broke me; I spent an hour staring at the ceiling thinking about my own dad and just couldn't shake the heavy feeling of grief."

> **Clear Example 2:** "Unpopular Opinion: I thought pulp fiction was a shit movie overall. I liked the dialogue and the most famous samuel l jackson scene. However I was pretty disappointed with the movie overall."

**The Uncertain Case:** "The third act of 'Interstellar' always makes me cry because the music and the acting perfectly capture the tragedy of lost time."

**The Boundary Resolution:** Even though the user names an emotional response ("makes me cry"), the sentence structure immediately shifts to attribute that emotion to specific filmmaking mechanics (the music, the acting). Because the core intent is explaining how the movie achieves its effect rather than detailing the user's personal life, this is classified as `CRAFT_EVALUATION`.

---

### 2. THEMATIC_INTERPRETATION

**Definition:** The text seeks to decode the implicit subtext, cultural symbolism, ideological messaging, or abstract themes intended by the filmmakers or found within the text.

> **Clear Example 1:** "The recurring motif of water in 'Parasite' isn't just about the weather; it symbolizes the downward flow of systemic wealth and how capitalism inevitably drowns the lower class while barely splashing the rich."

> **Clear Example 2:** "So I ran into an article about the film Obsession and the headline immediately grabbed my attention: 'Obsession is the GET OUT for White people.' I just saw Obsession and outside of the dinner table scene, I don’t see the comparison."

**The Uncertain Case:** "This was my introduction to the book adaptation 'The Lost Daughter.' If you want an explanation of why she takes the doll, why she abandoned her daughters, and why she goes to the beach, it acts as an epilogue to Ferrante's other work."

**The Boundary Resolution:** This comment is a recommendation that details plot mechanics ("why she takes the doll"). However, because it uses an external text specifically to provide a roadmap for decoding character motivations and thematic subtext, it functions as `THEMATIC_INTERPRETATION`.

---

### 3. CRAFT_EVALUATION

**Definition:** The text evaluates the structural, technical, historical, or commercial execution of a film, an actor's performance, a director's career, or industry distribution methods.

> **Clear Example 1:** "Generally, a script for any good film is very tight and cut to the bone. However, the cinematography and pacing are also crafted to help the viewer actually digest the beats and conversations in the movie."

> **Clear Example 2:** "My claim is that no actor in world cinema has a stronger run of consecutive films than Al Pacino between 1971 and 1975... with no weak link in between."

**The Uncertain Case:** "The messy editing in 'The Nomad' perfectly mirrors the chaotic, fractured mental state of the main character."

**The Boundary Resolution:** This is a classic hybrid sentence. It evaluates a technical craft element ("messy editing"), but immediately links it to an interpretive thematic meaning ("mirrors the mental state"). By applying the Tool vs. Target Rule, we see that editing (the craft tool) is being analyzed solely to explain a theme. Therefore, this is classified as `THEMATIC_INTERPRETATION`.

---

### Handling Ambuguity
We resolve this ambiguity by determining the ultimate target of the sentence.

If the technical filmmaking element is mentioned solely as a vehicle to reveal a deeper subtext or character psyche, it is labeled THEMATIC_INTERPRETATION. If the technical element is mentioned to criticize the movie’s structural construction, pacing, or execution quality, it is labeled CRAFT_EVALUATION. Finally, if the technical choice is cited primarily to explain a physical or emotional reaction in the viewer—such as causing boredom, confusion, or tears—it is labeled AUDIENCE_EXPERIENCE.

## Setup
