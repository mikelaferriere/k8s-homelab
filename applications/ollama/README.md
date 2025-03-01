# Increase context length for Ollama models
https://localai.hashnode.dev/how-to-increase-ollama-models-context-length-from-2048

---

# Adjusting Context Length for Deepseek and LLaMA 3 Models in Ollama

The default context length for the Deepseek or LLaMA 3 models in Ollama is set to **2048 tokens**, which may cause context loss. To fully utilize these models' extended context windows, you can adjust the `num_ctx` parameter.

## Method 1: Create a Custom Model with Increased Context Window

### Steps:
1. Export the Current Model File:
   ```bash
   ollama show --modelfile deepseek-r1:14b > Modelfile.deepseek-r1:14b-32k
   ```

2. Edit the Model File:
   - Open `Modelfile` in a text editor.
   - Ensure the line starting with `FROM` reads:  
     ```
     FROM deepseek-r1:14b
     ```
   - Add the following line to increase the context window:
     ```
     PARAMETER num_ctx 32000
     ```

3. Create the New Model:
   ```bash
   ollama create -f Modelfile.deepseek-r1:14b-32k deepseek-r1:14b-32k
   ```

4. Verify the Context Length:
   ```bash
   ollama show deepseek-r1:14b-32k
   ```
   - Ensure `num_ctx` is set to **32000** under the Parameters section.
