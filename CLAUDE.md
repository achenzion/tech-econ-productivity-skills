# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

Exploratory repository for skills useful for the tech economist. This is a greenfield Python project — no application code, packaging config, or tooling has been set up yet.

## Repository State

The `.gitignore` is pre-configured for a Python project and hints at intended tooling:
- Package managers: `uv`, `pixi`, `PDM`, or `pip` with `requirements.txt`
- Testing: `pytest`
- Linting/formatting: `ruff`
- Type checking: `mypy`
- Notebooks: Jupyter and Marimo
- Automation framework: Abstra

No `pyproject.toml`, `requirements.txt`, `Makefile`, or test configuration exists yet. When adding these, follow the conventions already implied by `.gitignore`.

## Development Setup

As the project is set up, document the following here:
- How to install dependencies
- How to run tests (`pytest` is expected)
- How to run linting (`ruff` is expected)
- How to run the project
