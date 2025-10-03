---
title : "Blog 1"
date :  2025-09-10 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
# Xây dựng trợ lý SRE đa tác tử với Amazon Bedrock AgentCore

*by Amit Arora và Dheeraj Oruganty vào ngày 26 SEP 2025 trong Amazon Bedrock, Amazon Bedrock Agents, Amazon Bedrock Knowledge Bases, Amazon Machine Learning, Artificial Intelligence, Generative AI, Technical How-to Permalink  Comments  Share*

Các kỹ sư độ tin cậy hệ thống (SRE) đang phải đối mặt với thách thức ngày càng phức tạp trong các hệ thống phân tán hiện đại. Trong các sự cố sản xuất, họ phải nhanh chóng tương quan dữ liệu từ nhiều nguồn—log, chỉ số (metrics), sự kiện Kubernetes và sổ tay vận hành (operational runbooks)—để xác định nguyên nhân gốc rễ và triển khai giải pháp. Các công cụ giám sát truyền thống cung cấp dữ liệu thô nhưng thiếu trí tuệ để tổng hợp thông tin trên các hệ thống đa dạng này, thường để SRE phải tự tay ghép nối câu chuyện đằng sau những lần hệ thống thất bại.

Với một giải pháp AI tạo sinh, SRE có thể đặt câu hỏi cho hạ tầng của mình bằng ngôn ngữ tự nhiên. Ví dụ, họ có thể hỏi “Tại sao các pod payment-service bị crash loop?” hoặc “Điều gì đang gây ra đột biến độ trễ API?” và nhận được những hiểu biết toàn diện, khả thi kết hợp trạng thái hạ tầng, phân tích log, chỉ số hiệu năng và các thủ tục khắc phục theo từng bước. Khả năng này biến phản hồi sự cố từ một quy trình thủ công, tốn thời gian thành một cuộc điều tra hợp tác, tiết kiệm thời gian.

Trong bài viết này, chúng tôi trình diễn cách xây dựng một trợ lý SRE đa tác tử sử dụng Amazon Bedrock AgentCore, LangGraph và Model Context Protocol (MCP). Hệ thống này triển khai các tác tử AI chuyên biệt cộng tác với nhau để cung cấp trí tuệ sâu và theo ngữ cảnh mà các nhóm SRE hiện đại cần cho phản hồi sự cố hiệu quả và quản lý hạ tầng. Chúng tôi hướng dẫn bạn qua toàn bộ triển khai, từ thiết lập môi trường demo đến triển khai trên Amazon Bedrock AgentCore Runtime cho mục đích sản xuất.

## Tổng quan giải pháp

Giải pháp này sử dụng một kiến trúc đa tác tử toàn diện giải quyết những thách thức của vận hành SRE hiện đại thông qua tự động hóa thông minh. Giải pháp bao gồm bốn tác tử AI chuyên biệt làm việc cùng nhau dưới một tác tử giám sát để cung cấp phân tích hạ tầng toàn diện và hỗ trợ phản hồi sự cố.

Các ví dụ trong bài viết này sử dụng dữ liệu được tạo tổng hợp từ môi trường demo của chúng tôi. Các máy chủ backend mô phỏng các cụm Kubernetes thực tế, log ứng dụng, chỉ số hiệu năng và sổ tay vận hành. Trong triển khai sản xuất, các máy chủ giả lập này sẽ được thay thế bằng các kết nối tới hệ thống hạ tầng thực của bạn, dịch vụ giám sát và kho tài liệu.

Kiến trúc thể hiện một số khả năng chính:

* **Truy vấn hạ tầng bằng ngôn ngữ tự nhiên** – Bạn có thể hỏi những câu hỏi phức tạp về hạ tầng bằng tiếng Anh thông thường và nhận phân tích chi tiết kết hợp dữ liệu từ nhiều nguồn.
* **Cộng tác đa tác tử** – Các tác tử chuyên trách Kubernetes, log, metrics và quy trình vận hành làm việc cùng nhau để cung cấp hiểu biết toàn diện.
* **Tổng hợp dữ liệu theo thời gian thực** – Tác tử truy cập dữ liệu hạ tầng trực tiếp qua các API chuẩn hóa và trình bày các phát hiện có tương quan.
* **Tự động thực thi runbook** – Tác tử truy xuất và hiển thị các thủ tục vận hành theo từng bước cho các kịch bản sự cố phổ biến.
* **Ghi nguồn (source attribution)** – Mỗi phát hiện đều đi kèm ghi nguồn rõ ràng để xác minh và kiểm toán.

Sơ đồ sau minh họa kiến trúc giải pháp.

*AWS AgentCore architecture showing SRE support agent workflow with API monitoring and authentication components*

Kiến trúc minh họa cách tác tử hỗ trợ SRE tích hợp liền mạch với các thành phần Amazon Bedrock AgentCore:

* **Giao diện khách hàng** – Nhận cảnh báo về thời gian phản hồi API suy giảm và trả về phản hồi tổng hợp từ tác tử.
* **Amazon Bedrock AgentCore Runtime** – Quản lý môi trường thực thi cho giải pháp SRE đa tác tử.
* **Tác tử hỗ trợ SRE** – Hệ thống cộng tác đa tác tử xử lý sự cố và điều phối phản hồi.
* **Amazon Bedrock AgentCore Gateway** – Định tuyến yêu cầu tới các công cụ chuyên biệt thông qua giao diện OpenAPI:

  * Kubernetes API để lấy sự kiện cụm
  * Logs API để phân tích mẫu log
  * Metrics API để phân tích xu hướng hiệu năng
  * Runbooks API để tìm kiếm quy trình vận hành
