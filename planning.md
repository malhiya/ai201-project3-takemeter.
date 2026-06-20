# TakeMeter

This project focuses on film enthusiats dedicated to deep, text-heavy cinematic discourse that acts as an intellectual alternative to casual movie forums. The dataset classifies posts into three distinct labels: AUDIENCE_EXPERIENCE (personal emotions and viewing habits), THEMATIC_INTERPRETATION (decoding abstract meaning and subtext), and CRAFT_EVALUATION (analyzing technical execution and industry context). Separating these categories matters deeply to regulars because the community’s internal prestige is built entirely on moving past raw, consumer-level reactions and into rigorous textual or technical analysis.

## Community

We chose the broad community of **film enthusiasts**—cinephiles, academics, and dedicated hobbyists who gather online to have deep, analytical conversations about cinema. This community is a strong fit for classification precisely because that wide membership produces wildly varied discourse: a single discussion can swing from a gut-level emotional reaction, to a dense reading of a film's symbolism, to a technical critique of its editing or box-office strategy. That natural spread across personal, interpretive, and craft-focused registers is exactly what makes the labels meaningful, while the frequent hybrid takes that blend all three give the task genuine ambiguity worth resolving.

## Labels

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

## Edge Cases

### Handling Ambiguity

We resolve this ambiguity by determining the **ultimate target** of the sentence:

- If the technical filmmaking element is mentioned solely as a vehicle to reveal a deeper subtext or character psyche, it is labeled `THEMATIC_INTERPRETATION`.
- If the technical element is mentioned to criticize the movie's structural construction, pacing, or execution quality, it is labeled `CRAFT_EVALUATION`.
- If the technical choice is cited primarily to explain a physical or emotional reaction in the viewer—such as causing boredom, confusion, or tears—it is labeled `AUDIENCE_EXPERIENCE`.

---

### Edge Case 1: The "Tool-to-Meaning" Hybrid

> `CRAFT_EVALUATION` vs. `THEMATIC_INTERPRETATION`

**The Scenario:** A post relies heavily on technical filmmaking vocabulary (editing, cinematography, aspect ratios) but uses those terms exclusively to decode a hidden theme or a character's internal state.

**The Ambiguity:** It looks like craft critique due to the advanced technical jargon, but it acts like interpretation because it bypasses evaluating whether the film is "good or bad" to focus on meaning.

> **Example:** "The director utilizes a boxy 1:33 aspect ratio and claustrophobic framing to mirror the protagonist's growing feeling of being trapped by her domestic life."

**The Resolution (The Tool vs. Target Rule):** Identify the ultimate target of the sentence. If a technical filmmaking element is mentioned solely as a vehicle to reveal subtext, symbolism, or psychology, classify it as `THEMATIC_INTERPRETATION`. If the technical element is mentioned to criticize or praise the movie's construction, pacing, or execution quality, classify it as `CRAFT_EVALUATION`.

**Verdict:** `THEMATIC_INTERPRETATION`

---

### Edge Case 2: The "Technical Trigger" Emotion

> `CRAFT_EVALUATION` vs. `AUDIENCE_EXPERIENCE`

**The Scenario:** A post highlights a specific technical flaw or choice but focuses entirely on the negative physical or emotional impact it had on the viewer.

**The Ambiguity:** It lists concrete filmmaking mechanics, but the primary subject of the sentence remains the viewer's personal suffering or detachment.

> **Example:** "The aggressive, loud sound design and the constant rapid cross-cutting in the third act gave me a massive headache and made me want to walk out of the theater."

**The Resolution (The Impact Rule):** If a technical or craft choice is cited primarily to explain or validate a visceral, physical, or emotional reaction in the viewer (such as causing boredom, confusion, headaches, or tears), classify it as `AUDIENCE_EXPERIENCE`. It only becomes `CRAFT_EVALUATION` if the user removes their personal feelings and evaluates the mechanics objectively.

**Verdict:** `AUDIENCE_EXPERIENCE`

---

## Data Collection Plan

