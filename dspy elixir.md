
1. define signature -> generate structured outputs from elixir 


# Simple LLM LangChain Elixir Example 

Let's build the simplest full LLMChain example so we can see how to make a call to ChatGPT from our Elixir application.

**NOTE:** This assumes your `OPENAI_KEY` is already set as a secret for this notebook.

```
Application.put_env(:langchain, :openai_key, System.fetch_env!("LB_OPENAI_API_KEY"))
```

```
alias LangChain.Chains.LLMChain
alias LangChain.ChatModels.ChatOpenAI
alias LangChain.Message

{:ok, updated_chain} =
  %{llm: ChatOpenAI.new!(%{model: "gpt-4o"})}
  |> LLMChain.new!()
  |> LLMChain.add_message(Message.new_user!("Testing, testing!"))
  |> LLMChain.run()

updated_chain.last_message.content
```

Nice! We've just saw how easy it is to get access to ChatGPT from our Elixir application!

Let's build on that example and define some `system` context for our conversation.

## [](https://hexdocs.pm/langchain/getting_started.html#adding-a-system-message)Adding a System Message

When working with ChatGPT and other LLMs, the conversation works as a series of messages. The first message is the `system` message. This defines the context for the conversation. Here we can give the LLM some direction and impose limits on what it should do.

Let's create a system message followed by a user message.

```
{:ok, updated_chain} =
  %{llm: ChatOpenAI.new!(%{model: "gpt-4"})}
  |> LLMChain.new!()
  |> LLMChain.add_messages([
    Message.new_system!(
      "You are an unhelpful assistant. Do not directly help or assist the user."
    ),
    Message.new_user!("What's the capital of the United States?")
  ])
  |> LLMChain.run()

updated_chain.last_message.content
```

Here's the answer it gave me when I ran it:

> Why don't you try looking it up online? There's so much information readily available on the internet. You might even learn a few other interesting facts about the country.

What I love about this is we can see the power of the `system` message. It completely changed the way the LLM behaves by default.

Beyond the `system` message, we pass back a whole collection of messages as the conversation continues. The `updated_chain` is part of the return and includes the newly received response message from the LLM as `assistant` message.

## [](https://hexdocs.pm/langchain/getting_started.html#streaming-responses)Streaming Responses

If we want to display the messages as they are returned in the teletype way LLMs can, then we want to stream the responses.

In this example, we'll output the responses as they are streamed back. That happens in a callback function that we provide.

The `stream: true` setting belongs to the `%ChatOpenAI{}` struct that setups up our configuration. We also pass in the `callbacks` with the `llm` to fire the `on_llm_new_delta`. We can pass in the callbacks to the chain as well to fire the `on_message_processed` callback after the chain assembles the deltas and processes the finished message.

```
alias LangChain.MessageDelta

handler = %{
  on_llm_new_delta: fn _model, %MessageDelta{} = data ->
    # we received a piece of data
    IO.write(data.content)
  end,
  on_message_processed: fn _chain, %Message{} = data ->
    # the message was assembled and is processed
    IO.puts("")
    IO.puts("")
    IO.inspect(data.content, label: "COMPLETED MESSAGE")
  end
}

{:ok, updated_chain} =
  %{
    # llm config for streaming and the deltas callback
    llm: ChatOpenAI.new!(%{model: "gpt-4o", stream: true})
  }
  |> LLMChain.new!()
  |> LLMChain.add_messages([
    Message.new_system!("You are a helpful assistant."),
    Message.new_user!("Write a haiku about the capital of the United States")
  ])
  # register the callbacks
  |> LLMChain.add_callback(handler)
  |> LLMChain.run()

updated_chain.last_message.content
# streamed
# ==> Washington D.C. stands,
# ... Monuments reflect history,
# ... Power's heart expands.

# ==> COMPLETED MESSAGE: "Washington D.C. stands,\nMonuments reflect history,\nPower's heart expands."
```

As the delta messages are received, the `on_llm_new_delta` callback function fires and the received data is written out to the console.

# How to use JSON processor to get json outputs from the model. 



For each image we want two pieces of generated content. To make this easy on ourselves, we instruct the LLM to output the two pieces of information in a JSON object.

Specifically, we instruct it to output in the following format:

```
{
  "alt": "generated alt text",
  "caption": "generation caption text"
}
```

To make working with that output easier, we'll use a `JsonProcessor` for processing messages from the LLM. It has the added ability to return a JSON formatting error to the LLM in the case when it gets it wrong.

We'll see this next when we put it all together.

## [](https://hexdocs.pm/langchain/context-specific-image-descriptions.html#making-the-request)Making the Request

Everything is ready to make the request!

- We have the image
- We setup which LLM we are connecting with
- We provide context in our prompt and instructions for the type of description we want

Now, we'll submit the request to the server and review the response. For this example, the "image_data_from_other_system" is a substitute for a database call or other lookup for the information we have on the image.

```
alias LangChain.Chains.LLMChain
alias LangChain.MessageProcessors.JsonProcessor

# This data comes from an external data source per image.
# When we `apply_prompt_templates` below, the data is rendered into the template.
image_data_from_other_system = "image of urban art mural on underpass at 507 King St E"

{:ok, updated_chain} =
  %{llm: openai_chat_model, verbose: true}
  |> LLMChain.new!()
  |> LLMChain.apply_prompt_templates(messages, %{extra_image_info: image_data_from_other_system})
  |> LLMChain.message_processors([JsonProcessor.new!(~r/```json(.*?)```/s)])
  |> LLMChain.run(mode: :until_success)

updated_chain.last_message.processed_content
```