* **Amazon Bedrock AgentCore Memory** – Lưu trữ và truy xuất ngữ cảnh phiên và các tương tác trước đó để đảm bảo liên tục.
* **Amazon Bedrock AgentCore Identity** – Xử lý xác thực để truy cập công cụ bằng tích hợp Amazon Cognito.
* **Amazon Bedrock AgentCore Observability** – Thu thập và trực quan hóa trace của tác tử để giám sát và gỡ lỗi.
* **Amazon Bedrock LLMs** – Cung cấp trí tuệ cho tác tử thông qua các mô hình ngôn ngữ lớn (LLM) Claude của Anthropic.

Giải pháp đa tác tử sử dụng mẫu tác tử giám sát (supervisor-agent), trong đó một bộ điều phối trung tâm phối hợp năm tác tử chuyên biệt:

* **Tác tử giám sát** – Phân tích truy vấn đến và tạo kế hoạch điều tra, định tuyến công việc cho các chuyên gia phù hợp và tổng hợp kết quả thành báo cáo toàn diện.
* **Tác tử hạ tầng Kubernetes** – Xử lý điều phối container và vận hành cụm, điều tra lỗi pod, vấn đề triển khai, ràng buộc tài nguyên và sự kiện cụm.
* **Tác tử log ứng dụng** – Xử lý dữ liệu log để tìm thông tin liên quan, xác định mẫu và bất thường, và tương quan sự kiện giữa nhiều dịch vụ.
* **Tác tử chỉ số hiệu năng** – Giám sát chỉ số hệ thống và xác định vấn đề hiệu năng, cung cấp phân tích thời gian thực và xu hướng lịch sử.
* **Tác tử runbook vận hành** – Cung cấp truy cập tới thủ tục được ghi chép, hướng dẫn khắc phục sự cố và quy trình leo thang dựa trên tình huống hiện tại.

## Sử dụng các nguyên thủy (primitives) của Amazon Bedrock AgentCore

Giải pháp cho thấy sức mạnh của Amazon Bedrock AgentCore bằng cách sử dụng nhiều nguyên thủy lõi. Giải pháp hỗ trợ hai nhà cung cấp cho LLM của Anthropic. Amazon Bedrock hỗ trợ Claude 3.7 Sonnet của Anthropic cho triển khai tích hợp với AWS, và Anthropic API hỗ trợ Claude 4 Sonnet của Anthropic cho truy cập API trực tiếp.

Thành phần Amazon Bedrock AgentCore Gateway chuyển đổi các API backend của tác tử SRE (Kubernetes, log ứng dụng, chỉ số hiệu năng và runbook vận hành) thành các công cụ Model Context Protocol (MCP). Điều này cho phép các tác tử được xây dựng bằng một framework mã nguồn mở hỗ trợ MCP (như LangGraph trong bài viết này) truy cập liền mạch vào các API hạ tầng.

Bảo mật cho toàn bộ giải pháp được cung cấp bởi Amazon Bedrock AgentCore Identity. Nó hỗ trợ xác thực đầu vào (ingress) để kiểm soát truy cập an toàn cho các tác tử kết nối tới gateway, và xác thực đầu ra (egress) để quản lý xác thực với các máy chủ backend, cung cấp truy cập API an toàn mà không cần hardcode thông tin xác thực.

Môi trường thực thi không máy chủ để triển khai tác tử SRE trong sản xuất được cung cấp bởi Amazon Bedrock AgentCore Runtime. Nó tự động scale từ 0 để xử lý các cuộc điều tra sự cố đồng thời trong khi duy trì cô lập phiên hoàn toàn. Amazon Bedrock AgentCore Runtime hỗ trợ cả OAuth và AWS Identity and Access Management (IAM) cho xác thực tác tử. Ứng dụng gọi tác tử phải có quyền IAM và chính sách tin cậy phù hợp. Để biết thêm thông tin, hãy xem *Identity and access management for Amazon Bedrock AgentCore*.

Amazon Bedrock AgentCore Memory biến tác tử SRE từ một hệ thống không trạng thái thành một trợ lý học hỏi thông minh, cá nhân hóa các cuộc điều tra dựa trên sở thích người dùng và ngữ cảnh lịch sử. Thành phần bộ nhớ cung cấp ba chiến lược riêng biệt:

* **Chiến lược sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Lưu trữ sở thích cá nhân về phong cách điều tra, kênh giao tiếp, quy trình leo thang và định dạng báo cáo. Ví dụ, Alice (một SRE kỹ thuật) nhận phân tích hệ thống chi tiết với các bước khắc phục, trong khi Carol (một lãnh đạo) nhận tóm tắt tập trung vào tác động kinh doanh.
* **Chiến lược tri thức hạ tầng** (`/sre/infrastructure/{user_id}/{session_id}`) – Tích lũy kiến thức miền qua các cuộc điều tra, cho phép tác tử học từ những phát hiện trước. Khi tác tử Kubernetes xác định một mẫu rò rỉ bộ nhớ, tri thức này trở nên sẵn có cho các cuộc điều tra tương lai, cho phép xác định nguyên nhân gốc nhanh hơn.
* **Chiến lược bộ nhớ điều tra** (`/sre/investigations/{user_id}/{session_id}`) – Duy trì ngữ cảnh lịch sử các sự cố và phương án giải quyết. Điều này cho phép giải pháp đề xuất các cách khắc phục đã được chứng minh và tránh các phản mẫu từng thất bại.

Thành phần bộ nhớ chứng minh giá trị của mình thông qua các cuộc điều tra được cá nhân hóa. Khi cả Alice và Carol điều tra “Thời gian phản hồi API đã giảm chất lượng gấp 3 lần trong giờ qua,” họ nhận các phát hiện kỹ thuật giống hệt nhau nhưng cách trình bày hoàn toàn khác.

