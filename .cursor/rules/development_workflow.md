# Development Workflow - Kiro + cc-sdd + CanonTDD + Claude Code

**Four-Phase Methodology for AI-Driven Development**

This document defines the exact workflow for implementing features in this project.

## Phase 1: Kiro - User Story Definition

Define WHAT needs to be built with structured context.

### Kiro Format

```
Feature: [Feature Name]

GIVEN [context/precondition]
WHEN [action/event occurs]
THEN [expected outcome/result]
```

### Example: Trend Fetching

```
Feature: Fetch trends from HackerNews

GIVEN the HackerNews API is available
WHEN I call fetch_hackernews_trends()
THEN I should receive a list of top 30 trends with rank, title, and score
AND each trend should be validated for non-empty fields
AND the list should be sorted by score descending
AND if API fails, fallback to cached trends from 24 hours ago
```

**Key Questions to Answer:**
- What is the exact input/output?
- - What are the edge cases?
  - - What happens on error?
    - - What data validation is needed?
     
      - ---

      ## Phase 2: cc-sdd - API Specification Generation

      Generate formal OpenAPI specification via Claude.

      ### What Claude generates:

      1. **Input/Output Schemas** (JSON Schema)
      2. 2. **Error Cases** (all possible failure modes)
         3. 3. **Type Definitions** (Python TypedDict/dataclass)
            4. 4. **Validation Rules** (what makes input valid)
               5. 5. **Retry Strategy** (exponential backoff rules)
                 
                  6. ### Example Output Structure
                 
                  7. ```yaml
                     openapi: 3.0.0
                     paths:
                       /trends/fetch:
                         post:
                           requestBody:
                             schema:
                               type: object
                               properties:
                                 source: {enum: [hackernews, reddit]}
                                 limit: {type: integer, default: 30}
                           responses:
                             200:
                               schema:
                                 type: array
                                 items:
                                   $ref: '#/components/schemas/Trend'
                             429:
                               description: "Rate limit exceeded - retry after 60s"
                             500:
                               description: "API error - use fallback cache"

                     components:
                       schemas:
                         Trend:
                           type: object
                           required: [id, title, score]
                           properties:
                             id: {type: string}
                             title: {type: string, minLength: 1}
                             score: {type: integer, minimum: 0}
                     ```

                     ### Claude Prompts Used

                     **Prompt**: Generate OpenAPI spec for [feature]
                     - Include all error cases
                     - - Define validation rules
                       - - Specify retry strategy
                         - - Add type hints for Python 3.11+
                          
                           - ---

                           ## Phase 3: CanonTDD - Test-Driven Development

                           Implement RED → GREEN → REFACTOR cycle.

                           ### RED Phase

                           Write failing test that validates the spec from Phase 2.

                           ```python
                           import pytest
                           from src.trend_fetcher import fetch_hackernews_trends, TrendFetchError

                           def test_fetch_hackernews_returns_sorted_trends():
                               """RED: This test fails before implementation"""
                               result = fetch_hackernews_trends(limit=10)

                               assert isinstance(result, list)
                               assert len(result) == 10
                               assert all(hasattr(t, 'id') for t in result)
                               assert all(hasattr(t, 'title') for t in result)
                               assert all(hasattr(t, 'score') for t in result)
                               # Verify sorted by score descending
                               assert result[0].score >= result[-1].score

                           def test_fetch_hackernews_handles_api_timeout():
                               """RED: Test error handling"""
                               with pytest.raises(TrendFetchError):
                                   fetch_hackernews_trends(timeout=0.001)

                           def test_fetch_hackernews_uses_cache_on_failure(mocker):
                               """RED: Test fallback behavior"""
                               mocker.patch('requests.get', side_effect=ConnectionError())
                               result = fetch_hackernews_trends()
                               assert result is not None  # Should use cache
                           ```

                           ### GREEN Phase

                           Write minimal implementation to pass tests.

                           ```python
                           from dataclasses import dataclass
                           from typing import List
                           import requests

                           @dataclass
                           class Trend:
                               id: str
                               title: str
                               score: int

                           def fetch_hackernews_trends(limit: int = 30, timeout: int = 10) -> List[Trend]:
                               """Fetch top trends from HackerNews"""
                               try:
                                   response = requests.get(
                                       'https://hacker-news.firebaseio.com/v0/topstories.json',
                                       timeout=timeout
                                   )
                                   response.raise_for_status()

                                   story_ids = response.json()[:limit]
                                   trends = []

                                   for sid in story_ids:
                                       try:
                                           item = requests.get(
                                               f'https://hacker-news.firebaseio.com/v0/item/{sid}.json',
                                               timeout=timeout
                                           ).json()
                                           trends.append(Trend(
                                               id=str(sid),
                                               title=item['title'],
                                               score=item['score']
                                           ))
                                       except (KeyError, requests.RequestException):
                                           continue

                                   return sorted(trends, key=lambda t: t.score, reverse=True)
                               except requests.RequestException:
                                   return load_cached_trends()
                           ```

                           ### REFACTOR Phase

                           Improve code while tests still pass.

                           - Extract retry logic
                           - - Add comprehensive logging
                             - - Optimize data structures
                               - - Improve docstrings
                                 - - Add type hints everywhere
                                  
                                   - ```python
                                     from typing import List
                                     from tenacity import retry, stop_after_attempt, wait_exponential

                                     @retry(
                                         stop=stop_after_attempt(3),
                                         wait=wait_exponential(multiplier=1, min=2, max=10)
                                     )
                                     def fetch_hackernews_trends(limit: int = 30, timeout: int = 10) -> List[Trend]:
                                         """
                                         Fetch top trends from HackerNews API.

                                         Retries up to 3 times with exponential backoff on failure.
                                         Falls back to cached trends if all retries fail.

                                         Args:
                                             limit: Maximum number of trends to return (default: 30)
                                             timeout: HTTP request timeout in seconds (default: 10)

                                         Returns:
                                             List of Trend objects sorted by score (descending)

                                         Raises:
                                             TrendFetchError: If both API and cache fail

                                         Example:
                                             >>> trends = fetch_hackernews_trends(limit=10)
                                             >>> print(trends[0].title)
                                         """
                                         # Implementation...
                                     ```

                                     ### Testing Requirements (CRITICAL)

                                     ✅ **MUST HAVE:**
                                     - 100% test pass rate before committing
                                     - - All edge cases covered:
                                       -   - Empty response
                                           -   - Null values
                                               -   - API timeout (with retry)
                                                   -   - Network error (fallback to cache)
                                                       -   - Invalid JSON response
                                                           -   - Rate limiting (429 status)
                                                               - - Type hints on all functions
                                                                 - - Comprehensive docstrings
                                                                  
                                                                   - ❌ **DO NOT:**
                                                                   - - Skip tests to "move faster"
                                                                     - - Use try/except without logging
                                                                       - - Ignore type checking errors
                                                                         - - Commit untested code paths
                                                                          
                                                                           - ---

                                                                           ## Phase 4: Claude Code - Interactive Implementation

                                                                           Use Claude Code to implement based on Phase 3 tests.

                                                                           ### Claude Code Workflow

                                                                           1. **Initialize**: Claude Code reads `project_overview.md` and `test_strategy.md`
                                                                           2. 2. **Review**: Check existing tests from Phase 3
                                                                              3. 3. **Implement**: Write production code to pass all tests
                                                                                 4. 4. **Validate**: Run tests, ensure 100% pass rate
                                                                                    5. 5. **Refactor**: Optimize while maintaining test pass rate
                                                                                      
                                                                                       6. ### What Claude Code Does NOT Do
                                                                                      
                                                                                       7. - ❌ Decide WHAT to build (that's Phase 1-3)
                                                                                          - - ❌ Skip testing
                                                                                            - - ❌ Make breaking changes
                                                                                              - - ❌ Modify API signature without test updates
                                                                                               
                                                                                                - ### What Claude Code DOES Do
                                                                                               
                                                                                                - - ✅ Write production code matching the spec
                                                                                                  - - ✅ Add comprehensive error handling
                                                                                                    - - ✅ Optimize performance (in REFACTOR phase)
                                                                                                      - - ✅ Generate documentation
                                                                                                        - - ✅ Validate all edge cases
                                                                                                         
                                                                                                          - ---
                                                                                                          
                                                                                                          ## Complete Workflow Example: Trend Fetching
                                                                                                          
                                                                                                          ### Step 1: Kiro (Write User Stories)
                                                                                                          
                                                                                                          ```
                                                                                                          Feature: Fetch and cache HackerNews trends

                                                                                                          GIVEN HackerNews API is accessible
                                                                                                          WHEN I request top 30 stories
                                                                                                          THEN I receive Trend objects with id, title, score
                                                                                                          AND results are sorted by score (descending)
                                                                                                          AND each Trend is validated before returning

                                                                                                          GIVEN HackerNews API is down
                                                                                                          WHEN I request trends
                                                                                                          THEN return cached trends from last successful fetch
                                                                                                          AND log warning about API failure
                                                                                                          ```
                                                                                                          
                                                                                                          ### Step 2: cc-sdd (Generate Spec)
                                                                                                          
                                                                                                          **Prompt to Claude:**
                                                                                                          ```
                                                                                                          Generate OpenAPI 3.0 specification for the following:
                                                                                                          - Fetch top N trends from HackerNews
                                                                                                          - Return Trend objects with: id (string), title (string), score (int)
                                                                                                          - Handle all error cases (timeout, network, invalid response)
                                                                                                          - Specify retry strategy for transient failures
                                                                                                          - Include Python type hints for dataclass
                                                                                                          ```
                                                                                                          
                                                                                                          **Claude Output**: See `prompts/api_specification.md`
                                                                                                          
                                                                                                          ### Step 3: CanonTDD (Write Tests First)
                                                                                                          
                                                                                                          ```bash
                                                                                                          # RED: Tests fail
                                                                                                          pytest tests/test_trend_fetcher.py -v

                                                                                                          # Then implement features to pass tests
                                                                                                          ```
                                                                                                          
                                                                                                          ### Step 4: Claude Code (Implement)
                                                                                                          
                                                                                                          ```bash
                                                                                                          # Open Claude Code
                                                                                                          claude code

                                                                                                          # Claude reads:
                                                                                                          # - project_overview.md
                                                                                                          # - development_workflow.md (this file)
                                                                                                          # - test_strategy.md
                                                                                                          # - tests/test_trend_fetcher.py (existing tests)

                                                                                                          # Claude implements until all tests pass
                                                                                                          ```
                                                                                                          
                                                                                                          ---
                                                                                                          
                                                                                                          ## Key Metrics
                                                                                                          
                                                                                                          **Success Criteria:**
                                                                                                          - ✅ Test pass rate: 100%
                                                                                                          - - ✅ Type hint coverage: 100%
                                                                                                            - - ✅ Edge case coverage: All documented cases
                                                                                                              - - ✅ Docstring coverage: All public functions
                                                                                                                - - ✅ Code review: Approved before merge
                                                                                                                 
                                                                                                                  - **Validation Commands:**
                                                                                                                 
                                                                                                                  - ```bash
                                                                                                                    # Run tests
                                                                                                                    pytest tests/ -v --cov=src/

                                                                                                                    # Type checking
                                                                                                                    mypy src/ --strict

                                                                                                                    # Code formatting
                                                                                                                    black src/ --check
                                                                                                                    ruff check src/

                                                                                                                    # Commit only if all pass
                                                                                                                    git add .
                                                                                                                    git commit -m "feat: [feature name]"
                                                                                                                    ```
                                                                                                                    
                                                                                                                    ---
                                                                                                                    
                                                                                                                    ## When to Use Each Phase
                                                                                                                    
                                                                                                                    | Situation | Phase |
                                                                                                                    |-----------|-------|
                                                                                                                    | Adding new feature | Kiro → cc-sdd → CanonTDD → Claude Code |
                                                                                                                    | Fixing bug | cc-sdd (for edge case) → CanonTDD (new test) → Claude Code |
                                                                                                                    | Refactoring | CanonTDD (existing tests) → Claude Code |
                                                                                                                    | Optimizing | CanonTDD (benchmark test) → Claude Code (REFACTOR only) |
                                                                                                                    
                                                                                                                    ---
                                                                                                                    
                                                                                                                    ## Error Recovery
                                                                                                                    
                                                                                                                    ### If Tests Fail in Phase 4
                                                                                                                    
                                                                                                                    1. **Review failing test** - What's the expectation?
                                                                                                                    2. 2. **Check implementation** - Does it match spec from Phase 2?
                                                                                                                       3. 3. **Fix minimal code** - Change only what's needed
                                                                                                                          4. 4. **Re-run tests** - Verify fix
                                                                                                                             5. 5. **Refactor** - Now that tests pass again
                                                                                                                               
                                                                                                                                6. ### If Spec is Wrong
                                                                                                                               
                                                                                                                                7. 1. **Update Kiro** (Phase 1) with correct requirements
                                                                                                                                   2. 2. **Regenerate cc-sdd spec** (Phase 2) with Claude
                                                                                                                                      3. 3. **Update tests** (Phase 3) to match new requirements
                                                                                                                                         4. 4. **Re-implement** (Phase 4) with Claude Code
                                                                                                                                           
                                                                                                                                            5. ---
                                                                                                                                           
                                                                                                                                            6. **Status**: Phase A - Initialization
                                                                                                                                            7. **Reference**: See CLAUDE.md for quick links