To compile the dataset of 200 high-quality, text-heavy examples, data will be collected manually via copy-pasting into a spreadsheet. This keeps the researcher close to the raw data, helping to internalize the linguistic boundaries and nuance of the community before the LoRA training begins.

### 1. Primary Sourcing Location & Search Strategies

The primary focus of this dataset is **r/TrueFilm**, an entirely public, unauthenticated subreddit. To efficiently isolate highly concentrated examples of each label manually rather than scrolling blindly, the following keyword queries and thread structures will be used:

- **`AUDIENCE_EXPERIENCE`** — Search for threads detailing viewing habits, visceral reactions, and emotional impacts.
  - *Keywords:* "visceral", "emotional drain", "bored", "theater experience", "how do you feel about", "cried", "makes me feel".
- **`THEMATIC_INTERPRETATION`** — Search for threads dedicated to decoding meaning, subtext, and cultural frameworks.
  - *Keywords:* "symbolism", "metaphor", "ending explained", "representation", "allegory", "meaning of".
- **`CRAFT_EVALUATION`** — Search for threads tracking technical filmmaking execution, industry realities, and career runs.
  - *Keywords:* "cinematography", "pacing", "script treatment", "filmography run", "box office", "editing".

### 2. Target Distribution per Label

To prevent the LoRA model from developing an unhelpful classification bias, the manually built dataset will follow a balanced, roughly equal-third distribution split across 200 total items:

| Label | Target Count | Data Type Mix |
| --- | --- | --- |
| `CRAFT_EVALUATION` | 70 items | ~55 comments / 15 original posts |
| `THEMATIC_INTERPRETATION` | 65 items | ~55 comments / 10 original posts |
| `AUDIENCE_EXPERIENCE` | 65 items | ~50 comments / 15 original posts |
| **TOTAL** | **200 items** | **160 Comments / 40 Posts (80/20 Ratio)** |

### 3. Alternative Source Mitigation Strategy (For Underrepresentation)

Because r/TrueFilm naturally skews heavily toward academic text and industry craft analysis, the `AUDIENCE_EXPERIENCE` category (personal emotional diary entries) might prove slower to harvest manually. If that label remains underrepresented after a **1.5-hour** manual collection window on r/TrueFilm, the search will be expanded to alternative public, unauthenticated film platforms:

- **r/Letterboxd:** Comments or review text will be sourced here, as the user base leans heavily into personal diary logs, visual vibes, and subjective text.
- **r/movies:** High-engagement public discussion threads for emotional or high-impact movies (e.g., *Aftersun*, *Interstellar*, *Everything Everywhere All At Once*) will be targeted to scrape pure personal reactions.

> **Strict Length Guardrail:** Every collected item must fall within a defined length window of **4 sentences (minimum) to 300 words (maximum)**. If data is pulled from these alternative sources, a strict manual filter will be applied: only comments containing **4 or more sentences** will be collected. This ensures the text maintains the articulate, dense prose profile required to match the r/TrueFilm baseline data. The **300-word maximum** keeps each item under DistilBERT's 512-token input limit (~300 words ≈ ~450 tokens), so no post is silently truncated during training or inference.

## Evaluation Metrics & Validation Strategy
Evaluating a text classification model on a compact, highly balanced dataset (200 rows) requires looking far beyond simple Overall Accuracy. Because r/TrueFilm language is notoriously dense and naturally blends multiple styles, a baseline overall accuracy of 33% on our 3-class task is trivial—a random guesser matches that.

To ensure the fine-tuned model meaningfully exceeds the baseline and learns the precise boundaries between our categories, the evaluation pipeline will track and generate a comprehensive suite of per-class and structural metrics.

**Two models are evaluated on the identical held-out test set** and reported side by side:

1. **Fine-tuned DistilBERT** — trained on our 200 labeled examples.
2. **Groq `llama-3.3-70b` (zero-shot baseline)** — given only the label definitions in its prompt, with no training.