**Alice** nhận phân tích kỹ thuật:

```
memory_client.retrieve_user_preferences(user_id="Alice")
# Returns: {"investigation_style": "detailed_systematic_analysis", "reports": "technical_exposition_with_troubleshooting_steps"}
```

**Carol** nhận tóm tắt điều hành:

```
memory_client.retrieve_user_preferences(user_id="Carol")
# Returns: {"investigation_style": "business_impact_focused","reports": "executive_summary_without_technical_details"}
```

## Bổ sung khả năng quan sát (observability) cho tác tử SRE

Việc bổ sung khả năng quan sát cho một tác tử SRE triển khai trên Amazon Bedrock AgentCore Runtime rất đơn giản khi sử dụng nguyên thủy Amazon Bedrock AgentCore Observability. Điều này cho phép giám sát toàn diện qua Amazon CloudWatch với metrics, trace và log. Thiết lập observability cần ba bước:

1. **Thêm các gói OpenTelemetry** vào `pyproject.toml`:

   ```toml
   dependencies = [
       # ... other dependencies ...
       "opentelemetry-instrumentation-langchain",
       "aws-opentelemetry-distro~=0.10.1",
   	]
   ```
2. **Cấu hình observability** cho các tác tử của bạn để bật metrics trong CloudWatch.
3. **Khởi động container** bằng tiện ích `opentelemetry-instrument` để tự động instrument ứng dụng của bạn.

Lệnh sau được thêm vào Dockerfile cho tác tử SRE:

```dockerfile
# Run application with OpenTelemetry instrumentation
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Như minh họa trong ảnh chụp màn hình sau, khi bật observability, bạn có khả năng quan sát những điều sau:

* **Chỉ số gọi LLM** – Sử dụng token, độ trễ và hiệu năng mô hình trên các tác tử.
* **Trace thực thi công cụ** – Thời lượng và tỷ lệ thành công cho mỗi lần gọi công cụ MCP.
* **Hoạt động bộ nhớ** – Mẫu truy xuất và hiệu quả lưu trữ.
* **Theo dõi yêu cầu đầu-cuối** – Luồng yêu cầu hoàn chỉnh từ truy vấn người dùng đến phản hồi cuối cùng.

*AWS CloudWatch observability dashboard for SRE agent showing session metrics, trace counts, and FM token usage trends*

Nguyên thủy observability tự động thu thập các chỉ số này mà không cần thay đổi mã bổ sung, cung cấp khả năng giám sát cấp sản xuất ngay từ đầu.

## Luồng từ phát triển đến sản xuất

Tác tử SRE tuân theo quy trình triển khai có cấu trúc bốn bước từ phát triển cục bộ đến sản xuất, với các thủ tục chi tiết được ghi lại trong *Development to Production Flow* trong kho GitHub đi kèm:

*The four-step structured deployment process*

Quy trình triển khai duy trì tính nhất quán giữa các môi trường: mã cốt lõi của tác tử (`sre_agent/`) không thay đổi, và thư mục `deployment/` chứa các tiện ích dành riêng cho triển khai. Cùng một tác tử hoạt động cục bộ và trong sản xuất thông qua cấu hình môi trường, với Amazon Bedrock AgentCore Gateway cung cấp truy cập công cụ MCP xuyên suốt các giai đoạn phát triển và triển khai.

## Hướng dẫn triển khai (walkthrough)

Trong phần sau, chúng tôi tập trung vào cách Amazon Bedrock AgentCore Gateway, Memory và Runtime phối hợp để xây dựng giải pháp cộng tác đa tác tử này và triển khai end-to-end với hỗ trợ MCP và trí tuệ bền vững.

Chúng tôi bắt đầu bằng việc thiết lập kho mã và thiết lập môi trường runtime cục bộ với khóa API, nhà cung cấp LLM và hạ tầng demo. Sau đó, chúng tôi kích hoạt các thành phần AgentCore cốt lõi bằng cách tạo gateway cho truy cập API chuẩn hóa, cấu hình xác thực và thiết lập kết nối công cụ. Chúng tôi bổ sung trí tuệ thông qua AgentCore Memory, tạo chiến lược cho sở thích người dùng và lịch sử điều tra đồng thời nạp persona để phản hồi sự cố được cá nhân hóa. Cuối cùng, chúng tôi cấu hình các tác tử riêng lẻ với công cụ chuyên biệt, tích hợp khả năng bộ nhớ, điều phối luồng làm việc cộng tác và triển khai lên AgentCore Runtime với khả năng quan sát đầy đủ.

Các hướng dẫn chi tiết cho từng bước có trong kho:

* **Use Case Setup Guide** – Triển khai backend và thiết lập môi trường phát triển.
* **Deployment Guide** – Đóng gói cho sản xuất và triển khai Amazon Bedrock AgentCore Runtime.

## Điều kiện tiên quyết (Prerequisites)

Bạn có thể tìm yêu cầu chuyển tiếp cổng (port forwarding) và các hướng dẫn thiết lập khác trong phần **Prerequisites** của tệp README.

## Chuyển đổi API thành công cụ MCP với Amazon Bedrock AgentCore Gateway

Amazon Bedrock AgentCore Gateway minh chứng sức mạnh của chuẩn hóa giao thức bằng cách chuyển đổi các API backend hiện có thành công cụ MCP mà các framework tác tử có thể tiêu thụ. Sự chuyển đổi này diễn ra liền mạch, chỉ cần các đặc tả OpenAPI.

### Tải lên đặc tả OpenAPI

Quy trình gateway bắt đầu bằng việc tải đặc tả API hiện có của bạn lên Amazon Simple Storage Service (Amazon S3). Script `create_gateway.sh` tự động xử lý việc tải bốn đặc tả API (Kubernetes, Logs, Metrics và Runbooks) lên bucket S3 đã cấu hình với siêu dữ liệu và loại nội dung phù hợp. Các đặc tả này sẽ được dùng để tạo endpoint mục tiêu API trong gateway.

### Tạo nhà cung cấp danh tính và gateway

Xác thực được xử lý liền mạch thông qua Amazon Bedrock AgentCore Identity. Script `main.py` tạo cả nhà cung cấp thông tin xác thực và gateway:

```python
# Create AgentCore Gateway with JWT authorization
def create_gateway(
    client: Any,
    gateway_name: str,
    role_arn: str,
    discovery_url: str,
    allowed_clients: list = None,
    description: str = "AgentCore Gateway created via SDK",
    search_type: str = "SEMANTIC",
    protocol_version: str = "2025-03-26",
) -> Dict[str, Any]:
    
    # Build auth config for Cognito
    auth_config = {"customJWTAuthorizer": {"discoveryUrl": discovery_url}}
    if allowed_clients:
        auth_config["customJWTAuthorizer"]["allowedClients"] = allowed_clients
    
    protocol_configuration = {
        "mcp": {"searchType": search_type, "supportedVersions": [protocol_version]}
    }

    response = client.create_gateway(
        name=gateway_name,
        roleArn=role_arn,
        protocolType="MCP",
        authorizerType="CUSTOM_JWT",
        authorizerConfiguration=auth_config,
        protocolConfiguration=protocol_configuration,
        description=description,
        exceptionLevel='DEBUG'
    )
    return response
