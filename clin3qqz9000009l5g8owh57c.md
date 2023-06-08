---
title: "Code vs. Cells"
seoTitle: "Code vs. Cells"
seoDescription: "Why I've Ditched Excel and Google Sheets and instead built small domain models."
datePublished: Thu Jun 08 2023 12:15:36 GMT+0000 (Coordinated Universal Time)
cuid: clin3qqz9000009l5g8owh57c
slug: code-vs-cells
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686226283587/5a72b35b-8b0f-43a3-a2ca-e6c1bd62468e.jpeg
tags: python, ddd, domain-driven-design, excel, scripting

---

## Motivation

I like [Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design), and seeing how to utilize it. A fun exercise is trying to model everyday problems while building them test driven. I've started doing that with simple problems I would previously have solved with a spreadsheet (i.e. [Google Sheets](https://www.google.com/sheets/about/)). It's great for learning, and I've realized I get a better result.

## Calculating velocity

I have on multiple occasions acted as [Scrum Master](https://www.scrum.org/resources/what-is-a-scrum-master) (I'd argue I haven't run scrum, nor [that I want to](https://www.youtube.com/watch?v=hxXmTnb3mFU&t=448s)), and the question of velocity comes up. I've previously created excel sheets where each sheet was a sprint, and then I had a "front page" with the summarized calculations. It was a mess when I suddenly wanted to introduce a new datapoint, or reformat a given page - as references to specific cells would suddenly be broken. It was a mess.

Instead, I created Sprinthon. It's a very simple Python tool - Utilizing [Pydantic](https://pydantic.dev/) I was able to fletch out the domain model rather quickly. A sprint simply consists of:

```python
from pydantic import BaseModel, validator

class SprintScope(BaseModel):
    initial: Optional[StoryPoints]
    final: Optional[StoryPoints]
    burned: Optional[StoryPoints]

    @property
    def growth(self) -> Goal:
        if self.final and self.initial:
            return self.final - self.initial

        return None

    @property
    def spillover(self) -> Goal:
        if self.final and self.burned:
            return self.final - self.burned

        return None


class Sprint(BaseModel):
    number: int
    scope: Optional[SprintScope] = None
```

And I suddenly I had a list of sprints:

```python
sprints = [
    Sprint(
        number=2,
        scope=SprintScope(
            initial=50,
            final=55,
            burned=45,
        ),
        workdays=5,
    ),
    Sprint(
        number=1,
        scope=SprintScope(
            initial=60,
            final=61,
            burned=55,
        ),
        workdays=5,
    ),
]
```

And getting any sort of statistics out of this is pretty trivial. The average burned story points are pretty easy to get:

```python
avg_burned = avg(sprint.scope.burned for sprint in sprints if sprint.scope),
```

One could easily make it a [Rolling average](https://en.wikipedia.org/wiki/Moving_average) or something more complex if wanted.

Furthermore, I realized I'd want to add `working_days` to the model after we had a sprint that was partly swallowed by vacations. Easy to add to the model:

```python
class Sprint(BaseModel):
    number: int
    workdays: float | None = None
    scope: Optional[SprintScope] = None

    def get_daily_velocity(self) -> float | None:
        if self.workdays is None or self.scope is None or self.scope.burned is None:
            return None

        return round(self.scope.burned / self.workdays, 2)
```

By allowing optional fields, I can easily enforce various backward and future-proofing, and having an IDE, I can easily update the sprints that don't conform. By writing a few unit tests it is easy to validate that my results are what I expect:

```python
def test_has_daily_velocity(some_sprint: Sprint):
    some_sprint.scope.burned = 10
    some_sprint.workdays = 5

    assert some_sprint.get_daily_velocity() == 2
```

I ended up extending it to create a `MarkdownWriter` which means I can export a report of the sprints, which I can convert to PDF or whatever I'd want. That would have been a lot more work in a spreadsheet.

## Conclusion

I often forget how easy it is to write code. I often forget how much I enjoy writing code. By using scripting languages (I also wrote a similar tool for another purpose in F#), I can easily help automate the boring stuff. It might be what [Automate The Boring Stuff With Python](https://automatetheboringstuff.com/) argues as well. But it's quite neat - and by making micro tools they might grow into an open-source package that can help others.

Worst case, you've solved your problem and improved your coding and problem-solving skills in the process.