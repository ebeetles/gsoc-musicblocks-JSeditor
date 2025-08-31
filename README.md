# Advanced MusicBlocks JavaScript Editor and MIDI Generation Tool

This is a comprehensive summary of my work on the **Advanced JavaScript Editor with MusicBlocks Interactions** project during *Google Summer of Code 2025* with [Sugar Labs](https://github.com/sugarlabs/). This project focused on enhancing the JavaScript editor within the MusicBlocks environment to provide a seamless bridge between visual block-based programming and textual JavaScript coding.

## Abstract

[*Music Blocks*](https://musicblocks.sugarlabs.org/) is a block-based programming environment that teaches beginners how to program through the creation of music. While the existing system excelled at visual (block-based) programming, there was a significant gap in helping learners transition to text-based coding. The purpose of my project was **to develop an advanced JavaScript editor that enables bidirectional conversion between JavaScript code and MusicBlocks visual blocks, along with comprehensive debugging tools and syntax highlighting to create a complete learning environment**.

Additionally, I developed a **MusicBlocks Generation Model** that creates an end-to-end pipeline for generating music from natural language prompts using AI-powered retrieval-augmented generation (RAG). This system addresses the challenge of helping users create music when they don't know how to program, by providing an AI-powered generation system that outputs directly to MusicBlocks format.

This project addressed a critical educational need: children cannot learn to code without actually coding. Although MusicBlocks provided excellent visual programming concepts, learners needed a pathway to text-based programming. The JavaScript-to-MusicBlocks conversion feature enables smooth transitions between visual and textual programming, promoting deeper understanding of programming concepts. The generation model further lowers barriers by enabling music creation through natural language, creating multiple entry points for learners.

## Tech Stack

For code parsing and analysis, I integrated:
- **[Acorn Parser](https://github.com/acornjs/acorn)** for JavaScript AST generation
- **[Highlight.js](https://github.com/highlightjs/highlight.js/)** for syntax highlighting
- **Custom AST traversal** algorithms for code-to-block conversion

For the MusicBlocks Generation Model, I implemented:
- **[Google Gemini API](https://ai.google.dev/)** for AI-powered music generation and embeddings
- **[FAISS](https://github.com/facebookresearch/faiss)** for efficient vector similarity search
- **Vector embeddings** for MIDI file and metadata representation
- **MIDI processing** libraries for file manipulation and conversion

The project also required updating the existing MusicBlocks architecture, including:
- Block generation and manipulation
- Program execution logic
- UI/UX components for the editor interface
- MIDI-to-MusicBlocks conversion pipeline

## Core Project Features

### 1. JavaScript code to MusicBlocks Conversion

The project's main feature is a seamless translation from JavaScript code to MusicBlocks visual blocks. This conversion system provides learners with a smooth pathway from visual programming to text-based coding. Previously, there was only conversion from blocks to code, but now the other direction is possible.

**Key Capabilities:**
- **JavaScript to Blocks**: Convert any JavaScript code to equivalent MusicBlocks visual blocks
- **Real-time Conversion**: Instant translation with immediate visual feedback
- **Comprehensive Block Support**: Supports all major block types including rhythm, flow, number, boolean, pitch, tone, and action palettes

**Technical Implementation:**
The conversion system uses a sophisticated pipeline:
1. **AST Generation**: Acorn parser converts JavaScript code into Abstract Syntax Trees
2. **Pattern Matching**: Config-driven system matches AST patterns to block types
3. **Block Generation**: Creates MusicBlocks blocklist representation
4. **Connection Management**: Handles complex block relationships and spacing

**Config-Driven Architecture:**
The system uses JSON configuration files to define block mappings, making it easily extensible:

```json
{
    "name": "repeat",
    "comments": "Repeat block in the Flow palette",
    "arguments": [
        {
            "type": "NumberExpression"
        }
    ],
    "ast": {
        "identifiers": [
            {
                "property": "type",
                "value": "ForStatement"
            }
        ],
        "argument_properties": [
            "test.right"
        ],
        "children_properties": [
            "body.body"
        ]
    },
    "blocklist_connections": [
        "parent_or_previous_sibling",
        "argument",
        "first_child",
        "next_sibling"
    ],
    "default_vspaces": {
        "argument": 1
    }
}
```

### 2. Interactive Debugger System

The debugger represents a significant educational advancement, teaching essential problem-solving skills through interactive debugging tools.

**Core Functionality:**
- **Breakpoint System**: Set breakpoints directly in the JavaScript editor with visual indicators
- **Variable Inspection**: Real-time display of all user-defined and MusicBlocks variables
- **Step-by-Step Execution**: Integration with existing "run slowly" and "run step by step" features
- **Visual Block Integration**: Debugger statements convert to visual blocks for seamless debugging
- **Status Block Integration**: Automatic population of status blocks with relevant variables

**Educational Features:**
- **Problem-Solving Skills**: Teaches learners to break down problems and identify issues
- **Variable Tracking**: Shows how variables change during program execution
- **Execution Control**: Multiple ways to control program flow for debugging
- **Visual Feedback**: Clear indication of current execution state

**User Experience:**
- **Intuitive Controls**: Simple button-based interface suitable for young learners
- **Visual Integration**: Seamless integration with existing MusicBlocks workflow
- **Flexible Usage**: Multiple initiation methods for different debugging scenarios

### 3. Enhanced JavaScript Editor

The project includes a comprehensive JavaScript editor with professional-grade features designed for educational use.

**Editor Features:**
- **Syntax Highlighting**: Professional code highlighting for improved readability
- **Error Detection**: Visual indicators for syntax and logical errors
- **Multiple Themes**: Different color schemes for various learning preferences

**Educational Enhancements:**
- **Visual Appeal**: Engaging interface that makes coding fun for young learners
- **Code Readability**: Clear syntax highlighting improves understanding
- **Professional Feel**: Creates authentic programming environment
- **Accessibility**: Designed for diverse learning needs and abilities

### 4. Configurable Architecture

The project's architecture is designed for extensibility and maintainability, making it easy to add new features and block types.

**Architecture Benefits:**
- **Maintainability**: Clear separation of logic and configuration
- **Documentation**: Self-documenting block definitions
- **Testing**: Easily updated testing of configurations

**Technical Features:**
- **JSON Configuration**: Human-readable block definitions
- **AST Pattern Matching**: Flexible pattern matching for complex JavaScript constructs
- **Connection Management**: Sophisticated algorithms for block relationships
- **Error Handling**: Comprehensive error detection and reporting

### 5. Production-Ready Deployment

The project has been successfully deployed and integrated into the MusicBlocks environment.

**Deployment Features:**
- **Merged PRs**: Successfully integrated into main MusicBlocks repository
- **Comprehensive Testing**: Unit tests for all supported block types
- **Documentation**: Detailed guides for users and contributors
- **Performance Optimization**: Minified configurations for faster loading
- **User Testing**: Validated with real educational environments

The JSON-based configuration system enables:
- **Easy Extension**: Adding new blocks requires only config updates
- **Maintainability**: Clear separation of logic and configuration
- **Documentation**: Self-documenting block definitions
- **Testing**: Automated validation of configurations

## MusicBlocks Generation Model

In addition to the JavaScript Editor project, I developed a **MusicBlocks Generation Model** that creates an end-to-end pipeline for generating music from natural language prompts. This project addresses the challenge of helping users create music when they don't know how to program, by providing an AI-powered generation system that outputs directly to MusicBlocks format.

### Project Overview

The MusicBlocks Generation Model uses Retrieval-Augmented Generation (RAG) to create music based on user queries like "guitar solo with similar style to hotel california" or "jazz piano piece". The system generates MIDI files that are then automatically converted to MusicBlocks projects, creating a complete workflow from natural language input to visual programming blocks.

A walkthrough is shown below:

https://youtu.be/5Ha7WlbHOfc

### Core Components

#### 1. RAG Pipeline for Music Generation

**Data Collection & Processing:**
- **MIDI Dataset**: Collected and cleaned a comprehensive dataset of MIDI files
- **Metadata Extraction**: Extracted artist names, song titles, musical styles, BPM, and other musical characteristics
- **Vector Embedding**: Used Gemini embedding model to create vector representations of MIDI data and metadata
- **Similarity Search**: Implemented retrieval system that finds musically similar pieces based on user queries

**Generation Process:**
- **Query Processing**: User inputs natural language description of desired music
- **Retrieval**: System finds similar musical pieces from the dataset
- **AI Generation**: Uses Gemini API with carefully engineered prompts to generate new MIDI files
- **Style Preservation**: Maintains musical characteristics and style of requested music

#### 2. MIDI Upload Widget

**Core Functionality:**
- **File Upload**: Allows users to upload any MIDI file for conversion to MusicBlocks
- **Automatic Conversion**: Converts uploaded MIDI files to MusicBlocks visual blocks
- **Direct Generation**: Integrated with RAG pipeline for prompt-based music generation
- **Seamless Integration**: Bridges the gap between AI-generated music and MusicBlocks format

**User Experience:**
- **Simple Interface**: Intuitive widget design suitable for young learners
- **Multiple Input Methods**: Support for both file uploads and text prompts
- **Real-time Conversion**: Immediate visual feedback during the conversion process

### Technical Architecture

**RAG System Components:**
- **Vector Database**: Stores embeddings of MIDI files and metadata for efficient retrieval
- **Embedding Model**: Gemini-based embeddings for improved accuracy
- **Generation Model**: Gemini API integration for high-quality MIDI generation
- **Prompt Engineering**: Carefully crafted prompts for consistent musical output

**Integration Points:**
- **MIDI Processing**: Handles various MIDI file formats and characteristics
- **MusicBlocks Conversion**: Automatic conversion from MIDI to visual blocks
- **Error Handling**: Robust error handling for corrupted or malformed files

### Key Achievements

**Week 8-10 Progress:**
- **Week 8**: Built complete RAG pipeline with MIDI dataset and vector embeddings
- **Week 9**: Integrated Gemini embedding model and created MIDI upload widget
- **Week 10**: Connected RAG pipeline with MIDI widget for end-to-end generation

**Quality Improvements:**
- **Enhanced Retrieval**: Gemini embeddings significantly improved similarity search accuracy
- **Better Generation**: Improved prompt engineering and model selection for higher quality MIDI output
- **Complete Workflow**: Successfully demonstrated end-to-end generation from prompt to MusicBlocks

### Educational Impact

The MusicBlocks Generation Model serves as a powerful educational tool by:
- **Lowering Barriers**: Enables music creation without programming knowledge
- **Inspiration**: Provides starting points for learners to modify and experiment
- **Learning Pathway**: Helps users understand how musical concepts translate to visual blocks
- **Creativity**: Encourages exploration of different musical styles and genres

## Challenges and Solutions

### Major Technical Challenges

1. **Complex Block Relationships**
   - **Challenge**: MusicBlocks blocks have intricate connection patterns
   - **Solution**: Developed sophisticated connection management algorithms

2. **AST Pattern Matching**
   - **Challenge**: JavaScript constructs don't map directly to block structures
   - **Solution**: Created flexible pattern matching with configurable AST traversal

3. **Music Generation Quality**
   - **Challenge**: Initial MIDI generation had quality and tempo issues
   - **Solution**: Improved prompt engineering and model selection, with ongoing tempo optimization

4. **User Experience Design**
   - **Challenge**: Creating intuitive tools for young learners
   - **Solution**: Focused on discoverable, simple interactions with powerful underlying functionality

## Conclusion

The Advanced JavaScript Editor with MusicBlocks Interactions project successfully bridges the gap between visual block-based programming and textual JavaScript coding. By providing bidirectional conversion, comprehensive debugging tools, and enhanced editor features, the project creates a complete learning environment that empowers young programmers to develop both visual and textual programming skills.

The MusicBlocks Generation Model adds another dimension to this educational ecosystem by enabling AI-powered music creation that directly integrates with the visual programming environment. This creates a comprehensive platform where users can learn programming through music creation, whether starting with visual blocks, JavaScript code, or natural language descriptions.