```

### Triển khai mục tiêu endpoint API với nhà cung cấp thông tin xác thực

Mỗi API trở thành một mục tiêu MCP thông qua gateway. Giải pháp tự động xử lý quản lý thông tin xác thực:

```python
def create_api_endpoint_target(
    client: Any,
    gateway_id: str,
    s3_uri: str,
    provider_arn: str,
    target_name_prefix: str = "open",
    description: str = "API Endpoint Target for OpenAPI schema",
) -> Dict[str, Any]:
    
    api_target_config = {"mcp": {"openApiSchema": {"s3": {"uri": s3_uri}}}}

    # API key credential provider configuration
    credential_config = {
        "credentialProviderType": "API_KEY",
        "credentialProvider": {
            "apiKeyCredentialProvider": {
                "providerArn": provider_arn,
                "credentialLocation": "HEADER",
                "credentialParameterName": "X-API-KEY",
            }
        },
    }
    
    response = client.create_gateway_target(
        gatewayIdentifier=gateway_id,
        name=target_name_prefix,
        description=description,
        targetConfiguration=api_target_config,
        credentialProviderConfigurations=[credential_config],
    )
    return response
```

### Xác nhận công cụ MCP đã sẵn sàng cho framework tác tử

Sau triển khai, Amazon Bedrock AgentCore Gateway cung cấp endpoint chuẩn hóa `/mcp` được bảo vệ bằng token JWT. Kiểm thử triển khai với `mcp_cmds.sh` cho thấy sức mạnh của sự chuyển đổi này:

```
Tool summary:
================
Total tools found: 21

Tool names:
• x_amz_bedrock_agentcore_search
• k8s-api___get_cluster_events
• k8s-api___get_deployment_status
• k8s-api___get_node_status
• k8s-api___get_pod_status
• k8s-api___get_resource_usage
• logs-api___analyze_log_patterns
• logs-api___count_log_events
• logs-api___get_error_logs
• logs-api___get_recent_logs
• logs-api___search_logs
• metrics-api___analyze_trends
• metrics-api___get_availability_metrics
• metrics-api___get_error_rates
• metrics-api___get_performance_metrics
• metrics-api___get_resource_metrics
• runbooks-api___get_common_resolutions
• runbooks-api___get_escalation_procedures
• runbooks-api___get_incident_playbook
• runbooks-api___get_troubleshooting_guide
• runbooks-api___search_runbooks
```

## Tương thích rộng với các framework tác tử

Gateway tiêu chuẩn hóa MCP này hiện có thể được cấu hình như một máy chủ Streamable-HTTP cho các client MCP, bao gồm **AWS Strands**, framework phát triển tác tử của Amazon, **LangGraph** (framework được dùng trong triển khai tác tử SRE của chúng tôi) và **CrewAI**, một framework cộng tác đa tác tử.

Ưu điểm của cách tiếp cận này là các API hiện có không cần sửa đổi—chỉ cần đặc tả OpenAPI. Amazon Bedrock AgentCore Gateway xử lý:

* **Dịch giao thức** – Từ REST APIs sang MCP.
* **Xác thực** – Xác thực token JWT và chèn thông tin xác thực.
* **Bảo mật** – Kết thúc TLS và kiểm soát truy cập.
* **Chuẩn hóa** – Đặt tên công cụ và xử lý tham số nhất quán.

Điều này có nghĩa là bạn có thể lấy các API hạ tầng hiện có (Kubernetes, giám sát, ghi log, tài liệu) và ngay lập tức cung cấp chúng cho các framework tác tử hỗ trợ MCP—thông qua một giao diện duy nhất, an toàn và chuẩn hóa.

## Triển khai trí tuệ bền vững với Amazon Bedrock AgentCore Memory

Trong khi Amazon Bedrock AgentCore Gateway cung cấp truy cập API liền mạch, Amazon Bedrock AgentCore Memory biến tác tử SRE từ một hệ thống không trạng thái thành một trợ lý biết học hỏi. Việc triển khai bộ nhớ cho thấy chỉ với vài dòng mã là có thể bật cá nhân hóa tinh vi và lưu giữ tri thức xuyên suốt các phiên.

### Khởi tạo các chiến lược bộ nhớ

Thành phần bộ nhớ của tác tử SRE được xây dựng trên mô hình dựa trên sự kiện của Amazon Bedrock AgentCore Memory với định tuyến không gian tên (namespace) tự động. Trong quá trình khởi tạo, giải pháp tạo ba chiến lược bộ nhớ với mẫu không gian tên cụ thể:

```python
from sre_agent.memory.client import SREMemoryClient
from sre_agent.memory.strategies import create_memory_strategies

