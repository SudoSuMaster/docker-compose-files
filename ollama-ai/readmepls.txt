includes ollama webui and server


1. bind mount,
change path to your own
[your own path]:/app/backend/data 
exapmle F:\Docker\ollama\webui:/app/backend/data 
exapmle F:\Docker\ollama\ollama:/root/.ollama

2. to install models: docker exec [Your ollama server id] [Your model]
example: docker exec d9ded6e6523a ollama run llama2-uncensored
