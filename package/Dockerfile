FROM python:3.9-alpine AS base

FROM base AS build

# We need to build cffi, so we will use multi-stage build
# RUN apk add --no-cache gcc libc-dev make git libffi-dev openssl-dev python3-dev libxml2-dev libxslt-dev g++ cargo \
#     && pip install --no-cache-dir -U pip

RUN pip install --no-cache-dir -U pip

WORKDIR /app

COPY ./requirements.txt ./

RUN pip install --no-cache-dir -r requirements.txt

#################################
FROM base AS prod

ENV PYTHONUNBUFFERED=1

WORKDIR /app

# RUN apk add libgcc --no-cache
# COPY --from=build /usr/lib/ /usr/lib/
COPY --from=build /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/
COPY --from=build /usr/local/bin/ /usr/local/bin/
# COPY pyproject.toml ./
COPY scripts/docker-entrypoint.sh ./
# COPY assets/migrations ./assets/migrations
COPY app ./app

EXPOSE 8000

ENTRYPOINT ["/app/docker-entrypoint.sh"]