# Initialize memory client
memory_client = SREMemoryClient(
    memory_name="sre_agent_memory",
    region="us-east-1"
)

# Create three specialized memory strategies
strategies = create_memory_strategies()
for strategy in strategies:
    memory_client.create_strategy(strategy)
```

Ba chiến lược phục vụ các mục đích riêng:

* **Sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Phong cách điều tra cá nhân và sở thích giao tiếp.
* **Tri thức hạ tầng**: `/sre/infrastructure/{user_id}/{session_id}` – Kiến thức miền tích lũy qua các cuộc điều tra.
* **Tóm tắt điều tra**: `/sre/investigations/{user_id}/{session_id}` – Mẫu sự cố lịch sử và phương án giải quyết.

### Nạp persona và sở thích người dùng

Giải pháp đi kèm sẵn các persona minh họa điều tra được cá nhân hóa. Script `manage_memories.py` nạp các persona này:

```python
# Load Alice - Technical SRE Engineer
alice_preferences = {
    "investigation_style": "detailed_systematic_analysis",
    "communication": ["#alice-alerts", "#sre-team"],
    "escalation": {"contact": "alice.manager@company.com", "threshold": "15min"},
    "reports": "technical_exposition_with_troubleshooting_steps",
    "timezone": "UTC"
}

# Load Carol - Executive/Director
carol_preferences = {
    "investigation_style": "business_impact_focused",
    "communication": ["#carol-executive", "#strategic-alerts"],
    "escalation": {"contact": "carol.director@company.com", "threshold": "5min"},
    "reports": "executive_summary_without_technical_details",
    "timezone": "EST"
}

# Store preferences using memory client
memory_client.store_user_preference("Alice", alice_preferences)
memory_client.store_user_preference("Carol", carol_preferences)
```

### Định tuyến không gian tên tự động trong thực tế

Sức mạnh của Amazon Bedrock AgentCore Memory nằm ở định tuyến không gian tên tự động. Khi tác tử SRE tạo sự kiện, nó chỉ cần cung cấp `actor_id`—Amazon Bedrock AgentCore Memory sẽ tự động xác định sự kiện thuộc về các không gian tên nào:

```python
# During investigation, the supervisor agent stores context
memory_client.create_event(
    memory_id="sre_agent_memory-abc123",
    actor_id="Alice",  # AgentCore Memory routes this automatically
    session_id="investigation_2025_01_15",
    messages=[("investigation_started", "USER")]
)

# Memory system automatically:
# 1. Checks strategy namespaces <!-- "all" is necessary here for technical accuracy -->
# 2. Matches actor_id "Alice" to /sre/users/Alice/preferences
# 3. Stores event in User Preferences Strategy
# 4. Makes event available for future retrievals
```

### Xác thực trải nghiệm điều tra được cá nhân hóa

Tác động của thành phần bộ nhớ trở nên rõ ràng khi cả Alice và Carol điều tra cùng một vấn đề. Sử dụng các phát hiện kỹ thuật giống hệt nhau, giải pháp tạo ra các cách trình bày hoàn toàn khác nhau của cùng một nội dung.

Báo cáo kỹ thuật của **Alice** chứa phân tích hệ thống chi tiết cho đội kỹ thuật:

**Technical Investigation Summary**

**Root Cause:** Rò rỉ bộ nhớ ở bộ xử lý thanh toán gây ra OOM kills

**Analysis:**

* Tần suất khởi động lại pod tăng 300% lúc 14:23 UTC
* Mức sử dụng bộ nhớ đạt đỉnh 8.2GB (80% giới hạn của container)
* Độ trễ thu gom rác JVM tăng vọt lên 2.3s

**Next Step:**

1. Thực hiện phân tích heap dump (`kubectl exec payment-pod -- jmap`)
2. Rà soát các lần triển khai mã gần đây liên quan quản lý bộ nhớ
3. Cân nhắc tăng giới hạn bộ nhớ và triển khai tắt an toàn (graceful shutdown)

Tóm tắt điều hành của **Carol** tập trung vào tác động kinh doanh cho các bên liên quan cấp điều hành:

**Business Impact Assessment**
**Status:** CRITICAL - Khả năng xử lý thanh toán của khách hàng suy giảm
**Impact:** Tỷ lệ giao dịch thất bại 23%, rủi ro doanh thu 47 nghìn USD
**Timeline:** Phát hiện vấn đề lúc 14:23 UTC, ước tính khắc phục 45 phút
**Business Actions:** - Đã khởi động kênh giao tiếp khách hàng qua trang trạng thái - Đội tài chính được báo động để theo dõi tác động doanh thu - Leo thang tới VP Engineering nếu chưa giải quyết trước 15:15 UTC

Thành phần bộ nhớ cho phép cá nhân hóa đồng thời liên tục học hỏi từ mỗi cuộc điều tra, xây dựng tri thức tổ chức giúp cải thiện phản hồi sự cố theo thời gian.

## Triển khai sản xuất với Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore giúp triển khai các tác tử hiện có lên sản xuất trở nên đơn giản. Quy trình gồm ba bước chính: đóng gói container cho tác tử, triển khai lên Amazon Bedrock AgentCore Runtime và gọi (invoke) tác tử đã triển khai.

### Đóng gói container cho tác tử

Amazon Bedrock AgentCore Runtime yêu cầu container ARM64. Mã sau cho thấy Dockerfile đầy đủ:

```dockerfile
# Use uv's ARM64 Python base image
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:python3.12-bookworm-slim

