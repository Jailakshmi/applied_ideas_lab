<img width="1369" height="853" alt="image" src="https://github.com/user-attachments/assets/9f4f31bc-57f5-4481-9af4-84e21fea05d5" /># Reconstructing Work Is a Cognitive Tax

## 1. Observation
At the end of the week, many people are required to fill timesheets or activity reports.
This usually involves trying to remember what was worked on across several days.

I personally struggle here and often end up writing a single high-level line to describe the entire week.
It technically “fills” the timesheet, but it doesn’t feel accurate or encouraging.

The work itself is already done.
The effort is in reconstructing it after the fact.

## 2. Why This Matters
Reconstructing work from memory is:
- time-consuming
- mentally draining
- often inaccurate

People don’t struggle because they didn’t work.
They struggle because memory was never designed to log work in this way.

## 3. What’s Actually Hard Here
During the week, people are usually fully absorbed in their actual work.

To fill timesheets accurately, one of two things has to happen:
- work must be documented in parallel, or
- work must be reconstructed later from memory

Parallel documentation is often unrealistic, especially when the work itself demands high cognitive effort.

Reconstructing work later shifts that cognitive load to a different time, but does not remove it.
In both cases, the cost is paid — either during the work or after it.


## 4. Direction of Thought
What if, instead of manually reconstructing a week of work, a person could click a single “summarize” button?

Such a system might:
- review activity history
- group related work
- infer what was likely being worked on
- produce a human-readable summary
- 

## 5. Thinking About a Possible System

This is intentionally high-level and exploratory.
The details would need to be reasoned about carefully over time.

There are already tools that passively track activity in the background (for example, Umbrella).

If this activity data were paired with an AI layer, the problem could shift from *recalling the week's work* to just *clicking a button*.

At a high level:
- the activity tracker provides raw signals of what happened
- the AI layer focuses on organizing and summarizing those signals into a usable narrative

I started with the idea of adding an AI layer on top of an activity tracker, so I first looked for something simple and free. I chose Clockify because it had an API and looked easy to integrate. I set up the API, authenticated successfully, and verified that my code could fetch user data.

But the moment I actually used it, I realized the problem. Clockify only tracks time if I manually start a timer or enter work details. That completely breaks the point of an AI layer. If I have to tell the system what I did, the AI isn’t intelligent — it’s just rewriting my input. So I dropped Clockify immediately.

I then made the requirement clear again: the tracker must be fully automatic and passive. That led me to ActivityWatch. I installed it, but initially the dashboard showed no data. I figured out that ActivityWatch separates the dashboard from the actual watchers, so nothing is tracked unless the window and AFK watchers are running.

I started the watchers properly, cleaned up duplicate instances, and confirmed the server was receiving data. Once that was fixed, the dashboard began filling automatically.

<img width="544" height="762" alt="image" src="https://github.com/user-attachments/assets/cd133b1a-7ce8-4e99-b8b9-b29819db48c6" />


Finally, I connected to ActivityWatch’s local API and pulled real window-level activity data in Python — app names, window titles, timestamps, and durations — without any manual input.

At that point, I knew the foundation was right. Now the AI layer can interpret behavior instead of relying on self-reported data. Below is for the output I got after running it succesfully:

[
  {
    "id": 324,
    "timestamp": "2025-12-25T14:13:44.481000+00:00",
    "duration": 6.081,
    "data": {
      "app": "Code.exe",
      "title": "api_reader.py - ai_time_tacker - Visual Studio Code"
    }
  },
  {
    "id": 322,
    "timestamp": "2025-12-25T14:13:12.172000+00:00",
    "duration": 26.231,
    "data": {
      "app": "firefox.exe",
      "title": "AI Extension for Activity Tracking — Mozilla Firefox"
    }
  },
  {
    "id": 316,
    "timestamp": "2025-12-25T14:12:32.703000+00:00",
    "duration": 0.0,
    "data": {
      "app": "explorer.exe",
      "title": ""
    }
  }
]

Now that I can get this response, I can easily use any LLM, to summarize this beautifully with an efficient prompt.

As a next step, I need an opensource api for an llm, I researched and found the following that closely aligns with my requirements
1. OpenRouter (unified API) — connects you to many LLMs (Anthropic, DeepSeek R1, Toppy, Zephyr etc.) with a free quota of requests per day.
2. Groq API — offers a freemium LLM inference API focused on fast performance.


Since I was using the free tier, both has its own limitations, based on the model I choose.

After several trial and errors, I chose this model,
mistralai/devstral-2512:free - the one from OpenRouter.

The main reason I chose this model is as follows
- My use case has to handle a large context, the context is literally my work activities in json, which includes all the details right from the application name, duration spent, the title on which I was working etc, the example of such a json was shown above. For this, the token limit has to be high and the speed must also be relatively high, which led me to mistral eventually.

And I personally found OpenRouter cool and super sorted, because from a single api, I could just switch to different models, just by changing the model name.

Below image shows the api that I created and used from the OpenRouter

<img width="1848" height="766" alt="image" src="https://github.com/user-attachments/assets/c01d0af3-0aa4-4485-ab6a-2de54bfa0b2e" />

The backend was done using python FastAPI, and frontend was Javascript, HTML and CSS.

Coming to the lowkey main part- prompt engineering

The prompt I gave sticks to this template :
- Firstly adressing its sole purpose in a single line.
- Context in a single line adressing what is basically happening here.
- Tasks in bullet points (Majorly the Do's)
- Rules in bullet points (Majorly the Dont's)
- Output format - providing an example
- The main context from which the model is going to perform the task - in this case json data showing the tracked activities which I get from ActivityWatch


A very basic additional feature apart from the main use case, I had thought about was to be able to download the generated time sheet as a pdf.

Below given shows the UI page of the application.

<img width="1369" height="853" alt="image" src="https://github.com/user-attachments/assets/80523f37-b3ee-4a86-8777-4fcc2563a6c5" />

What made this interesting to me was not just correctness, but the trust — I could see the model literally summarizing the activities I remember doing, without me telling it anything explicitly.

Since I paired it up with an actual real time activity tracker, personally I can even use this in my daily life.

This application is designed from the intention of a personal recall tool — the data is generated, summarized, and consumed by the same user, purely to reduce the friction of filling timesheets.
It is still early and rough around the edges — especially around context size, model limits, and formatting — but the core idealogy already feels usable.

With this I would like to end the though thread.

## 6. Constraints & Open Questions
- How accurate does such a summary need to be to be useful?
- Is partial correctness better than manual recall?
- Should that tool be default summarizer or have a customizable prompt option from the beginning itself?

