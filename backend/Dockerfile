FROM python:3.11-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

WORKDIR /app

COPY app.py .

CMD ["python", "app.py"]