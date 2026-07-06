# # Self-Forming Companion (Gemini + Astrocyte-Consolidating Memory)

A minimal reference implementation connecting the Self-Forming Agents
memory architecture (individuation through lived experience, with a
biologically-grounded two-stage consolidation mechanism for its long-term
substrate) to a real conversational LLM (Google Gemini).

Underlying research:
Igarashi, K. (2026). Self-Forming Agents: Individuation through Lived
Experience. https://doi.org/10.5281/zenodo.21122080

The consolidation mechanism (`self_forming_kernel/kernel.py`) implements:
- A fast, continuously-updating substrate (R) -- loosely, neuron-like.
- A slow substrate (C) where each distinct topic/direction gets its own,
  independent "eligibility" trace -- loosely, astrocyte-receptor-like
  (Dewa et al., 2025) -- and only consolidates into long-term memory when
  BOTH primed (recurring) AND tagged with sufficient valence, gated by a
  smooth, ion-channel-style (Boltzmann/sigmoid) function rather than a
  hard on/off threshold.

This exploratory consolidation mechanism is not yet part of the
peer-reviewed paper -- see the paper's accompanying Zenodo code record
for the standalone validation experiments.

## Setup

1. **Get a free Gemini API key**: https://aistudio.google.com/apikey
   (Google accounts get a free tier; no credit card required to start.)

2. **Install dependencies**:
   ```
   pip install -r requirements.txt
   ```

3. **Set your API key** as an environment variable (keeps it out of any
   code you might share):
   ```
   export GEMINI_API_KEY="your-key-here"      # Mac/Linux
   setx GEMINI_API_KEY "your-key-here"        # Windows (restart terminal after)
   ```
   Or, in a notebook (e.g. Google Colab):
   ```python
   import os
   os.environ["GEMINI_API_KEY"] = "your-key-here"
   ```

4. **Run**:
   ```
   python companion.py
   ```

   Type messages to chat. Use `/resonance <text>` to check how strongly
   the memory currently recognizes a topic, without adding to memory.
   `/quit` to exit.

## What's real, what's a placeholder

- **Real**: the memory mechanism itself (`self_forming_kernel/kernel.py`)
  -- every number it produces is a genuine computation, not scripted.
- **Real**: the Gemini embedding and chat calls -- this actually talks to
  Gemini's models.
- **Placeholder / simple by design**:
  - `estimate_valence()` in `companion.py` is a crude heuristic (counts
    exclamation marks and emotional keywords). Replace it with something
    better if you have ideas -- e.g. asking Gemini to self-rate
    significance, or a proper sentiment/arousal model.
  - The embedding-to-kernel-dimension projection is a fixed random
    projection, not a learned one.
  - No persistence across runs by default -- `kernel.R` and `kernel.C`
    are plain NumPy arrays; add `np.save` / `np.load` calls if you want
    memory to survive between sessions.

## Files

- `companion.py` -- the chat loop; connects the kernel to Gemini.
- `self_forming_kernel/kernel.py` -- the memory mechanism itself.
- `requirements.txt` -- dependencies.

## License

MIT. Use, modify, and extend freely.