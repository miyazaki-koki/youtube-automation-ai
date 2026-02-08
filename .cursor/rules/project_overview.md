# Project Overview - youtube-automation-ai

**AI-driven YouTube content automation with Kiro + cc-sdd + CanonTDD + Claude Code**

## Vision

Automate multi-genre YouTube video creation using AI-driven development, generating conclusion-first, high-density content that maximizes viewer engagement through global trend analysis and Japanese market localization.

### Three-Phase Monetization Strategy

**Phase A: Self-Operation (Current)**
- Run own YouTube channels
- - Target: ¥30,000-50,000/month from 100-200 auto-generated videos
  - - Revenue: YouTube AdSense + Affiliate marketing
   
    - **Phase B: SaaS-ification**
    - - Package automation as subscription service
      - - Target: 100+ users × ¥2,000/month = ¥2M monthly
       
        - **Phase C: Hybrid**
        - - Continue Phase A for direct revenue
          - - Parallel Phase B for recurring SaaS revenue
            - - Maximum income diversification
             
              - ## Content Strategy
             
              - ### Video Structure (CRITICAL)
             
              - Each video must follow this exact structure:
              - 1. **Conclusion-First Opening** (0:00-0:15)
                2.    - Lead with the key insight/conclusion
                      -    - Hook viewers immediately
                       
                           -    2. **Multi-Genre Segments** (3+ topics per video)
                                3.    - Conclusion → Detail → Proof for each segment
                                      -    - High-density information packing
                                       
                                           -    3. **Suspenseful Transitions**
                                                4.    - Questions between segments
                                                      -    - Maintain engagement momentum
                                                       
                                                           -    4. **Final Connection**
                                                                5.    - Tie all genres together
                                                                      -    - Reinforce main conclusion
                                                                       
                                                                           - ### Minimum Viable Components (Phase A)
                                                                           - - ✅ Video (MP4 with H.264)
                                                                             - - ✅ Audio (MP3 from text-to-speech)
                                                                               - - ✅ Subtitles (SRT format)
                                                                                 - - ⏳ Avatars (Phase B enhancement)
                                                                                  
                                                                                   - ### Content Focus
                                                                                   - - Multiple genres per video
                                                                                     - - Global trend detection → Japanese localization
                                                                                       - - Information density over length
                                                                                         - - Engagement through narrative tension
                                                                                          
                                                                                           - ## Tech Architecture
                                                                                          
                                                                                           - ### External APIs
                                                                                           - - **Claude API** - Script generation, trend analysis, localization
                                                                                             - - **ElevenLabs API** - High-quality TTS for narration
                                                                                               - - **YouTube Data API** - Upload and scheduling
                                                                                                 - - **Reddit API** - Trend detection
                                                                                                   - - **HackerNews API** - Tech trend mining
                                                                                                     - - **Google Trends API** - Global trend tracking
                                                                                                      
                                                                                                       - ### Processing Stack
                                                                                                       - - **FFmpeg** - Video composition
                                                                                                         - - **MoviePy** - Video manipulation
                                                                                                           - - **pydub** - Audio processing
                                                                                                            
                                                                                                             - ### Development Tools
                                                                                                             - - **pytest** - Testing framework
                                                                                                               - - **mypy** - Type checking
                                                                                                                 - - **black** - Code formatting
                                                                                                                   - - **ruff** - Linting
                                                                                                                    
                                                                                                                     - ## Development Methodology
                                                                                                                    
                                                                                                                     - ### Workflow: Kiro → cc-sdd → CanonTDD → Claude Code
                                                                                                                    
                                                                                                                     - **1. Kiro Phase (User Story Definition)**
                                                                                                                     - ```
                                                                                                                       GIVEN [user story context]
                                                                                                                       WHEN [action occurs]
                                                                                                                       THEN [expected outcome]
                                                                                                                       ```
                                                                                                                       
                                                                                                                       **2. cc-sdd Phase (API Specification)**
                                                                                                                       - Claude generates formal OpenAPI specs
                                                                                                                       - - Input/output schemas defined
                                                                                                                         - - Error handling paths documented
                                                                                                                           - - Edge cases explicitly stated
                                                                                                                            
                                                                                                                             - **3. CanonTDD Phase (Test-Driven Development)**
                                                                                                                             - - RED: Write failing test
                                                                                                                               - - GREEN: Write minimal implementation
                                                                                                                                 - - REFACTOR: Improve code while tests pass
                                                                                                                                   - - 100% pass rate required before moving forward
                                                                                                                                    
                                                                                                                                     - **4. Claude Code Phase (Interactive Implementation)**
                                                                                                                                     - - Implement based on spec + tests
                                                                                                                                       - - All edge cases must be covered
                                                                                                                                         - - Type hints required
                                                                                                                                           - - Comprehensive docstrings required
                                                                                                                                            
                                                                                                                                             - ### Key Rules
                                                                                                                                            
                                                                                                                                             - **MUST:**
                                                                                                                                             - - ✅ Implement Kiro + cc-sdd + CanonTDD + Claude Code
                                                                                                                                               - - ✅ Achieve 100% test pass rate
                                                                                                                                                 - - ✅ Preserve all edge cases
                                                                                                                                                   - - ✅ Use Python 3.11+ with type hints
                                                                                                                                                     - - ✅ Generate comprehensive docstrings
                                                                                                                                                       - - ✅ Validate all API responses
                                                                                                                                                         - - ✅ Handle timeouts with exponential backoff
                                                                                                                                                           - - ✅ Cache trend data for fallback scenarios
                                                                                                                                                            
                                                                                                                                                             - **DO NOT:**
                                                                                                                                                             - - ❌ Implement avatars in Phase A
                                                                                                                                                               - - ❌ Skip testing for any module
                                                                                                                                                                 - - ❌ Use ambiguous variable names
                                                                                                                                                                   - - ❌ Optimize prematurely (optimize in REFACTOR only)
                                                                                                                                                                     - - ❌ Over-engineer Phase A solutions
                                                                                                                                                                       - - ❌ Ignore error handling
                                                                                                                                                                         - - ❌ Create untested code paths
                                                                                                                                                                          
                                                                                                                                                                           - ## Project Structure
                                                                                                                                                                          
                                                                                                                                                                           - ```
                                                                                                                                                                             youtube-automation-ai/
                                                                                                                                                                             ├── CLAUDE.md                    # This context file
                                                                                                                                                                             ├── .cursor/
                                                                                                                                                                             │   └── rules/
                                                                                                                                                                             │       ├── project_overview.md  # Project vision & architecture
                                                                                                                                                                             │       ├── development_workflow.md
                                                                                                                                                                             │       ├── implementation_guidelines.md
                                                                                                                                                                             │       ├── content_structure.md
                                                                                                                                                                             │       └── test_strategy.md
                                                                                                                                                                             ├── src/
                                                                                                                                                                             │   ├── trend_fetcher/           # Phase 1: Fetch global trends
                                                                                                                                                                             │   ├── script_generator/        # Phase 1: Generate scripts
                                                                                                                                                                             │   ├── audio_generator/         # Phase 1: TTS generation
                                                                                                                                                                             │   ├── video_compositor/        # Phase 1: Video assembly
                                                                                                                                                                             │   ├── youtube_uploader/        # Phase 1: Upload to YouTube
                                                                                                                                                                             │   └── localization/            # Global → Japan localization
                                                                                                                                                                             ├── tests/
                                                                                                                                                                             │   ├── test_trend_fetcher.py
                                                                                                                                                                             │   ├── test_script_generator.py
                                                                                                                                                                             │   ├── test_audio_generator.py
                                                                                                                                                                             │   ├── test_video_compositor.py
                                                                                                                                                                             │   └── test_youtube_uploader.py
                                                                                                                                                                             ├── prompts/
                                                                                                                                                                             │   ├── trend_analysis.md
                                                                                                                                                                             │   ├── script_generation.md
                                                                                                                                                                             │   └── localization.md
                                                                                                                                                                             ├── pyproject.toml
                                                                                                                                                                             ├── pytest.ini
                                                                                                                                                                             └── README.md
                                                                                                                                                                             ```
                                                                                                                                                                             
                                                                                                                                                                             ## Constraint Summary
                                                                                                                                                                             
                                                                                                                                                                             ### API Failures
                                                                                                                                                                             - All trend sources down? → Use cached trends + fallback list
                                                                                                                                                                             - - Claude API timeout? → Retry with exponential backoff
                                                                                                                                                                               - - Invalid response? → Validate + retry with same prompt
                                                                                                                                                                                
                                                                                                                                                                                 - ### Data Validation
                                                                                                                                                                                 - - All API responses must be validated before use
                                                                                                                                                                                   - - Type hints required for all function signatures
                                                                                                                                                                                     - - No silent failures - all errors logged
                                                                                                                                                                                      
                                                                                                                                                                                       - ### Testing Standards
                                                                                                                                                                                       - - RED → GREEN → REFACTOR cycle mandatory
                                                                                                                                                                                         - - Edge cases: empty lists, null values, timeouts, API errors
                                                                                                                                                                                           - - Batch processing: handle 1000+ items efficiently
                                                                                                                                                                                             - - All tests must pass before committing
                                                                                                                                                                                              
                                                                                                                                                                                               - ## Next Steps
                                                                                                                                                                                              
                                                                                                                                                                                               - 1. ✅ Create `.cursor/rules/project_overview.md` (THIS FILE)
                                                                                                                                                                                                 2. 2. ⏳ Create `.cursor/rules/development_workflow.md`
                                                                                                                                                                                                    3. 3. ⏳ Create `.cursor/rules/implementation_guidelines.md`
                                                                                                                                                                                                       4. 4. ⏳ Create `.cursor/rules/content_structure.md`
                                                                                                                                                                                                          5. 5. ⏳ Create `.cursor/rules/test_strategy.md`
                                                                                                                                                                                                             6. 6. ⏳ Initialize Python project (pyproject.toml)
                                                                                                                                                                                                                7. 7. ⏳ Set up GitHub Actions CI/CD
                                                                                                                                                                                                                   8. 8. ⏳ Phase 1: Implement trend fetching
                                                                                                                                                                                                                      9. 9. ⏳ Phase 1: Implement script generation
                                                                                                                                                                                                                         10. 10. ⏳ Phase 1: Implement audio/video pipeline
                                                                                                                                                                                                                            
                                                                                                                                                                                                                             11. ---
                                                                                                                                                                                                                            
                                                                                                                                                                                                                             12. **Status**: Phase A - Initialization
                                                                                                                                                                                                                             13. **Last Updated**: 2025-01-XX
                                                                                                                                                                                                                             14. **Reference**: See CLAUDE.md for quick access links
