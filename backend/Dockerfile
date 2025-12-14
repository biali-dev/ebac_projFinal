FROM python:3.12-slim

# Environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    POETRY_VERSION=1.7.1 \
    POETRY_HOME="/opt/poetry" \
    POETRY_NO_INTERACTION=1

ENV PATH="$POETRY_HOME/bin:$PATH"

# System dependencies
RUN apt-get update && apt-get install --no-install-recommends -y \
    curl \
    build-essential \
    libpq-dev \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Set workdir
WORKDIR /app

# Copy dependency files first (cache-friendly)
COPY pyproject.toml poetry.lock ./

# Install dependencies (no venv inside container)
RUN poetry config virtualenvs.create false \
    && poetry install --no-dev --no-ansi

# Copy project
COPY . .

# Expose port
EXPOSE 8000

# Start Django with Gunicorn
CMD ["gunicorn", "project_name.wsgi:application", "--bind", "0.0.0.0:8000"]