WORKDIR /app

# Copy uv files
COPY pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync --frozen --no-dev

# Copy SRE agent module
COPY sre_agent/ ./sre_agent/

# Set environment variables
# Note: Set DEBUG=true to enable debug logging and traces
ENV PYTHONPATH="/app" \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Expose port
EXPOSE 8080

# Run application with OpenTelemetry instrumentation
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Các tác tử hiện có chỉ cần một lớp bọc FastAPI (`agent_runtime:app`) để tương thích với Amazon Bedrock AgentCore, và chúng tôi thêm `opentelemetry-instrument` để bật observability thông qua Amazon Bedrock AgentCore.

### Triển khai lên Amazon Bedrock AgentCore Runtime

Triển khai lên Amazon Bedrock AgentCore Runtime rất đơn giản với script `deploy_agent_runtime.py`:

```python
import boto3

# Create AgentCore client
client = boto3.client('bedrock-agentcore', region_name=region)

# Environment variables for your agent
env_vars = {
    'GATEWAY_ACCESS_TOKEN': gateway_access_token,
    'LLM_PROVIDER': llm_provider,
    'ANTHROPIC_API_KEY': anthropic_api_key  # if using Anthropic
}

# Deploy container to AgentCore Runtime
response = client.create_agent_runtime(
    agentRuntimeName=runtime_name,
    agentRuntimeArtifact={
        'containerConfiguration': {
            'containerUri': container_uri  # Your ECR container URI
        }
    },
    networkConfiguration={"networkMode": "PUBLIC"},
    roleArn=role_arn,
    environmentVariables=env_vars
)

print(f"Agent Runtime ARN: {response['agentRuntimeArn']}")
```

Amazon Bedrock AgentCore tự động xử lý hạ tầng, khả năng mở rộng và quản lý phiên.

### Gọi tác tử đã triển khai

Gọi tác tử đã triển khai cũng đơn giản như với `invoke_agent_runtime.py`:

```python
# Prepare your query with user_id and session_id for memory personalization
payload = json.dumps({
    "input": {
        "prompt": "API response times have degraded 3x in the last hour",
        "user_id": "Alice",  # User for personalized investigation
        "session_id": "investigation-20250127-123456"  # Session for context
    }
})

# Invoke the deployed agent
response = agent_core_client.invoke_agent_runtime(
    agentRuntimeArn=runtime_arn,
    runtimeSessionId=session_id,
    payload=payload,
    qualifier="DEFAULT"
)

# Get the response
response_data = json.loads(response['response'].read())
print(response_data)  # Full response includes output with agent's investigation
```

### Lợi ích chính của Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore Runtime cung cấp các lợi ích chính sau:

* **Không quản lý hạ tầng** – Không cần máy chủ, cân bằng tải hay cấu hình scale.
* **Cô lập phiên tích hợp** – Mỗi cuộc hội thoại hoàn toàn tách biệt.
* **Tích hợp AWS IAM** – Kiểm soát truy cập an toàn mà không cần xác thực tùy biến.
* **Tự động mở rộng** – Scale từ 0 tới hàng nghìn phiên đồng thời.

Quy trình triển khai đầy đủ, bao gồm xây dựng container và xử lý quyền AWS, được ghi lại trong **Deployment Guide**.

## Trường hợp sử dụng thực tế

Hãy khám phá cách tác tử SRE xử lý các kịch bản phản hồi sự cố phổ biến qua một cuộc điều tra thực.

Khi đối mặt với vấn đề sản xuất, bạn có thể truy vấn hệ thống bằng ngôn ngữ tự nhiên. Giải pháp sử dụng Amazon Bedrock AgentCore Memory để cá nhân hóa cuộc điều tra dựa trên vai trò và sở thích của bạn:

```bash
export USER_ID=Alice
sre-agent --prompt "API response times have degraded 3x in the last hour"
```

Tác tử giám sát truy xuất sở thích của Alice từ bộ nhớ (phong cách phân tích hệ thống chi tiết) và tạo một kế hoạch điều tra phù hợp với vai trò SRE Kỹ thuật của cô:

**Investigation Plan**

1. Sử dụng `metrics_agent` để phân tích chỉ số hiệu năng API gồm thời gian phản hồi, tỷ lệ lỗi và sử dụng tài nguyên nhằm xác định mức độ và mô hình chậm lại.
2. Sử dụng `logs_agent` để kiểm tra log ứng dụng về các lỗi liên quan đến phản hồi API chậm, tập trung vào vấn đề kết nối cơ sở dữ liệu và lỗi bộ nhớ.
3. Sử dụng `kubernetes_agent` để kiểm tra trạng thái pod và ràng buộc tài nguyên, đặc biệt cho các pod web-service và database, tìm trạng thái CrashLoopBackOff và thiếu ConfigMap.

**Độ phức tạp:** Đơn giản
**Tự động thực thi:** Có
**Tác tử tham gia:** Metrics Agent, Logs Agent, Kubernetes Agent

