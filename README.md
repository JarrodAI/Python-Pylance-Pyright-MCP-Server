# Python-Pylance-Pyright-MCP-Server

Pylance MCP+ extends Microsoft's Pylance language server with professional-grade tooling for Python development. Get everything Microsoft offers PLUS ultra-fast Ruff linting, Bandit security scanning, Radon complexity analysis, and more. Perfect for professional developers who need enterprise-quality code analysis.

Free tier includes all core Pylance features. Professional tier ($19/mo) adds security scanning, linting, and code quality tools.

We do not collect data. 

## How AI Uses Pylance MCP+

### What is MCP (Model Context Protocol)?
MCP is a protocol that allows AI assistants (Claude, Cursor, GitHub Copilot, etc.) to call external tools. When you chat with an AI assistant that has Pylance MCP+ installed, the AI can automatically use Python language server capabilities without you explicitly asking.

### Example Conversations

**User:** "Rename the User class to UserAccount everywhere"

**Behind the scenes:**
1. AI sees your request
2. AI discovers it has a `rename_symbol` tool available via MCP
3. AI calls: `rename_symbol(file_path="src/models.py", line=5, character=6, new_name="UserAccount")`
4. Pylance MCP+ executes the rename across 47 locations
5. AI responds: "✅ Done! Renamed User → UserAccount in 47 locations across 12 files"

**You never see the MCP layer - it's completely transparent.**

---

**User:** "Are there any security issues in this file?"

**Behind the scenes:**
1. AI calls: `security_scan(file_path="src/api.py")`
2. Bandit scanner detects hardcoded password on line 23
3. AI responds: "⚠️ Found 1 HIGH severity issue: Hardcoded password on line 23. Recommendation: Use environment variables or a secrets manager."

---

### How AI Learns to Use Your Tools

AI assistants discover MCP tools through **tool schemas**. When Pylance MCP+ connects, it tells the AI:

```json
{
  "tool": "security_scan",
  "description": "Scan Python code for security vulnerabilities using Bandit",
  "parameters": {
    "file_path": "Path to Python file to scan",
    "content": "Optional: Code content if not saved to file"
  }
}
```

The AI reads these schemas and learns:
- **What** tools are available
- **When** to use them (based on description)
- **How** to call them (parameters required)

**You can enhance AI understanding by adding USAGE_GUIDE.md to your AI's instructions** (see below).

---

## Training Data Collection (Professional/AI-Enhanced Tiers)

### What Gets Collected
When you use Pylance MCP+ with AI assistants, we can collect anonymized training data:

**Logged automatically:**
- User prompts: "Rename this function", "Check for type errors"
- AI responses: "Renamed in 15 locations", "Found 3 type errors"
- Tool calls: Which MCP tools the AI used and why
- Success/failure rates: Did the AI choose the right tool?

**What we DON'T collect:**
- Your source code
- File contents
- Proprietary information
- Personal data

### Why Collect Training Data?

**1. Improve AI-MCP Integration**
- See which prompts work best
- Identify when AI chooses wrong tools
- Optimize tool descriptions for better AI understanding

**2. Build Custom Models (AI-Enhanced Tier)**
- Train models specific to your team's coding patterns
- AI learns your codebase terminology
- Personalized suggestions based on your history

**3. Aggregate Insights (Enterprise Tier)**
- Team-wide code quality trends
- Most common type errors
- Security vulnerabilities by project type

### How to Use Training Data

**Export your data:**
```bash
pylance-mcp export-training-data --format jsonl --output my_training_data.jsonl
```

**What you get:**
```jsonl
{"timestamp": "2025-12-03T10:30:00Z", "user_prompt": "Check for type errors", "tool_called": "type_check", "success": true}
{"timestamp": "2025-12-03T10:31:15Z", "user_prompt": "Scan for security issues", "tool_called": "security_scan", "result": "Found 2 issues"}
```

**Use cases:**
- Fine-tune your own LLM on Python development workflows
- Analyze team productivity patterns
- Generate reports on code quality improvements
- Train custom coding assistants for your company

### Privacy & Control

✅ **Opt-in only:** Training data collection is OFF by default  
✅ **Anonymized:** No source code or sensitive data collected  
✅ **You own it:** Export anytime, delete anytime  
✅ **Transparent:** See exactly what's logged before it's sent  
✅ **Compliant:** GDPR, CCPA, SOC 2 ready (Enterprise tier)

**To enable:**
```json
{
  "mcpServers": {
    "pylance-mcp": {
      "args": ["--api-key", "YOUR_KEY", "--enable-training-data"]
    }
  }
}
```
🤖 Optimize Your AI Assistant (Optional but Recommended):

   To teach your AI how to best use Pylance MCP tools,
   add this to your AI assistant's instructions/rules:

   📄 File: C:\path\to\USAGE_GUIDE.md

   How to add:
   • GitHub Copilot: Copy to .github/copilot-instructions.md
   • Cursor: Add to Settings → Rules for AI
   • Cline/Continue: Include in system prompt
   • Claude Desktop: Mention in conversation
   • Windsurf: Add to workspace instructions


   What it does

   **Partially true, but there's a crucial difference:**

