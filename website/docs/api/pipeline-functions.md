---
title: Pipeline Functions
teaser: Other built-in pipeline components and helpers
source: spacy/pipeline/functions.py
menu:
  - ['merge_noun_chunks', 'merge_noun_chunks']
  - ['merge_entities', 'merge_entities']
  - ['merge_subtokens', 'merge_subtokens']
---

## merge_noun_chunks {#merge_noun_chunks tag="function"}

Merge noun chunks into a single token. Also available via the string name
`"merge_noun_chunks"`. After initialization, the component is typically added to
the processing pipeline using [`nlp.add_pipe`](/api/language#add_pipe).

> #### Example
>
> ```python
> texts = [t.text for t in nlp(u"I have a blue car")]
> assert texts == ["I", "have", "a", "blue", "car"]
>
> merge_nps = nlp.create_pipe("merge_noun_chunks")
> nlp.add_pipe(merge_nps)
>
> texts = [t.text for t in nlp(u"I have a blue car")]
> assert texts == ["I", "have", "a blue car"]
> ```

<Infobox variant="warning">

Since noun chunks require part-of-speech tags and the dependency parse, make
sure to add this component _after_ the `"tagger"` and `"parser"` components. By
default, `nlp.add_pipe` will add components to the end of the pipeline and after
all other components.

</Infobox>

| Name        | Type  | Description                                                  |
| ----------- | ----- | ------------------------------------------------------------ |
| `doc`       | `Doc` | The `Doc` object to process, e.g. the `Doc` in the pipeline. |
| **RETURNS** | `Doc` | The modified `Doc` with merged noun chunks.                  |

## merge_entities {#merge_entities tag="function"}

Merge named entities into a single token. Also available via the string name
`"merge_entities"`. After initialization, the component is typically added to
the processing pipeline using [`nlp.add_pipe`](/api/language#add_pipe).

> #### Example
>
> ```python
> texts = [t.text for t in nlp(u"I like David Bowie")]
> assert texts == ["I", "like", "David", "Bowie"]
>
> merge_ents = nlp.create_pipe("merge_entities")
> nlp.add_pipe(merge_ents)
>
> texts = [t.text for t in nlp(u"I like David Bowie")]
> assert texts == ["I", "like", "David Bowie"]
> ```

<Infobox variant="warning">

Since named entities are set by the entity recognizer, make sure to add this
component _after_ the `"ner"` component. By default, `nlp.add_pipe` will add
components to the end of the pipeline and after all other components.

</Infobox>

| Name        | Type  | Description                                                  |
| ----------- | ----- | ------------------------------------------------------------ |
| `doc`       | `Doc` | The `Doc` object to process, e.g. the `Doc` in the pipeline. |
| **RETURNS** | `Doc` | The modified `Doc` with merged entities.                     |

## merge_subtokens {#merge_subtokens tag="function" new="2.1"}

Merge subtokens into a single token. Also available via the string name
`"merge_subtokens"`. After initialization, the component is typically added to
the processing pipeline using [`nlp.add_pipe`](/api/language#add_pipe).

As of v2.1, the parser is able to predict "subtokens" that should be merged into
one single token later on. This is especially relevant for languages like
Chinese, Japanese or Korean, where a "word" isn't defined as a
whitespace-delimited sequence of characters. Under the hood, this component uses
the [`Matcher`](/api/matcher) to find sequences of tokens with the dependency
label `"subtok"` and then merges them into a single token.

> #### Example
>
> Note that this example assumes a custom Chinese model that oversegments and
> was trained to predict subtokens.
>
> ```python
> doc = nlp("拜托")
> print([(token.text, token.dep_) for token in doc])
> # [('拜', 'subtok'), ('托', 'subtok')]
>
> merge_subtok = nlp.create_pipe("merge_subtokens")
> nlp.add_pipe(merge_subtok)
>
> doc = nlp("拜托")
> print([token.text for token in doc])
> # ['拜托']
> ```

<Infobox variant="warning">

Since subtokens are set by the parser, make sure to add this component _after_
the `"parser"` component. By default, `nlp.add_pipe` will add components to the
end of the pipeline and after all other components.

</Infobox>

| Name        | Type    | Description                                                  |
| ----------- | ------- | ------------------------------------------------------------ |
| `doc`       | `Doc`   | The `Doc` object to process, e.g. the `Doc` in the pipeline. |
| `label`     | unicode | The subtoken dependency label. Defaults to `"subtok"`.       |
| **RETURNS** | `Doc`   | The modified `Doc` with merged subtokens.                    |
