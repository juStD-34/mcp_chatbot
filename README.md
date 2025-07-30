# MCP Research Assistant Project

> **Model Context Protocol (MCP) Research Assistant** - Hệ thống chatbot tìm kiếm và phân tích tài liệu nghiên cứu khoa học sử dụng MCP

## 📋 Tổng quan

Dự án này được phát triển theo khóa học [MCP: Build Rich Context AI Apps with Anthropic](https://learn.deeplearning.ai/courses/mcp-build-rich-context-ai-apps-with-anthropic/lesson/fkbhh/introduction) của DeepLearning.AI. 

Project cung cấp **cái nhìn tổng quan và khái quát về Model Context Protocol (MCP)** - một giao thức mới cho phép AI models truy cập vào dữ liệu và công cụ bên ngoài một cách có cấu trúc. Thông qua việc xây dựng một hệ thống tìm kiếm và phân tích tài liệu nghiên cứu, project minh họa các khái niệm cốt lõi của MCP từ cơ bản đến nâng cao.

### 🎯 Mục tiêu chính

-  Cung cấp hiểu biết thực tế về MCP architecture và implementation
-  Triển khai từng bước các thành phần của MCP ecosystem
-  Xây dựng một ứng dụng hoàn chỉnh để minh họa khả năng của MCP

### ⚠️ Hạn chế hiện tại

**Đây là project demo, chưa phù hợp cho production vì các lý do sau:**

1. **Phụ thuộc vào hiểu biết kỹ thuật**: Người dùng cần biết rõ cấu trúc server để tương tác hiệu quả
2. **Logic đơn giản**: Chưa có khả năng hiểu ngữ cảnh và trả lời tự nhiên như ChatGPT
3. **Scope hạn chế**: Chỉ tập trung vào tìm kiếm papers từ arXiv
4. **Thiếu tính năng enterprise**: Không có authentication, caching, error handling mạnh mẽ

## 🗂️ Cấu trúc dự án

```
📁 Project Root/
├── 📁 L3/                     # Lesson 3: Basic Tool Integration
├── 📁 L4/                     # Lesson 4: MCP Server Development  
├── 📁 L5/                     # Lesson 5: MCP Client Development
├── 📁 L6/                     # Lesson 6: Multi-Server Integration
├── 📁 L7/                     # Lesson 7: Resources & Prompts
├── 📁 papers/                 # Cached research papers
└── 📄 uv.lock                # Dependencies lock file
```

## 📚 Nội dung từng bài học

### 🔧 **L3 - Basic Search Tool (Lesson 3)**
**Mục tiêu**: Xây dựng công cụ tìm kiếm cơ bản với Anthropic Claude

**Tính năng**:
- Tool `search_papers()`: Tìm kiếm papers trên arXiv theo chủ đề
- Tool `extract_info()`: Trích xuất thông tin chi tiết của paper
- Lưu trữ kết quả tìm kiếm dưới dạng JSON
- Tích hợp trực tiếp với Anthropic API

**Demo**:
```bash
cd L3/
uv run main.py
```

### 🖥️ **L4 - MCP Server Development (Lesson 4)**
**Mục tiêu**: Chuyển đổi tools thành MCP server hoàn chỉnh

**Tính năng**:
- MCP server sử dụng FastMCP framework
- Giao tiếp qua stdio transport
- Hai tools: `search_papers` và `extract_info`
- Testing với MCP Inspector

**Demo**:
```bash
cd L4/server/
npx @modelcontextprotocol/inspector uv run research_server.py
```

### 🤖 **L5 - MCP Client Development (Lesson 5)**
**Mục tiêu**: Xây dựng chatbot client kết nối với MCP server

**Tính năng**:
- Chatbot interface với Anthropic Claude
- Kết nối stdio với MCP server
- Tự động discover và sử dụng tools
- Interactive conversation loop

**Demo**:
```bash
cd L5/client/
uv run mcp_chatbot.py
```

### 🔗 **L6 - Multi-Server Integration (Lesson 6)**
**Mục tiêu**: Tích hợp nhiều MCP servers trong một client

**Servers được tích hợp**:
- **Research Server**: Tìm kiếm papers
- **Filesystem Server**: Đọc/ghi files
- **Fetch Server**: Tải nội dung từ URLs

**Tính năng nâng cao**:
- Tool discovery từ multiple servers
- Session management cho nhiều connections
- Workflow phức tạp (fetch → analyze → save)

**Demo**:
```bash
cd L6/client/
uv run mcp_chatbot.py
# Thử: "Fetch deeplearning.ai, find interesting terms, search papers, save results"
```

### 📊 **L7 - Resources & Prompts (Lesson 7)**
**Mục tiêu**: Bổ sung Resources và Prompts cho MCP server

**Tính năng mới**:
- **Resources**: 
  - `papers://folders` - Liệt kê topics có sẵn
  - `papers://{topic}` - Xem papers theo topic
- **Prompts**: 
  - `generate_search_prompt` - Template tìm kiếm có cấu trúc
- **Special Commands**:
  - `@folders` - Xem topics
  - `@{topic}` - Xem papers của topic
  - `/prompts` - Liệt kê prompts
  - `/prompt {name} {args}` - Thực thi prompt

**Demo**:
```bash
cd L7/client/
uv run mcp_chatbot.py
# Thử: "@folders", "@math", "/prompt generate_search_prompt topic=AI num_papers=3"
```

## 🚀 Cài đặt và chạy

### Yêu cầu hệ thống
- Python 3.12+
- Node.js 18+ (cho MCP Inspector)
- uv package manager

### Cài đặt
```bash
# Clone repository
git clone <repo-url>
cd mcp-research-assistant

# Cài đặt dependencies
uv sync

# Tạo file .env với ANTHROPIC_API_KEY
echo "ANTHROPIC_API_KEY=your_api_key_here" > .env
```

### Chạy các lesson
```bash
# Lesson 3 - Basic tools
cd L3 && uv run main.py

# Lesson 4 - Test MCP server
cd L4/server && npx @modelcontextprotocol/inspector uv run research_server.py

# Lesson 5-7 - Chatbot clients
cd L5/client && uv run mcp_chatbot.py
cd L6/client && uv run mcp_chatbot.py  
cd L7/client && uv run mcp_chatbot.py
```

## 🔮 Ý tưởng phát triển
**- Câu hỏi: làm cách nào để phát triển một chat bot có thể tự thực hiện gọi tool chứ không phải người dùng hướng dẫn, và chatbot có thể đọc dữ liệu từ db, gọi API , v.v.v ?**