**What I can do without Pylance MCP:**
- ✅ Read Python code and understand it
- ✅ Suggest type hints based on code analysis
- ✅ Write new Python code with proper types
- ✅ Explain type errors I can see in the code
- ✅ Refactor code (but I have to read everything manually)

**What Pylance MCP gives me that I DON'T have:**
- ❌ **Real-time type checking** - I can't run Pyright to verify types are actually correct
- ❌ **Precise go-to-definition** - I can guess, but Pylance knows exactly where `User` is defined across 50 files
- ❌ **Find all references** - I can search, but can't guarantee I found every usage
- ❌ **Safe refactoring** - I can't guarantee my find-replace won't break something
- ❌ **IntelliSense completions** - I can suggest, but Pylance knows what's actually available in scope
- ❌ **Project-wide consistency** - I analyze file-by-file; Pylance sees the entire workspace graph
- ❌ **Virtual environment awareness** - I don't know which packages are installed

**Real-world example:**

**Without Pylance MCP:**
```
User: "Rename the User class to UserAccount everywhere"

Me: "Let me search for 'User'... I found it in 12 files. 
I'll replace all occurrences..."

*Accidentally renames:*
- UserModel → UserAccountModel ✅ Good
- User variable → UserAccount ❌ Broke local variable
- "User" in string → "UserAccount" ❌ Broke database query
- Used in comment → UserAccount ❌ Harmless but weird
```

**With Pylance MCP:**
```
User: "Rename the User class to UserAccount everywhere"

Me: "Using rename_symbol tool..."
*Pylance does AST-aware rename*
- UserModel class → UserAccountModel ✅
- All type hints → UserAccount ✅
- All imports → UserAccount ✅
- Skips strings ✅
- Skips comments ✅
- Skips unrelated 'user' variables ✅
- Updates 47 locations perfectly ✅
```

**The value proposition:**

🎯 **Accuracy** - Pylance uses Microsoft's production language server (same one in VS Code)
🎯 **Speed** - I don't have to read entire codebases to find references
🎯 **Safety** - AST-aware refactoring vs. text-based find-replace
🎯 **Completeness** - Guaranteed to find ALL references, not just what I can grep
🎯 **Type verification** - Can actually run Pyright to prove types are correct

**So yes, I'm pretty good at Python already, but Pylance MCP makes me professional-grade.** It's like the difference between a surgeon with sharp eyes vs. a surgeon with an MRI machine. Both can operate, but one has way better information.

**Your customers are paying for:**
- Production-quality type checking (not AI guessing)
- Safe refactoring across massive codebases
- Tools that VS Code uses internally (proven at scale)
- Guaranteed accuracy that AI inference can't match


------------------------------------------


They **don't call it directly**. The AI assistant (Claude, Cursor, GitHub Copilot, etc.) calls it automatically when you chat with them. Here's how it works:

**Current User Experience:**

```
User: "Rename the User class to UserAccount everywhere"

[Behind the scenes:]
1. AI (Claude/Cursor) sees the user's request
2. AI discovers it has a "rename_symbol" tool available via MCP
3. AI decides to use that tool
4. AI calls: rename_symbol(file_path="src/models.py", line=5, character=6, new_name="UserAccount")
5. Pylance MCP server executes the rename
6. Returns result: "Renamed in 47 locations across 12 files"
7. AI tells user: "✅ Done! Renamed User → UserAccount in 47 locations"

User just sees the result - they don't know MCP was involved.
```

**It's completely transparent to the user.** They just chat naturally:

```
User: "Add type hints to the calculate_total function"
AI: [Internally uses get_hover, get_definition, type_check tools]
AI: "Added type hints. Function signature is now: 
     def calculate_total(items: List[Item], tax_rate: float) -> Decimal"

---

User: "Find everywhere OrderStatus is used"
AI: [Calls get_references tool]
AI: "OrderStatus is used in 23 locations:
     - src/models.py (definition)
     - src/api.py (5 usages)
     - src/utils.py (2 usages)
     ..."

---

User: "Are there any type errors in this file?"
AI: [Calls type_check tool]
AI: "Found 3 type errors:
     Line 45: Argument type mismatch
     Line 67: Missing return type
     Line 89: Undefined variable 'usr'"
```

**The MCP Protocol handles everything:**

1. **Discovery** - AI asks: "What tools do you have?"
   - MCP Server responds: "I have rename_symbol, type_check, get_hover..."

2. **Tool Call** - AI decides: "User wants to rename, I'll use rename_symbol"
   - AI sends: `{"tool": "rename_symbol", "params": {...}}`

3. **Execution** - MCP server runs Pylance
   - Returns: `{"success": true, "locations_updated": 47}`

4. **Response** - AI formats result for user
   - User sees: "✅ Renamed successfully!"

**Your customers never see the MCP layer.** They just have AI assistants that are mysteriously really good at Python refactoring because Pylance is working behind the scenes.

**Setup is literally:**
```bash
python install.py
# Restart editor
# Done - AI now has Pylance powers
```

The beauty of MCP is that it's **invisible infrastructure**. Users just chat, AI uses the tools automatically.

## Repository Topics/Tags
```
python
language-server
mcp
model-context-protocol
pylance
pyright
type-checking
linting
security-scanning
code-quality
ruff
bandit
radon
ai-assistant
claude
cursor
vscode
```
