# beautiful-gpt

To install dependencies:

```bash
bun install
```

To run:

```bash
bun run index.ts
```

This project was created using `bun init` in bun v0.5.9. [Bun](https://bun.sh) is a fast all-in-one JavaScript runtime.

## Initial Notes:

# Idea

A typesafe way to interact with with OpenAIs Chat/Completion APIs using TypeScript,
This is a tool for builders to build typsafe queries to open ai APIs (and idea can be carried over to other models of course)

this allows you to craft the perfect query for your programming project in a consumable way, these queries can be expensive to run so make sure you utilize
caching where you can.

This enables you to quickly prototype ideas and build real apps quickly, by answering questions in a data format that you can use.

```
const beGpt = createBeautifulGpt({
openaiToken: '',
dataResponseFmt: 'json', // json will be default
})
```

this would automatically request the response as json
You could be like openAiLib.requestAs(json)

```
openAiLib.createJsonFields(
[
{“foodName”: ‘string’},
{ “calories”: ‘number’},
{“fats_grams”: 'number'},
{“proteins_grams”: 'number'},
]
)
```

come up with config that works best with type inferencing // probably use sod?
Fields that you expect in the response which will then give you a typed response when you make the call

the prompt you want to amend
`openAiLib.createPrompt(“give me 10 foods that are roughly 200 calories each”)`



Result in the above case would be typed according to the json configs set, it will throw and error if the call either fails or the return doesn't match the data response, which then needs to be handles

if validation fails, it should try to make the call again, set a max limit on retries, before the data is validated successfully

next you can grab the full prompt that was generated by this API, that you can then use to make your API call, that way this wont limit you to what service you are using
`const prompt = openAiLib.getRawPrompt()`


This is just an example of how you might  pass prompt to a function that calls the openai api
`const apiResult = await yourImplementationToMakeAPiCall(getRawPrompt)`


validate data against the validation schema we created earlier
`const validatedData = openAiLive.validateApiData(apiResult.data)`

it will error if there's an difference between what is expected, if it errors you can setup a retry, but adjusting your prompt might work better

#### OLD IDEA
make call would make create the prompt that would be combined with the user prompt, to get it to return in a way that makes
the api return data in a JSON format
NOT GOING TO DO THIS INTIALLY BECUASE YOU CAN JUST GET THE RAW PROMPT/might be better to pass back in such a way that you can 
provide the fetcher (similar to how react-query works)
`const result = await openAiLib.makeCall()`

## Plans

Initially build on top of OpenAI package, but create interface layers so we can swap it out with an http layer direct to open ai

##Notes:
using zod we can validate if the response we got matches the zod assertion

Draft api up which we will give to chat gpt to create the initial outline of the api

Keep release process as simple as possible, just do everything manually for now, create the mono repo, create the package, and create a test app to try it with (remix app)