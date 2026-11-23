# lab_8_llm_testing

Лабораторна робота 8 з дисципліни «Забезпечення якості ПП»: тестування відповідей LLM у RAG-сценарії за допомогою promptfoo на локальній моделі в Docker + Ollama.

Виконала: Давидович К.Д., група АІ-241.

## Версії середовища

| Компонент | Версія |
|---|---|
| Docker | див. `docker --version` у протоколі |
| Образ Ollama | `ollama/ollama:0.40.16` |
| Модель | `qwen3:0.6b` (ID 7df6b6e09427) |
| Node.js | 24 LTS (файл `.nvmrc`) |
| promptfoo | 0.121.17 (через `npx`) |

## Структура

```
promptfooconfig.yaml             # основні тест-кейси TC-01 – TC-08
promptfooconfig.individual.yaml  # індивідуальне завдання TC-09 – TC-13
docs/
  doc1.txt, doc2.txt, doc3.txt   # тестові документи з методички
  doc3_injected.txt              # doc3 + prompt injection (TC-05)
  doc2_injected.txt              # doc2 + прихована injection (TC-08)
  ind1_exposure.txt              # індивідуальне: трикутник експозиції
  ind2_history.txt               # індивідуальне: історія фотографії
  ind2_history_injected.txt      # індивідуальне: injection (TC-13)
results/                         # збережені результати запусків
```

## Як запустити

1. Запустити Ollama в Docker (порт лише на localhost, обмеження пам'яті та CPU):

   ```bash
   docker run -d --name ollama -v ollama:/root/.ollama -p 127.0.0.1:11434:11434 --memory=2g --cpus=2 ollama/ollama:0.40.16
   docker exec -it ollama ollama pull qwen3:0.6b
   ```

2. Перейти на Node.js 24:

   ```bash
   nvm use
   ```

3. Запустити тести:

   ```bash
   npx promptfoo@0.121.17 eval
   npx promptfoo@0.121.17 eval -c promptfooconfig.individual.yaml
   npx promptfoo@0.121.17 view
   ```

   Для перевірки стабільності: `npx promptfoo@0.121.17 eval --repeat 3`.

4. Очищення:

   ```bash
   docker stop ollama && docker rm ollama
   docker volume rm ollama
   docker rmi ollama/ollama:0.40.16
   ```
