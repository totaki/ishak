## Пример работы через AWS CLI (s3 sync)

Пример синхронизации задач через AWS CLI.

### Переменные окружения (пример)
```
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_DEFAULT_REGION=ru-msk
export ISHAK_BUCKET=s3-issue
export ISHAK_PROJECT=ishak
```

### Скачивание проекта в .ishak
```
mkdir -p .ishak
aws s3 sync \
  s3://$ISHAK_BUCKET/projects/$ISHAK_PROJECT/ \
  .ishak/ \
  --endpoint-url https://hb.vkcloud-storage.ru \
  --region ru-msk
```

### Загрузка изменений обратно
```
aws s3 sync \
  .ishak/ \
  s3://$ISHAK_BUCKET/projects/$ISHAK_PROJECT/ \
  --endpoint-url https://hb.vkcloud-storage.ru \
  --region ru-msk
```

### Полезные alias
```
alias ishak-sync-down='aws s3 sync s3://$ISHAK_BUCKET/projects/$ISHAK_PROJECT/ .ishak/ --endpoint-url https://hb.vkcloud-storage.ru --region ru-msk'
alias ishak-sync-up='aws s3 sync .ishak/ s3://$ISHAK_BUCKET/projects/$ISHAK_PROJECT/ --endpoint-url https://hb.vkcloud-storage.ru --region ru-msk'
```

### Структура локальной папки
```
.ishak/
  README.md
  META.md
  issues/
    <issue_id>/
      README.md
      comment_<timestamp>_<role>.md
      files/
```

### Формат timestamp в комментариях
`<timestamp>` — это Unix time в миллисекундах (UTC), например:
`comment_1738865445123_agent.md`.
