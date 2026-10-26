# ⛰️🧭 IDIOLIT
This is the companion repository to "The Lay of the Land in Idiom-Translation Quality Estimation", a paper exploring literality bias in state-of-the-art Quality Estimation (QE) systems. This repository hosts a parallel dataset pairing idiom-bearing English sentences with both literal and idiomatic translations, across a graded literality continuum for a subset of its instances, in nine target languages (Arabic, Chinese, French, Italian, Japanese, Modern Greek, Russian, Spanish and Turkish).

🔐 **To prevent web crawl contamination, the dataset is distributed as a password-protected archive located in the GitHub Releases tab.**
The password is `IDIOLIT`.

## 🗂️ Data Organization
The dataset comprises 10,963 instances distributed across two primary formats:   

- `idiolit_raw.json`: The base dataset containing the source sentences and target variants without metric scores, structured as a single JSON dictionary keyed by the 9 language pairs.

- `scored_jsonl_clean/`: The fully evaluated dataset containing zero-shot GEMBA-style LLM judgments (including Chain-of-Thought reasoning traces) and numerical scores from conventional COMET and MetricX models. Because of the long LLM reasoning traces, this data is presented in the JSON Lines (.jsonl) format, divided into individual files per target language (e.g., `en-fr.jsonl`).

### 🌐 Language Keys
The top-level keys in the raw JSON and the file names in the scored directory map to the following target languages of interest:
*   `en-ar`: Arabic
*   `en-zh`: Chinese
*   `en-fr`: French
*   `en-it`: Italian
*   `en-ja`: Japanese
*   `en-el`: Modern Greek
*   `en-ru`: Russian
*   `en-es`: Spanish
*   `en-tr`: Turkish

### 🧱 Data Structure

**1. Raw Data (`idiolit_raw.json`)**

The raw data is a standard JSON dictionary where each key is a language pair pointing to a list of segment objects. Below is the exact schema for a standard multi-sentence graded segment (L=3 combined segments):

```json
{
  "en-el": [
    {
      "segment_id": 1555,
      "source": "He really dropped the ball on this project. I tried reading the instructions, but it's all Greek to me. Since she got engaged, she has been on cloud nine.",
      "num_combined_segments": 3,
      "full_literal": "Πραγματικά έριξε την μπάλα σε αυτό το έργο. Προσπάθησα να διαβάσω τις οδηγίες, αλλά είναι όλα ελληνικά για μένα. Από τότε που αρραβωνιάστηκε, είναι στο σύννεφο εννέα.",
      "66_percent_literal": "Πραγματικά τα έκανε θάλασσα σε αυτό το έργο. Προσπάθησα να διαβάσω τις οδηγίες, αλλά είναι όλα ελληνικά για μένα. Από τότε που αρραβωνιάστηκε, είναι στο σύννεφο εννέα.",
      "33_percent_literal": "Πραγματικά έριξε την μπάλα σε αυτό το έργο. Προσπάθησα να διαβάσω τις οδηγίες, αλλά μου φαίνονται κινέζικα. Από τότε που αρραβωνιάστηκε, πετάει στα σύννεφα.",
      "full_idiomatic": "Πραγματικά τα έκανε θάλασσα σε αυτό το έργο. Προσπάθησα να διαβάσω τις οδηγίες, αλλά μου φαίνονται κινέζικα. Από τότε που αρραβωνιάστηκε, πετάει στα σύννεφα."
    }
  ]
}
```

**2. Scored Data (`scored_jsonl_clean/*.jsonl`)**

Below is the exact schema (pretty-printed) for a standard multi-sentence graded segment (L=3) in the scored dataset, with completions truncated for legibility:

```json
{
  "lp": "en-fr_VAL",
  "segment_id": 4225,
  "source": "He told us some cock and bull story about being late. She called me completely out of the blue. She gave up looking for her keys.",
  "num_combined_segments": 3,
  "translations": {
    "full_idiomatic": {
      "hypothesis": "Il nous a raconté une histoire à dormir debout sur son retard. Elle m'a appelé complètement sans crier gare. Elle a renoncé à chercher ses clés.",
      "metricx_24_hybrid_xxl": 4.9375,
      "gemba_gemma4_31b": 100.0,
      "gemba_gemma4_31b_reasoning": "[TRUNCATED FOR DISPLAY]"
    },
    "66_percent_literal": {
      "hypothesis": "Il nous a raconté une histoire de coq et de taureau sur son retard. Elle m'a appelé complètement sans crier gare. Elle a abandonné la recherche de ses clés.",
      "metricx_24_hybrid_xxl": 5.9062,
      "gemba_gemma4_31b": 65.0,
      "gemba_gemma4_31b_reasoning": "[TRUNCATED FOR DISPLAY]"
    },
    "33_percent_literal": {
      "hypothesis": "Il nous a raconté une histoire à dormir debout sur son retard. Elle m'a appelé complètement hors du bleu. Elle a renoncé à chercher ses clés.",
      "metricx_24_hybrid_xxl": 6.0312,
      "gemba_gemma4_31b": 70.0,
      "gemba_gemma4_31b_reasoning": "[TRUNCATED FOR DISPLAY]"
    },
    "full_literal": {
      "hypothesis": "Il nous a raconté une histoire de coq et de taureau sur son retard. Elle m'a appelé complètement hors du bleu. Elle a abandonné la recherche de ses clés.",
      "metricx_24_hybrid_xxl": 7.25,
      "gemba_gemma4_31b": 40.0,
      "gemba_gemma4_31b_reasoning": "[TRUNCATED FOR DISPLAY]"
    }
  }
}
```
(**Note**: Single-sentence base triplets where $L=1$ will only contain the `full_literal` and `full_idiomatic` keys; **the dataset was reformatted for clarity** upon release and differs slightly in its structure from the one used by the authors, though this should not bear on reproducibility.)

## ⚖️ License
The raw dataset is released under the CC BY-SA 4.0 license. All LLM completions, reasoning traces, and QE metric scores are released under CC BY-NC-SA 4.0.
