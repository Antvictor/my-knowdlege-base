## 问题
- SpringBoot项目，接口使用RestController 或 ResponseBody 返回对象时，可以成功转为JSON字符串，且无乱码，但当接口返回String且是JSON字符串时，中文会乱码。 
  id:: 678f3c3a-af25-4f8f-b8f9-623c028b51b0
	-
- ## 原因
- **返回对象时**，Spring Boot 会自动将对象转为 JSON 格式，且会默认使用 UTF-8 编码，这就保证了中文字符的正确显示。
- **返回 `JSONString` 时**，由于 `JSON.toJSONString()` 返回的是一个字符串，而不是对象，Spring Boot 不会再对其进行处理，也就是没有设置 `Content-Type` 或 `CharacterEncoding`，因此可能会导致乱码。
-
- ## 解决方案
-
- [[Comments]]
  collapsed:: true
	- [[2025_01_24]]
	  collapsed:: true
		- ((678f3c3a-af25-4f8f-b8f9-623c028b51b0))
			- well, this is a comment
			- but， I can't find this