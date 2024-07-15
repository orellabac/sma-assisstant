# SMA Assistant

![Assistant](https://raw.githubusercontent.com/orellabac/sma-assistant/main/assistant.jpeg)

The [Snowpark Migration Accelerator (SMA)](https://www.snowflake.com/snowflake-professional-services/snowpark-migration-accelerator/) is designed to report "issues" to you regarding the code that it migrates.

At it's core, it is both an accelerator AND a troubleshooter.

The SMA is designed to convert what it can, but also to tell you everything that it can about what it cannot convert.

This extension tried to make it easy to link this issues with the SMA knowlegde base and whenever possible automate some quick fixes.

# Leveraging Cortex

The SMA Assistant can also leverage [CORTEX](https://docs.snowflake.com/en/user-guide/snowflake-cortex/overview) to start a discussion thread that can help you to understand the code or rewrite it.

SMA Assistant will leverage the existing connections in `~/.snowflake/config.toml`.

You can for example use the SnowCLI to setup a connection. The tool will be able to read the connections from the file and use them to start a discussion thread.

## Custom Prompts

The tool will show some prompts to the user to help him to understand the code and to provide some context to the discussion thread. Whenever it finds an [SMA Issue](https://docs.snowconvert.com/sma/issue-analysis/issue-codes-by-source) it will offer an option ![context_menu](cortex_menu_options.png)  to start a discussion thread which can help to find a solution. 

By default the tool will first to try gather information from the SMA documentation. If it cannot find any information it search for a "matching" prompt in the `sma-assistant-prompts.yaml` file in the current workspace folder.

Entries in the `sma-assistant-prompts.yaml` file are defined as follows:

```yaml
  - name: "RDD.flatMap"
    prompt: |
      Explain this tag: 
      ------ TAG
      @@firstline 
      and how it affects the following line of code: 
      ------ 
       **CODE** 
      @@secondline
      ------
      Consider these notes:
      ------ 
      **NOTES**
      Spark has a concept of RDD which is not present 
      In snowpark you need to use dataframe operations.
      ...
      ------
      And based on that information provided some rewrite suggestions. 
    matcher: ".*pyspark.rdd.RDD.flatMap.*"
```

The `matcher` is a regular expression that will be used to match the SMA issue. If the issue matches the regular expression the prompt will be displayed.

The `prompt` is a template that will be used to generate the prompt. The template can contain some placeholders that will be replaced with the actual values. The placeholders are defined as follows:

- `@@firstline`: The first line of the code that is associated with the SMA issue.
- `@@secondline`: The second line of the code that is associated with the SMA issue.


# Using the SMA Assistant

The first step is to first, download the [SMA](https://www.snowflake.com/snowflake-professional-services/snowpark-migration-accelerator/) and [convert some code](https://docs.snowconvert.com/sma/user-guide/conversion).

The SMA is an accelerator. That means its purpose is to make your journey to snowpark easier but there will be still some tasks to perform.

When you open your migrated code in VS Code with the SMA assistant, it will populate the problems views with identified tags inserted by SMA.

Your code is considered ready for testing when you have removed all the SMA issues.

![](https://raw.githubusercontent.com/orellabac/sma-assistant/main/problems_view.png)

The SMA also provides some quick fixes that can be applied to your code. And if you select the learn me option it will take to the SMA documentation which will provide some guidance on how to add the missing functionality.

## Credits

polar bear icon by Icons Producer from [Noun Project](`https://thenounproject.com/browse/icons/term/polar-bear/`)  (CC BY 3.0)