For both models we report overall accuracy and per-class F1, so the results directly answer the project's core question: *does fine-tuning a small model on our own data beat simply prompting a large general-purpose one?*

### 1. Core Metrics Matrix

| Metric | How It Is Measured | Why It Is Crucial For This Task |
| --- | --- | --- |
| **Precision** (Per Class) | Of all examples the model predicted as this label, what fraction actually were? | Measures the model's exactness. High precision prevents false positives, ensuring that technical jargon or film reviews do not accidentally pollute the `THEMATIC_INTERPRETATION` class. |
| **Recall** (Per Class) | Of all examples that truly are this label, what fraction did the model catch? | Measures the model's completeness. High recall ensures the model catches subtle, articulate personal entries in the `AUDIENCE_EXPERIENCE` class without missing them due to their academic density. |
| **F1-Score** (Per Class) | The harmonic mean of precision and recall. | The most useful single number per class. Balanced per-class F1-scores (≥0.70) prove the model is learning all distinctions well, while an F1-score approaching 0 flags an unlearned boundary or inconsistent labeling. |
| **Overall Accuracy** | Total fraction of test examples the model got right. | Provides a high-level sanity check against the 33% random baseline. |

### 2. Deep-Dive Diagnostics: The Confusion Matrix

We will generate a full Confusion Matrix where rows represent the true human labels and columns represent the predicted model labels. The diagonal values will track correct predictions, while off-diagonal cells will expose the exact direction of the model's errors.

This matrix is vital for isolating specific, actionable patterns in our r/TrueFilm data. For example, a high error volume at position `[row=THEMATIC_INTERPRETATION, col=CRAFT_EVALUATION]` will immediately tell us if the model is over-indexing on technical words and failing to process the "Tool-to-Meaning" hybrid rule.

### 3. Post-Training Error Analysis Protocol

To validate the model's true comprehension vs. shallow pattern matching, the final evaluation report will feature a qualitative audit containing:

- **3 Specific Failure Case Studies:** An in-depth analysis of at least three specific wrong predictions. Each analysis will be explicitly tied to our data constraints, the specific label boundary crossed, or the model's token behavior (rather than a generic "it got it wrong" statement).
- **A Failure Pattern Reflection:** A deep diagnostic reflection evaluating the gap between what the model captured and what was intended. This will explicitly target a specific systemic failure pattern (such as a recurring label pair confusion, a specific post length issue, or a distributional blind spot) to guide the next phase of training refinement.

### 4. The Validation Split Protocol

Because the entire dataset is exactly 200 items, we cannot afford to lose massive amounts of data to a static train/test split. We will deploy an **85/15 Stratified Split**:

- **170 Rows:** Training data.
- **30 Rows:** Validation data (exactly 10 rows from each of the 3 classes).

Using a stratified split guarantees that the validation pool contains a perfectly equal distribution of all three labels, preventing our metric calculations from becoming statistically distorted.

## Definition of Success

Success is defined relative to the **Groq `llama-3.3-70b` zero-shot baseline**, not an absolute production threshold. The core question is whether fine-tuning a small model on our own data beats simply prompting a large general-purpose model. We consider the project successful if, on the held-out test set, the fine-tuned DistilBERT meets all three of the following:

- **Beats the baseline on macro-F1.** The fine-tuned model's macro-averaged F1 must exceed the Groq baseline's macro-F1. This is the headline result.
- **Per-class F1 ≥ 0.70 for every label.** No single class may collapse. A class scoring near 0 signals an unlearned boundary or inconsistent labeling rather than a usable model.
- **A diagnosable error pattern, not random failure.** The confusion matrix should show errors concentrated in the predicted hard case (`CRAFT_EVALUATION` ↔ `THEMATIC_INTERPRETATION` on "Tool-to-Meaning" hybrids), confirming the model learned real distinctions and failed only where we expected it to.

> **Note on scale:** With only 30 test items (10 per class), a single misclassification moves a per-class F1 by ~10 points. These thresholds are therefore treated as indicative targets, and the qualitative error analysis (§3 above) carries as much weight as the raw numbers.