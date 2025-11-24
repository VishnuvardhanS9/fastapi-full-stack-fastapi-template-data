# ---------------------------------------------
# Backend Dockerfile for uv + FastAPI
# ---------------------------------------------

FROM python:3.12-slim AS builder

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    python3-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install uv globally
RUN pip install uv

WORKDIR /app

# Copy only dependency files first (for caching)
COPY pyproject.toml uv.lock ./

# Create virtual environment & install dependencies using uv sync
RUN uv venv /app/.venv
RUN UV_PROJECT_ENV=/app/.venv uv sync --no-dev

# Copy project code
COPY . .

# ---------------------------------------------
# Final minimal runtime image
# ---------------------------------------------
FROM python:3.12-slim

WORKDIR /app

# Copy virtual environment from builder
COPY --from=builder /app/.venv .venv
COPY --from=builder /app /app

ENV PATH="/app/.venv/bin:$PATH"

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
