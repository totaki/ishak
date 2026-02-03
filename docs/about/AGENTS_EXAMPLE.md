## Пример работы через AWS CLI (s3 sync)

Цель: хранить локальную копию проекта в папке `.ishak/`, синхронизируя всё из
`projects/ishak/`, кроме `archive/`. Внутри `.ishak/` не используем подкаталог
с project_id, так как работаем с одним проектом.

### Переменные окружения (пример)
```
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_DEFAULT_REGION=ru-msk
export AWS_ENDPOINT_URL=https://hb.vkcloud-storage.ru
export S3_BUCKET=s3-issue
```

### Скачивание проекта в .ishak (без archive)
```
mkdir -p .ishak
aws s3 sync \
  s3://$S3_BUCKET/projects/ishak/ \
  .ishak/ \
  --exclude "archive/*" \
  --endpoint-url $AWS_ENDPOINT_URL
```

### Загрузка изменений обратно (без archive)
```
aws s3 sync \
  .ishak/ \
  s3://$S3_BUCKET/projects/ishak/ \
  --exclude "archive/*" \
  --endpoint-url $AWS_ENDPOINT_URL
```

### Структура локальной папки
```
.ishak/
  README.md
  META.md
  issues/
    <issue_id>/
      issue.md
      comment_<timestamp>_<role>.md
      files/
```