Các tác tử điều tra tuần tự theo kế hoạch, mỗi bên đóng góp phân tích chuyên sâu của mình. Giải pháp sau đó tổng hợp những phát hiện này thành **executive summary** toàn diện:

**Executive Summary**
**Key Insights**

* **Nguyên nhân gốc:** Dịch vụ cơ sở dữ liệu lỗi do thiếu ConfigMap `database-config` trong namespace production, khiến pod cơ sở dữ liệu liên tục crash.
* **Tác động:** Suy giảm hiệu năng nghiêm trọng với thời gian phản hồi API tăng từ 150ms lên 5000ms (chậm hơn 33x).
* **Mức độ nghiêm trọng:** Cao – Cơ sở dữ liệu không sẵn sàng, cạn kiệt bộ nhớ (100%) và bão hòa CPU (95%) gây tỷ lệ lỗi 75%.

**Next Steps**

1. **Ngay lập tức (< 1 giờ):** Tạo/cập nhật ConfigMap `database-config` trong namespace production và khởi động lại pod cơ sở dữ liệu.
2. **Ngắn hạn (< 24 giờ):**

   * Sửa quyền trên thư mục `/var/lib/postgresql/data`
   * Tăng heap Java cho web-service để xử lý `OutOfMemoryErrors`
   * Tối ưu phương thức `UserService.loadAllUsers` gây vấn đề bộ nhớ
3. **Dài hạn (< 1 tuần):**

   * Triển khai giám sát tài nguyên với cảnh báo cho CPU (>80%), bộ nhớ (>90%)
   * Tối ưu truy vấn DB chậm, đặc biệt `"SELECT * FROM users WHERE status='active'"`
   * Scale tài nguyên hoặc triển khai autoscaling cho web-service

**Cảnh báo quan trọng**

* Pod cơ sở dữ liệu (`database-pod-7b9c4d8f2a-x5m1q`) ở trạng thái CrashLoopBackOff
* Web-service gặp `OutOfMemoryErrors` tại `UserService.loadAllUsers(UserService.java:45)`
* Node-3 chịu áp lực bộ nhớ (>85% sử dụng)
* `web-app-deployment` gặp lỗi kiểm tra sẵn sàng (readiness probe) với lỗi 503

**Bước khắc phục sự cố**

1. Xác minh trạng thái ConfigMap: `kubectl get configmap database-config -n production`
2. Kiểm tra log pod cơ sở dữ liệu: `kubectl logs database-pod-7b9c4d8f2a-x5m1q -n production`
3. Tạo/cập nhật ConfigMap: `kubectl create configmap database-config --from-file=database.conf -n production`
4. Sửa quyền thư mục dữ liệu: `kubectl exec database-pod-7b9c4d8f2a-x5m1q -n production -- chmod -R 700 /var/lib/postgresql/data`
5. Khởi động lại pod cơ sở dữ liệu: `kubectl delete pod database-pod-7b9c4d8f2a-x5m1q -n production`

Cuộc điều tra này cho thấy các nguyên thủy của Amazon Bedrock AgentCore phối hợp:

* **Amazon Bedrock AgentCore Gateway** – Cung cấp truy cập an toàn tới API hạ tầng thông qua công cụ MCP.
* **Amazon Bedrock AgentCore Identity** – Xử lý xác thực đầu vào và đầu ra.
* **Amazon Bedrock AgentCore Runtime** – Lưu trữ giải pháp đa tác tử với khả năng mở rộng tự động.
* **Amazon Bedrock AgentCore Memory** – Cá nhân hóa trải nghiệm của Alice và lưu tri thức điều tra cho tương lai.
* **Amazon Bedrock AgentCore Observability** – Thu thập metrics và trace chi tiết trong CloudWatch để giám sát và gỡ lỗi.

Tác tử SRE thể hiện điều phối tác tử thông minh, với tác tử giám sát định tuyến công việc tới chuyên gia dựa trên kế hoạch điều tra. Khả năng bộ nhớ của giải pháp đảm bảo mỗi cuộc điều tra đều xây dựng tri thức tổ chức và cung cấp trải nghiệm được cá nhân hóa dựa trên vai trò và sở thích.

Cuộc điều tra này thể hiện một số khả năng chính:

* **Tương quan đa nguồn** – Kết nối vấn đề cấu hình cơ sở dữ liệu với suy giảm hiệu năng API.
* **Điều tra tuần tự** – Tác tử làm việc có hệ thống theo kế hoạch, cung cấp cập nhật trực tiếp.
* **Ghi nguồn** – Phát hiện bao gồm công cụ và nguồn dữ liệu cụ thể.
* **Thông tin khả thi** – Cung cấp timeline rõ ràng và các bước khôi phục được ưu tiên.
* **Phát hiện hỏng hóc dây chuyền** – Có thể cho thấy cách một lỗi lan truyền qua hệ thống.

## Tác động kinh doanh

Các tổ chức triển khai hỗ trợ SRE bằng AI báo cáo những cải thiện đáng kể ở các chỉ số vận hành then chốt. Những cuộc điều tra ban đầu vốn mất 30–45 phút nay có thể hoàn tất trong 5–10 phút, cung cấp cho SRE bối cảnh toàn diện trước khi đi sâu phân tích chi tiết. Sự cắt giảm mạnh thời gian điều tra này chuyển trực tiếp thành thời gian khắc phục sự cố nhanh hơn và giảm thời gian ngừng dịch vụ. Giải pháp cải thiện cách SRE tương tác với hạ tầng: thay vì điều hướng nhiều dashboard và công cụ, kỹ sư có thể đặt câu hỏi bằng ngôn ngữ tự nhiên và nhận insight tổng hợp từ các nguồn dữ liệu liên quan. Việc giảm chuyển đổi ngữ cảnh này giúp đội ngũ duy trì tập trung trong các sự cố quan trọng và giảm gánh nặng nhận thức khi điều tra. Quan trọng hơn, giải pháp dân chủ hóa tri thức trong đội. Tất cả thành viên có thể tiếp cận cùng kỹ thuật điều tra toàn diện, giảm phụ thuộc vào tri thức truyền miệng và gánh nặng trực ca. Phương pháp luận nhất quán mà giải pháp cung cấp đảm bảo cách tiếp cận điều tra đồng đều giữa các thành viên và loại sự cố, nâng cao độ tin cậy tổng thể và giảm khả năng bỏ sót bằng chứng.

