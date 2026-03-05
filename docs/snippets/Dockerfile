FROM python:3.11
ENV PYTHONUNBUFFERED=1

WORKDIR /workspace

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY main.py .
COPY model.py .
COPY test.py .

CMD ["python", "main.py"]