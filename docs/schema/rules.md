# Validation Rules Schema

## Overview

This document describes the rules and conventions used by breakdown to convert Markdown to JSON.

## Node Types

### Headings
- Level (1-6) required
- Content required
- Empty headings not allowed

### Lists
- Can be nested up to 3 levels
- Both ordered and unordered lists are supported
- List items require content

### Code Blocks
- Language specification required
- Content can be empty
- Supports syntax highlighting hints

## Validation Rules

The validator checks:
1. All nodes have required properties
2. Node types are valid
3. Content meets format requirements
4. Structure follows nesting rules 