Các báo cáo điều tra được tạo tự động cung cấp tài liệu có giá trị cho các buổi rà soát sau sự cố và giúp đội ngũ học hỏi từ mỗi sự cố, xây dựng tri thức tổ chức theo thời gian. Hơn nữa, giải pháp mở rộng các khoản đầu tư hạ tầng AWS hiện có, hoạt động song song với các dịch vụ như Amazon CloudWatch, AWS Systems Manager và các công cụ vận hành AWS khác để cung cấp một hệ thống trí tuệ vận hành hợp nhất.

## Mở rộng giải pháp

Kiến trúc mô-đun giúp việc mở rộng giải pháp theo nhu cầu cụ thể của bạn trở nên đơn giản.

Ví dụ, bạn có thể thêm các tác tử chuyên biệt cho miền của mình:

* **Tác tử bảo mật** – Cho kiểm tra tuân thủ và phản hồi sự cố bảo mật.
* **Tác tử cơ sở dữ liệu** – Cho khắc phục sự cố và tối ưu hóa đặc thù DB.
* **Tác tử mạng** – Cho gỡ lỗi kết nối và hạ tầng.

Bạn cũng có thể thay thế các API demo bằng kết nối tới hệ thống thực của bạn:

* **Tích hợp Kubernetes** – Kết nối tới API cụm của bạn để lấy trạng thái pod, triển khai và sự kiện.
* **Tập trung log** – Tích hợp với dịch vụ quản lý log của bạn (Elasticsearch, Splunk, CloudWatch Logs).
* **Nền tảng chỉ số** – Kết nối với dịch vụ giám sát của bạn (Prometheus, Datadog, CloudWatch Metrics).
* **Kho runbook** – Liên kết tới tài liệu vận hành và playbook của bạn lưu trên wiki, Git hoặc knowledge base.

## Dọn dẹp

Để tránh phát sinh chi phí về sau, hãy dùng script dọn dẹp để xóa các tài nguyên AWS tính phí được tạo trong demo:

```bash
# Complete cleanup - deletes AWS resources and local files
./scripts/cleanup.sh
```

Script này tự động thực hiện:

* Dừng các máy chủ backend
* Xóa gateway và các mục tiêu của nó
* Xóa tài nguyên Amazon Bedrock AgentCore Memory
* Xóa Amazon Bedrock AgentCore Runtime
* Xóa các tệp đã tạo (gateway URI, token, agent ARN, memory ID)

Để biết hướng dẫn dọn dẹp chi tiết, tham khảo **Cleanup Instructions**.

## Kết luận

Tác tử SRE cho thấy cách các hệ thống đa tác tử có thể biến phản hồi sự cố từ một quy trình thủ công, tốn thời gian thành một cuộc điều tra hợp tác, tiết kiệm thời gian, cung cấp cho SRE những insight cần thiết để giải quyết vấn đề nhanh chóng và tự tin.

Bằng cách kết hợp hạ tầng cấp doanh nghiệp của Amazon Bedrock AgentCore với truy cập công cụ chuẩn hóa trong MCP, chúng tôi đã tạo nên nền tảng có thể thích ứng khi hạ tầng của bạn phát triển và các khả năng mới xuất hiện.

Toàn bộ triển khai có trong kho GitHub của chúng tôi, bao gồm môi trường demo, hướng dẫn cấu hình và ví dụ mở rộng. Chúng tôi khuyến khích bạn khám phá giải pháp, tùy biến cho hạ tầng của bạn và chia sẻ trải nghiệm với cộng đồng.

Để bắt đầu xây dựng trợ lý SRE của riêng bạn, hãy tham khảo các tài nguyên sau:

* **Automate tasks in your application using AI agents**
* **Amazon Bedrock AgentCore Samples GitHub repository**
* **Model Context Protocol documentation**
* **LangGraph documentation**

## Về tác giả

**Amit Arora** là Kiến trúc sư Chuyên gia AI và ML tại Amazon Web Services, hỗ trợ khách hàng doanh nghiệp sử dụng các dịch vụ máy học trên đám mây để mở rộng nhanh chóng các đổi mới của họ. Ông cũng là giảng viên kiêm nhiệm trong chương trình thạc sĩ khoa học dữ liệu và phân tích tại Đại học Georgetown ở Washington, D.C.

**Dheeraj Oruganty** là Chuyên viên Triển khai tại Amazon Web Services. Anh đam mê xây dựng các giải pháp AI tạo sinh và Máy học sáng tạo mang lại tác động kinh doanh thực sự. Chuyên môn của anh bao gồm Đánh giá AI Tác tử, Benchmarking và Điều phối Tác tử, nơi anh tích cực đóng góp vào nghiên cứu thúc đẩy lĩnh vực này. Anh có bằng thạc sĩ Khoa học Dữ liệu từ Đại học Georgetown. Ngoài công việc, anh thích “geek” về ô tô, xe máy và khám phá thiên nhiên.
