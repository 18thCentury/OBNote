```
llama-server `
--model gemma-4-E2B-it-Q4_K_M.gguf `
--mmproj mmproj-gemma-4-E2B-it-BF16.gguf `
--host 0.0.0.0 `
--port 8080 `
--threads 6 `
--flash-attn 1 `
--no-mmap `
--direct-io `
--ctx-size 65536 `
--batch-size 512 `
--ubatch-size 256 `
--jinja `
--n-gpu-layers 7 `
--n-predict -1 `
--keep -1 `
--temp 1 `
--top-k 64 `
--top-p 0.95 --chat-template chatml -r '<|im_end|>'
```

```
llama-server --model gemma-4-E2B-it-Q4_K_M.gguf --mmproj mmproj-gemma-4-E2B-it-BF16.gguf --host 0.0.0.0 --port 8080 --threads 6 --ctx-size 65536 --jinja --n-gpu-layers 7 --temp 1 --top-k 64 --top-p 0.95 --chat-template chatml -r '<|im_end|>'
```


```
llama-server --model gemma-4-E2B-it-UD-Q5_K_XL.gguf --mmproj mmproj-gemma-4-E2B-it-BF16.gguf --host 0.0.0.0 --port 8080 --threads 6 --ctx-size 65536 --jinja --n-gpu-layers 7 --temp 1 --top-k 64 --top-p 0.95 --chat-template chatml -r '<|im_end|>'
```