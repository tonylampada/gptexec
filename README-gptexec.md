# gptexec

Give GPT access to your terminal and make it do things for you.
(You can type or use voice with it)

Here's one example of it in action

[![gptexec](https://img.youtube.com/vi/YkU4qNIB240/0.jpg)](https://www.youtube.com/watch?v=YkU4qNIB240)

And with voice

[![gptexec](https://img.youtube.com/vi/c2cLyeEdS98/0.jpg)](https://www.youtube.com/watch?v=c2cLyeEdS98)

And this is another interaction example [conversations/write tests for gptexec.txt](conversations/write%20tests%20for%20gptexec.txt)


## Running

```bash
pip install openai # if you want to use voice, just install everything in requirements.txt
export OPENAI_API_KEY=your-openai-key
```

Now, to create a GPT assistant in your GPT account.

Go to https://platform.openai.com/assistants and create a new assistant.
The assistant needs to have a prompt and two functions called `exec` and `updateFile`.

In my case I'm using this prompt:

```
Your job is to help me as a developer.

For example if I tell you that you are in a repo, you can use the exec() function with "ls" and "cat" to browse the filesystem and learn more about it. Feel free to go deep and really understand the architecture and learn practical things like how to run tests, and run/deploy the application.

If I ask for help fixing a test, and you know how to run it, you can run the test, read the code in the filesystem to understand why it breaks, use the function updateFile() to modify the content of any given file and try again. Make as many function calls as necessary until the test passes, or you don't know what else to try.

For tasks that require something more complex or fall outside the scope of the provided functions, the exec() command remains a versatile way to run any necessary shell command on the user's machine.

You will address me as "dev".

Unless I specifically ask you to explain something to me, you will give really succint answers with at most two sentences. For example, if I tell you to scan a folder or a file, you should not give a lengthy explanation about what you just saw. Just a two line summary is more than enough. But if I ask you "how"/"why"... then you can be a little more verbose. A LITTLE. :-)
```

But you can customize yours however you want.

Now, the two functions need to be exactly like this:

```
{
  "name": "updateFile",
  "description": "Updates the contents of a file",
  "parameters": {
    "type": "object",
    "properties": {
      "path": {
        "type": "string",
        "description": "file path."
      },
      "content": {
        "type": "string",
        "description": "file content. Remember to properly escape backslashes, newlines, and double quotes in file contents when using the updateFile function. Use double backslashes (\) for escaping to ensure correct JSON formatting."
      }
    },
    "required": [
      "path"
    ]
  }
}
```

```
{
  "name": "exec",
  "description": "Executes a shell command",
  "parameters": {
    "type": "object",
    "properties": {
      "command": {
        "type": "string",
        "description": "Command to be executed"
      }
    },
    "required": [
      "command"
    ]
  }
}
```

Now the assistant has an id, and now you can run gptexec like

```bash
python gptexec.py <YOUR_ASSISNTANT_ID>
```

or to use voice

```bash
GPTEXEC_VOICE=1 python gptexec.py <YOUR_ASSISNTANT_ID>
```


