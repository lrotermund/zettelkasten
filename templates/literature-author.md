---
<%* if (tp.file.title.charAt(0) == "}") { tR += `title: "${tp.file.title.match(/}b_(.*)-/)[1].replace(/_/g, " ")}"` } %>
<%* if (tp.file.title.charAt(0) == "}") { tR += `author: "${tp.file.title.match(/-(.*)$/)[1].replace(/_/g, " ")}"`} %>
tags: literature/todo
---

<%* if (tp.file.title.charAt(0) == "}") { tR += "# " + tp.file.title.match(/}b_(.*)-/)[1].replace(/_/g, " ") } %>

<%* if (tp.file.title.charAt(0) == "}") { %>
## Author
<%* tR += `[[@${tp.file.title.match(/-(.*)$/)[1]}]]` } %>