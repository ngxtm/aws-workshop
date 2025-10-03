---
title : "Blog 1"
date :  2025-09-10 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
## Xây dựng trợ lý vận hành độ tin cậy hệ thống (SRE) đa tác tử với Amazon Bedrock AgentCore

*by Amit Arora và Dheeraj Oruganty vào ngày 26 THÁNG 9 2025 trong Amazon Bedrock, Amazon Bedrock Agents, Amazon Bedrock Knowledge Bases, Amazon Machine Learning, Artificial Intelligence, Generative AI, Technical How-to*
*Permalink  Comments  Share*

Các kỹ sư độ tin cậy hệ thống (SRE) đang đối mặt với một thách thức ngày càng phức tạp trong các hệ thống phân tán hiện đại. Trong các sự cố sản xuất, họ phải nhanh chóng tương quan dữ liệu từ nhiều nguồn—log, metric, sự kiện Kubernetes và runbook vận hành—để xác định nguyên nhân gốc rễ và triển khai giải pháp. Các công cụ giám sát truyền thống cung cấp dữ liệu thô nhưng thiếu trí tuệ để tổng hợp thông tin trên các hệ thống đa dạng này, thường để SRE phải tự ghép nối câu chuyện phía sau các lỗi hệ thống.

Với một giải pháp AI sinh (generative AI), SRE có thể đặt câu hỏi cho hạ tầng của họ bằng ngôn ngữ tự nhiên. Ví dụ, họ có thể hỏi “Tại sao các pod payment-service bị crash looping?” hoặc “Điều gì gây ra spike độ trễ API?” và nhận được các insight toàn diện, khả thi kết hợp trạng thái hạ tầng, phân tích log, metric hiệu năng và các thủ tục khắc phục theo từng bước. Khả năng này biến phản ứng sự cố từ một quy trình thủ công, tốn thời gian thành một cuộc điều tra hợp tác, hiệu quả về thời gian.

Trong bài viết này, chúng tôi trình bày cách xây dựng một trợ lý SRE đa tác tử bằng Amazon Bedrock AgentCore, LangGraph và Model Context Protocol (MCP). Hệ thống này triển khai các tác tử AI chuyên biệt cộng tác để cung cấp trí tuệ ngữ cảnh sâu mà các đội SRE hiện đại cần cho ứng phó sự cố hiệu quả và quản trị hạ tầng. Chúng tôi sẽ hướng dẫn bạn toàn bộ triển khai, từ thiết lập môi trường demo đến triển khai lên Amazon Bedrock AgentCore Runtime cho sử dụng sản xuất.

### Tổng quan giải pháp

Giải pháp này sử dụng một kiến trúc đa tác tử toàn diện giải quyết các thách thức của vận hành SRE hiện đại thông qua tự động hóa thông minh. Giải pháp bao gồm bốn tác tử AI chuyên biệt phối hợp với nhau dưới một tác tử giám sát để cung cấp phân tích hạ tầng toàn diện và hỗ trợ ứng phó sự cố.

Các ví dụ trong bài sử dụng dữ liệu được tạo tổng hợp từ môi trường demo của chúng tôi. Các máy chủ backend mô phỏng các cụm Kubernetes thực tế, log ứng dụng, metric hiệu năng và runbook vận hành. Trong triển khai sản xuất, các máy chủ stub này sẽ được thay thế bằng kết nối tới các hệ thống hạ tầng thực tế của bạn, các dịch vụ giám sát và kho tài liệu.

Kiến trúc thể hiện một số khả năng chính sau:

* **Truy vấn hạ tầng bằng ngôn ngữ tự nhiên** – Bạn có thể đặt các câu hỏi phức tạp về hạ tầng bằng tiếng Anh đơn giản và nhận được phân tích chi tiết kết hợp dữ liệu từ nhiều nguồn.
* **Cộng tác đa tác tử** – Các tác tử chuyên biệt cho Kubernetes, log, metric và thủ tục vận hành cùng làm việc để cung cấp insight toàn diện.
* **Tổng hợp dữ liệu thời gian thực** – Các tác tử truy cập dữ liệu hạ tầng trực tiếp thông qua các API chuẩn hóa và trình bày phát hiện đã được tương quan.
* **Tự động thực thi runbook** – Các tác tử truy xuất và hiển thị các thủ tục vận hành theo từng bước cho các kịch bản sự cố phổ biến.
* **Ghi nguồn (source attribution)** – Mỗi phát hiện đều bao gồm ghi nguồn rõ ràng để xác minh và kiểm toán.

Sơ đồ sau minh họa kiến trúc giải pháp.

*AWS AgentCore architecture showing SRE support agent workflow with API monitoring and authentication components* (Kiến trúc AWS AgentCore hiển thị luồng công việc của tác tử hỗ trợ SRE với các thành phần giám sát API và xác thực)

Kiến trúc cho thấy tác tử hỗ trợ SRE tích hợp liền mạch với các thành phần Amazon Bedrock AgentCore như sau:

* **Giao diện khách hàng** – Nhận cảnh báo về thời gian phản hồi API suy giảm và trả về phản hồi toàn diện của tác tử.
* **Amazon Bedrock AgentCore Runtime** – Quản lý môi trường thực thi cho giải pháp SRE đa tác tử.
* **Tác tử hỗ trợ SRE** – Hệ thống cộng tác đa tác tử xử lý sự cố và điều phối phản hồi.
* **Amazon Bedrock AgentCore Gateway** – Định tuyến yêu cầu tới các công cụ chuyên biệt thông qua giao diện OpenAPI:

  * Kubernetes API để lấy sự kiện cụm
  * Logs API để phân tích mẫu log
  * Metrics API để phân tích xu hướng hiệu năng
  * Runbooks API để tìm kiếm thủ tục vận hành
* **Amazon Bedrock AgentCore Memory** – Lưu trữ và truy xuất ngữ cảnh phiên và tương tác trước đó để đảm bảo tính liên tục.
* **Amazon Bedrock AgentCore Identity** – Xử lý xác thực cho truy cập công cụ bằng tích hợp Amazon Cognito.
* **Amazon Bedrock AgentCore Observability** – Thu thập và trực quan hóa trace của tác tử cho giám sát và gỡ lỗi.
* **Amazon Bedrock LLMs** – Cung cấp trí tuệ cho tác tử thông qua các mô hình ngôn ngữ lớn (LLM) Claude của Anthropic.

Giải pháp đa tác tử sử dụng mô hình giám sát–tác tử, trong đó một bộ điều phối trung tâm phối hợp năm tác tử chuyên biệt:

* **Tác tử giám sát (Supervisor agent)** – Phân tích truy vấn vào, tạo kế hoạch điều tra, định tuyến công việc đến các chuyên gia phù hợp và tổng hợp kết quả thành báo cáo toàn diện.
* **Tác tử hạ tầng Kubernetes** – Xử lý điều phối container và vận hành cụm, điều tra lỗi pod, vấn đề triển khai, ràng buộc tài nguyên và sự kiện cụm.
* **Tác tử log ứng dụng** – Xử lý dữ liệu log để tìm thông tin liên quan, xác định mẫu và bất thường, và tương quan sự kiện giữa nhiều dịch vụ.
* **Tác tử metric hiệu năng** – Giám sát metric hệ thống và xác định vấn đề hiệu năng, cung cấp phân tích thời gian thực và xu hướng lịch sử.
* **Tác tử runbook vận hành** – Cung cấp truy cập tới các thủ tục được tài liệu hóa, hướng dẫn gỡ lỗi và quy trình leo thang dựa trên tình huống hiện tại.

### Sử dụng các primitive của Amazon Bedrock AgentCore

Giải pháp thể hiện sức mạnh của Amazon Bedrock AgentCore bằng cách sử dụng nhiều primitive cốt lõi. Giải pháp hỗ trợ hai nhà cung cấp cho LLM của Anthropic. Amazon Bedrock hỗ trợ Claude 3.7 Sonnet cho các triển khai tích hợp trên AWS, và Anthropic API hỗ trợ Claude 4 Sonnet cho truy cập API trực tiếp.

Thành phần Amazon Bedrock AgentCore Gateway chuyển đổi các API backend của tác tử SRE (Kubernetes, log ứng dụng, metric hiệu năng và runbook vận hành) thành các công cụ Model Context Protocol (MCP). Điều này cho phép các tác tử được xây dựng bằng một framework mã nguồn mở hỗ trợ MCP (như LangGraph trong bài viết này) có thể truy cập liền mạch các API hạ tầng.

Bảo mật cho toàn bộ giải pháp được cung cấp bởi Amazon Bedrock AgentCore Identity. Nó hỗ trợ xác thực ingress để kiểm soát truy cập an toàn cho các tác tử kết nối vào gateway, và xác thực egress để quản lý xác thực với các máy chủ backend, cung cấp truy cập API an toàn mà không cần hardcode thông tin xác thực.

Môi trường thực thi serverless để triển khai tác tử SRE trong sản xuất được cung cấp bởi Amazon Bedrock AgentCore Runtime. Nó tự động scale từ 0 để xử lý các cuộc điều tra sự cố đồng thời trong khi vẫn duy trì cách ly phiên hoàn toàn. Amazon Bedrock AgentCore Runtime hỗ trợ cả OAuth và AWS Identity and Access Management (IAM) cho xác thực tác tử. Ứng dụng gọi các tác tử phải có quyền IAM và chính sách tin cậy phù hợp. Để biết thêm thông tin, xem *Identity and access management for Amazon Bedrock AgentCore*.

Amazon Bedrock AgentCore Memory biến tác tử SRE từ một hệ thống không trạng thái thành một trợ lý học tập thông minh cá nhân hóa các cuộc điều tra dựa trên sở thích người dùng và bối cảnh lịch sử. Thành phần memory cung cấp ba chiến lược khác biệt:

* **Chiến lược sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Lưu trữ sở thích cá nhân về phong cách điều tra, kênh giao tiếp, thủ tục leo thang và định dạng báo cáo. Ví dụ, Alice (một SRE kỹ thuật) nhận phân tích hệ thống chi tiết cùng các bước gỡ lỗi, trong khi Carol (một lãnh đạo) nhận tóm tắt tập trung vào tác động kinh doanh.
* **Chiến lược tri thức hạ tầng** (`/sre/infrastructure/{user_id}/{session_id}`) – Tích lũy chuyên môn miền trên các cuộc điều tra, cho phép tác tử học từ các phát hiện trước đó. Khi tác tử Kubernetes xác định một mẫu rò rỉ bộ nhớ, tri thức này sẽ sẵn có cho các cuộc điều tra tương lai, cho phép xác định nguyên nhân gốc nhanh hơn.
* **Chiến lược bộ nhớ điều tra** (`/sre/investigations/{user_id}/{session_id}`) – Duy trì bối cảnh lịch sử của các sự cố trong quá khứ và cách giải quyết của chúng. Điều này cho phép giải pháp đề xuất các hướng khắc phục đã được chứng minh và tránh các anti-pattern từng thất bại.

Thành phần memory thể hiện giá trị của nó thông qua các cuộc điều tra được cá nhân hóa. Khi cả Alice và Carol điều tra “Thời gian phản hồi API đã suy giảm 3 lần trong giờ qua,” họ nhận được các phát hiện kỹ thuật giống hệt nhau nhưng phần trình bày hoàn toàn khác.

Alice nhận một phân tích kỹ thuật:

```
memory_client.retrieve_user_preferences(user_id="Alice")
# Returns: {"investigation_style": "detailed_systematic_analysis", "reports": "technical_exposition_with_troubleshooting_steps"}
```

Carol nhận một bản tóm tắt dành cho lãnh đạo:

```
memory_client.retrieve_user_preferences(user_id="Carol")
# Returns: {"investigation_style": "business_impact_focused","reports": "executive_summary_without_technical_details"}
```

### Bổ sung khả năng quan sát (observability) cho tác tử SRE

Việc bổ sung quan sát cho một tác tử SRE được triển khai trên Amazon Bedrock AgentCore Runtime trở nên đơn giản nhờ primitive Amazon Bedrock AgentCore Observability. Điều này cho phép giám sát toàn diện qua Amazon CloudWatch với metric, trace và log. Thiết lập quan sát yêu cầu ba bước:

Thêm các gói OpenTelemetry vào `pyproject.toml`:

```
dependencies = [
    # ... other dependencies ...
    "opentelemetry-instrumentation-langchain",
    "aws-opentelemetry-distro~=0.10.1",
]
```

Cấu hình observability cho các tác tử của bạn để bật metric trong CloudWatch.
Khởi động container của bạn bằng tiện ích `opentelemetry-instrument` để tự động instrument ứng dụng.

Lệnh sau được thêm vào Dockerfile cho tác tử SRE:

```
# Run application with OpenTelemetry instrumentation
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Như được hiển thị trong ảnh chụp màn hình sau, khi bật quan sát, bạn có tầm nhìn vào các nội dung sau:

* **Metric gọi LLM** – Mức sử dụng token, độ trễ và hiệu năng mô hình trên các tác tử.
* **Trace thực thi công cụ** – Thời lượng và tỷ lệ thành công cho mỗi lần gọi công cụ MCP.
* **Hoạt động bộ nhớ** – Mẫu truy xuất và hiệu quả lưu trữ.
* **Theo dõi yêu cầu đầu-cuối** – Luồng yêu cầu hoàn chỉnh từ truy vấn người dùng đến phản hồi cuối cùng.

*AWS CloudWatch observability dashboard for SRE agent showing session metrics, trace counts, and FM token usage trends* (Bảng điều khiển quan sát CloudWatch cho tác tử SRE hiển thị metric phiên, số lượng trace và xu hướng sử dụng token FM)

Primitive observability tự động thu thập các metric này mà không cần thay đổi mã bổ sung, cung cấp khả năng giám sát cấp độ sản xuất ngay từ đầu.

### Quy trình từ phát triển đến sản xuất

Tác tử SRE tuân theo quy trình triển khai có cấu trúc bốn bước từ phát triển cục bộ đến sản xuất, với quy trình chi tiết được ghi trong *Development to Production Flow* trong repo GitHub đi kèm:

*The four-step structured deployment process* (Quy trình triển khai có cấu trúc bốn bước)

Quy trình triển khai duy trì tính nhất quán giữa các môi trường: mã tác tử lõi (`sre_agent/`) không đổi, và thư mục `deployment/` chứa các tiện ích dành riêng cho triển khai. Cùng một tác tử hoạt động ở local và production thông qua cấu hình môi trường, với Amazon Bedrock AgentCore Gateway cung cấp quyền truy cập công cụ MCP trên các giai đoạn phát triển và triển khai khác nhau.

### Hướng dẫn triển khai (Implementation walkthrough)

Trong phần sau, chúng tôi tập trung vào cách Amazon Bedrock AgentCore Gateway, Memory và Runtime phối hợp để xây dựng giải pháp cộng tác đa tác tử này và triển khai end-to-end với hỗ trợ MCP và trí tuệ bền vững.

Chúng tôi bắt đầu bằng thiết lập repository và thiết lập môi trường runtime cục bộ với API key, nhà cung cấp LLM và hạ tầng demo. Sau đó, chúng tôi đưa các thành phần AgentCore cốt lõi online bằng cách tạo gateway cho truy cập API chuẩn hóa, cấu hình xác thực và thiết lập kết nối công cụ. Chúng tôi thêm trí tuệ thông qua AgentCore Memory, tạo các chiến lược cho sở thích người dùng và lịch sử điều tra đồng thời nạp persona cho phản ứng sự cố được cá nhân hóa. Cuối cùng, chúng tôi cấu hình các tác tử riêng lẻ với công cụ chuyên biệt, tích hợp khả năng memory, điều phối quy trình cộng tác và triển khai lên AgentCore Runtime với khả năng quan sát đầy đủ.

Hướng dẫn chi tiết cho từng bước được cung cấp trong repository:

* **Use Case Setup Guide** – Triển khai backend và thiết lập phát triển.
* **Deployment Guide** – Đóng gói sản xuất và triển khai Amazon Bedrock AgentCore Runtime.

### Điều kiện tiên quyết (Prerequisites)

Bạn có thể tìm yêu cầu chuyển tiếp cổng và các hướng dẫn thiết lập khác trong phần *Prerequisites* của README.

### Chuyển đổi API thành công cụ MCP với Amazon Bedrock AgentCore Gateway

Amazon Bedrock AgentCore Gateway thể hiện sức mạnh của chuẩn hóa giao thức bằng cách chuyển đổi các API backend hiện có thành các công cụ MCP mà các framework tác tử có thể tiêu thụ. Sự chuyển đổi này diễn ra liền mạch, chỉ cần các đặc tả OpenAPI.

#### Tải lên đặc tả OpenAPI

Quy trình gateway bắt đầu bằng việc tải lên các đặc tả API hiện có của bạn lên Amazon Simple Storage Service (Amazon S3). Script `create_gateway.sh` tự động xử lý việc tải lên bốn đặc tả API (Kubernetes, Logs, Metrics và Runbooks) vào bucket S3 đã cấu hình của bạn với metadata và content type phù hợp. Các đặc tả này sẽ được dùng để tạo các đích endpoint API trong gateway.

#### Tạo nhà cung cấp danh tính và gateway

Xác thực được xử lý liền mạch thông qua Amazon Bedrock AgentCore Identity. Script `main.py` tạo cả nhà cung cấp thông tin xác thực và gateway:

```
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

#### Triển khai các đích endpoint API với nhà cung cấp thông tin xác thực

Mỗi API trở thành một đích MCP thông qua gateway. Giải pháp tự động xử lý quản lý thông tin xác thực:

```
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

#### Xác nhận công cụ MCP sẵn sàng cho framework tác tử

Sau triển khai, Amazon Bedrock AgentCore Gateway cung cấp một endpoint `/mcp` chuẩn hóa được bảo vệ bằng token JWT. Kiểm thử triển khai với `mcp_cmds.sh` cho thấy sức mạnh của sự chuyển đổi này:

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

### Tính tương thích rộng với framework tác tử

Gateway chuẩn MCP này giờ có thể được cấu hình như một máy chủ Streamable-HTTP cho các client MCP, bao gồm AWS Strands, framework phát triển tác tử của Amazon, LangGraph (framework dùng trong triển khai tác tử SRE của chúng tôi), và CrewAI, một framework cộng tác đa tác tử.

Lợi thế của cách tiếp cận này là các API hiện có không cần sửa đổi—chỉ cần đặc tả OpenAPI. Amazon Bedrock AgentCore Gateway xử lý các nội dung sau:

* **Chuyển đổi giao thức** – giữa REST API và MCP.
* **Xác thực** – xác nhận token JWT và chèn thông tin xác thực.
* **Bảo mật** – kết thúc TLS và kiểm soát truy cập.
* **Chuẩn hóa** – tên công cụ và xử lý tham số nhất quán.

Điều này có nghĩa bạn có thể đưa các API hạ tầng hiện có (Kubernetes, giám sát, logging, tài liệu) và ngay lập tức làm cho chúng khả dụng đối với các framework tác tử hỗ trợ MCP—thông qua một giao diện duy nhất, an toàn và chuẩn hóa.

### Triển khai trí tuệ bền vững với Amazon Bedrock AgentCore Memory

Trong khi Amazon Bedrock AgentCore Gateway cung cấp truy cập API liền mạch, Amazon Bedrock AgentCore Memory biến tác tử SRE từ một hệ thống không trạng thái thành một trợ lý học hỏi, thông minh. Việc triển khai memory cho thấy chỉ với vài dòng mã có thể bật cá nhân hóa tinh vi và lưu giữ tri thức xuyên phiên.

#### Khởi tạo các chiến lược bộ nhớ

Thành phần bộ nhớ của tác tử SRE được xây dựng trên mô hình sự kiện của Amazon Bedrock AgentCore Memory với định tuyến namespace tự động. Trong quá trình khởi tạo, giải pháp tạo ba chiến lược bộ nhớ với các mẫu namespace riêng:

```
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

Ba chiến lược phục vụ các mục đích khác nhau:

* **Sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Phong cách điều tra và tùy chọn giao tiếp cá nhân.
* **Tri thức hạ tầng** (`/sre/infrastructure/{user_id}/{session_id}`) – Chuyên môn miền tích lũy qua các cuộc điều tra.
* **Tóm tắt điều tra** (`/sre/investigations/{user_id}/{session_id}`) – Mẫu sự cố lịch sử và cách giải quyết.

#### Nạp persona và sở thích người dùng

Giải pháp đi kèm sẵn các persona minh họa các cuộc điều tra được cá nhân hóa. Script `manage_memories.py` nạp các persona này:

```
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

#### Định tuyến namespace tự động trong thực tế

Sức mạnh của Amazon Bedrock AgentCore Memory nằm ở định tuyến namespace tự động. Khi tác tử SRE tạo sự kiện, nó chỉ cần cung cấp `actor_id`—Amazon Bedrock AgentCore Memory tự động xác định các namespace mà sự kiện thuộc về:

```
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

#### Xác thực trải nghiệm điều tra được cá nhân hóa

Tác động của thành phần bộ nhớ trở nên rõ ràng khi cả Alice và Carol điều tra cùng một vấn đề. Sử dụng các phát hiện kỹ thuật giống hệt nhau, giải pháp tạo ra các phần trình bày hoàn toàn khác nhau của cùng một nội dung nền tảng.

Báo cáo kỹ thuật của Alice chứa phân tích hệ thống chi tiết cho các nhóm kỹ thuật:

**Technical Investigation Summary**

**Root Cause:** Payment processor memory leak causing OOM kills

**Analysis:**

* Pod restart frequency increased 300% at 14:23 UTC
* Memory utilization peaked at 8.2GB (80% of container limit)
* JVM garbage collection latency spiked to 2.3s

**Next Step:**

1. Implement heap dump analysis (`kubectl exec payment-pod -- jmap`)
2. Review recent code deployments for memory management changes
3. Consider increasing memory limits and implementing graceful shutdown

Bản tóm tắt cho lãnh đạo của Carol tập trung vào tác động kinh doanh dành cho các bên liên quan cấp điều hành:

**Business Impact Assessment**

**Status:** CRITICAL - Customer payment processing degraded
**Impact:** 23% transaction failure rate, $47K revenue at risk
**Timeline:** Issue detected 14:23 UTC, resolution ETA 45 minutes
**Business Actions:** - Customer communication initiated via status page - Finance team alerted for revenue impact tracking - Escalating to VP Engineering if not resolved by 15:15 UTC

Thành phần bộ nhớ cho phép cá nhân hóa này đồng thời liên tục học hỏi từ mỗi cuộc điều tra, xây dựng tri thức tổ chức cải thiện phản ứng sự cố theo thời gian.

### Triển khai lên sản xuất với Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore giúp việc triển khai các tác tử hiện có lên sản xuất trở nên đơn giản. Quy trình bao gồm ba bước chính: đóng gói container cho tác tử, triển khai lên Amazon Bedrock AgentCore Runtime và gọi (invoke) tác tử đã triển khai.

#### Đóng gói container cho tác tử

Amazon Bedrock AgentCore Runtime yêu cầu container ARM64. Mã sau hiển thị Dockerfile hoàn chỉnh:

```
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

Các tác tử hiện có chỉ cần một lớp bọc FastAPI (`agent_runtime:app`) để trở nên tương thích với Amazon Bedrock AgentCore, và chúng tôi thêm `opentelemetry-instrument` để bật quan sát thông qua Amazon Bedrock AgentCore.

#### Triển khai lên Amazon Bedrock AgentCore Runtime

Triển khai lên Amazon Bedrock AgentCore Runtime rất đơn giản với script `deploy_agent_runtime.py`:

```
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

Amazon Bedrock AgentCore tự động xử lý hạ tầng, scaling và quản lý phiên.

#### Gọi tác tử đã triển khai

Gọi tác tử đã triển khai cũng đơn giản với `invoke_agent_runtime.py`:

```
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

#### Các lợi ích chính của Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore Runtime cung cấp các lợi ích chính sau:

* **Không cần quản lý hạ tầng** – Không có máy chủ, cân bằng tải hay cấu hình scale.
* **Cách ly phiên tích hợp** – Mỗi cuộc hội thoại được cách ly hoàn toàn.
* **Tích hợp AWS IAM** – Kiểm soát truy cập an toàn mà không cần xác thực tùy chỉnh.
* **Tự động mở rộng** – Scale từ 0 tới hàng nghìn phiên đồng thời.

Toàn bộ quy trình triển khai, bao gồm xây dựng container và xử lý quyền AWS, được ghi trong *Deployment Guide*.

### Tình huống thực tế (Real-world use cases)

Hãy khám phá cách tác tử SRE xử lý các kịch bản ứng phó sự cố phổ biến với một cuộc điều tra thực.

Khi đối mặt với một vấn đề sản xuất, bạn có thể truy vấn hệ thống bằng ngôn ngữ tự nhiên. Giải pháp sử dụng Amazon Bedrock AgentCore Memory để cá nhân hóa cuộc điều tra dựa trên vai trò và sở thích của bạn:

```
export USER_ID=Alice
sre-agent --prompt "API response times have degraded 3x in the last hour"
```

Tác tử giám sát truy xuất sở thích của Alice từ bộ nhớ (phong cách phân tích hệ thống chi tiết) và tạo một kế hoạch điều tra phù hợp với vai trò của cô ấy là SRE Kỹ thuật:

**Investigation Plan**

1. Use metrics_agent to analyze API performance metrics including response times, error rates, and resource utilization to identify the extent and pattern of slowdown
2. Use logs_agent to examine application logs for errors related to slow API responses, focusing on database connection issues and memory errors
3. Use kubernetes_agent to check pod status and resource constraints, especially for web-service and database pods, looking for CrashLoopBackOff states and missing ConfigMaps
   **Complexity:** Simple
   **Auto-execute:** Yes
   **Agents involved:** Metrics Agent, Logs Agent, Kubernetes Agent

Các tác tử điều tra tuần tự theo kế hoạch, mỗi bên đóng góp phân tích chuyên biệt của mình. Sau đó, giải pháp tổng hợp các phát hiện này thành một **bản tóm tắt điều hành (Executive Summary)** toàn diện:

**Executive Summary**

**Key Insights**

* Root Cause: Database service failure due to missing ConfigMap 'database-config' in production namespace, causing database pod to crash repeatedly
* Impact: Severe performance degradation with API response times increased from 150ms to 5000ms (33x slower)
* Severity: High - Database unavailability, memory exhaustion (100%), and CPU saturation (95%) causing 75% error rate

**Next Steps**

1. Immediate (< 1 hour): Create/update ConfigMap 'database-config' in production namespace and restart database pod
2. Short-term (< 24 hours):

   * Fix permissions on '/var/lib/postgresql/data' directory
   * Increase Java heap space for web-service to address OutOfMemoryErrors
   * Optimize UserService.loadAllUsers method causing memory issues
3. Long-term (< 1 week):

   * Implement resource monitoring with alerts for CPU (>80%), memory (>90%)
   * Optimize slow database queries, particularly "SELECT * FROM users WHERE status='active'"
   * Scale up resources or implement autoscaling for web-service

**Critical Alerts**

* Database pod (database-pod-7b9c4d8f2a-x5m1q) in CrashLoopBackOff state
* Web-service experiencing OutOfMemoryErrors in UserService.loadAllUsers(UserService.java:45)
* Node-3 experiencing memory pressure (>85% usage)
* Web-app-deployment showing readiness probe failures with 503 errors

**Troubleshooting Steps**

1. Verify ConfigMap status: `kubectl get configmap database-config -n production`
2. Check database pod logs: `kubectl logs database-pod-7b9c4d8f2a-x5m1q -n production`
3. Create/update ConfigMap: `kubectl create configmap database-config --from-file=database.conf -n production`
4. Fix data directory permissions: `kubectl exec database-pod-7b9c4d8f2a-x5m1q -n production -- chmod -R 700 /var/lib/postgresql/data`
5. Restart database pod: `kubectl delete pod database-pod-7b9c4d8f2a-x5m1q -n production`

Cuộc điều tra này cho thấy cách các primitive của Amazon Bedrock AgentCore phối hợp với nhau:

* **Amazon Bedrock AgentCore Gateway** – Cung cấp truy cập an toàn tới các API hạ tầng thông qua công cụ MCP.
* **Amazon Bedrock AgentCore Identity** – Xử lý xác thực ingress và egress.
* **Amazon Bedrock AgentCore Runtime** – Lưu trữ giải pháp đa tác tử với khả năng mở rộng tự động.
* **Amazon Bedrock AgentCore Memory** – Cá nhân hóa trải nghiệm của Alice và lưu trữ tri thức điều tra cho các sự cố sau.
* **Amazon Bedrock AgentCore Observability** – Thu thập metric và trace chi tiết trong CloudWatch cho giám sát và gỡ lỗi.

Tác tử SRE thể hiện khả năng điều phối tác tử thông minh, với tác tử giám sát định tuyến công việc cho các chuyên gia dựa trên kế hoạch điều tra. Khả năng bộ nhớ của giải pháp đảm bảo mỗi cuộc điều tra xây dựng tri thức tổ chức và cung cấp trải nghiệm cá nhân hóa dựa trên vai trò và sở thích người dùng.

Cuộc điều tra này nêu bật một số khả năng quan trọng:

* **Tương quan đa nguồn** – Kết nối vấn đề cấu hình cơ sở dữ liệu với suy giảm hiệu năng API.
* **Điều tra tuần tự** – Các tác tử làm việc có hệ thống theo kế hoạch điều tra đồng thời cung cấp cập nhật trực tiếp.
* **Ghi nguồn** – Phát hiện bao gồm công cụ và nguồn dữ liệu cụ thể.
* **Insight khả thi** – Cung cấp dòng thời gian rõ ràng của các sự kiện và bước khôi phục được ưu tiên.
* **Phát hiện lỗi lan truyền** – Có thể giúp hiển thị cách một lỗi lan truyền qua hệ thống.

### Tác động kinh doanh (Business impact)

Các tổ chức triển khai trợ lý SRE được hỗ trợ AI báo cáo những cải thiện đáng kể trong các chỉ số vận hành chính. Các cuộc điều tra ban đầu trước đây mất 30–45 phút nay có thể hoàn thành trong 5–10 phút, cung cấp cho SRE bối cảnh toàn diện trước khi đi sâu vào phân tích chi tiết. Sự giảm đáng kể thời gian điều tra này chuyển trực tiếp thành tốc độ giải quyết sự cố nhanh hơn và giảm thời gian ngừng hoạt động. Giải pháp cải thiện cách SRE tương tác với hạ tầng của họ. Thay vì điều hướng qua nhiều bảng điều khiển và công cụ, kỹ sư có thể đặt câu hỏi bằng ngôn ngữ tự nhiên và nhận insight tổng hợp từ các nguồn dữ liệu liên quan. Sự giảm chuyển đổi ngữ cảnh này cho phép các đội duy trì tập trung trong các sự cố quan trọng và giảm gánh nặng nhận thức trong các cuộc điều tra. Quan trọng hơn, giải pháp dân chủ hóa tri thức trong toàn đội. Tất cả thành viên có thể truy cập cùng kỹ thuật điều tra toàn diện, giảm phụ thuộc vào tri thức bản địa và gánh nặng trực on-call. Phương pháp nhất quán do giải pháp cung cấp đảm bảo cách tiếp cận điều tra vẫn đồng nhất giữa các thành viên và loại sự cố, cải thiện độ tin cậy tổng thể và giảm khả năng bỏ sót bằng chứng.

Các báo cáo điều tra được tạo tự động cung cấp tài liệu có giá trị cho các buổi rà soát sau sự cố và giúp đội học hỏi từ mỗi sự cố, xây dựng tri thức tổ chức theo thời gian. Hơn nữa, giải pháp mở rộng các khoản đầu tư hạ tầng AWS hiện có, hoạt động cùng với các dịch vụ như Amazon CloudWatch, AWS Systems Manager và các công cụ vận hành AWS khác để cung cấp một hệ thống trí tuệ vận hành hợp nhất.

### Mở rộng giải pháp (Extending the solution)

Kiến trúc mô-đun giúp việc mở rộng giải pháp cho nhu cầu cụ thể của bạn trở nên đơn giản.

Ví dụ, bạn có thể thêm các tác tử chuyên biệt cho miền của bạn:

* **Security agent** – Kiểm tra tuân thủ và phản ứng sự cố bảo mật.
* **Database agent** – Gỡ lỗi và tối ưu hóa chuyên biệt cho cơ sở dữ liệu.
* **Network agent** – Gỡ lỗi kết nối và hạ tầng mạng.

Bạn cũng có thể thay thế các API demo bằng các kết nối tới hệ thống thực tế của bạn:

* **Tích hợp Kubernetes** – Kết nối tới API cụm của bạn để lấy trạng thái pod, deployment và sự kiện.
* **Tập trung log** – Tích hợp với dịch vụ quản lý log của bạn (Elasticsearch, Splunk, CloudWatch Logs).
* **Nền tảng metric** – Kết nối với dịch vụ giám sát của bạn (Prometheus, Datadog, CloudWatch Metrics).
* **Kho runbook** – Liên kết tới tài liệu vận hành và playbook được lưu trong wiki, Git hoặc knowledge base.

### Dọn dẹp (Clean up)

Để tránh phát sinh chi phí trong tương lai, hãy sử dụng script dọn dẹp để xóa các tài nguyên AWS tính phí được tạo trong quá trình demo:

```
# Complete cleanup - deletes AWS resources and local files
./scripts/cleanup.sh
```

Script này tự động thực hiện các hành động sau:

* Dừng các máy chủ backend.
* Xóa gateway và các đích (target) của nó.
* Xóa tài nguyên Amazon Bedrock AgentCore Memory.
* Xóa Amazon Bedrock AgentCore Runtime.
* Xóa các tệp được sinh ra (gateway URI, token, agent ARN, memory ID).

Để biết hướng dẫn dọn dẹp chi tiết, tham khảo *Cleanup Instructions*.

### Kết luận (Conclusion)

Tác tử SRE cho thấy cách các hệ thống đa tác tử có thể biến phản ứng sự cố từ thủ công, tốn thời gian thành một cuộc điều tra hợp tác, hiệu quả về thời gian, cung cấp cho SRE các insight họ cần để giải quyết vấn đề nhanh chóng và tự tin.

Bằng cách kết hợp hạ tầng cấp doanh nghiệp của Amazon Bedrock AgentCore với truy cập công cụ được chuẩn hóa trong MCP, chúng tôi đã tạo ra một nền tảng có thể thích ứng khi hạ tầng của bạn phát triển và các khả năng mới xuất hiện.

Toàn bộ triển khai có sẵn trong repository GitHub của chúng tôi, bao gồm môi trường demo, hướng dẫn cấu hình và ví dụ mở rộng. Chúng tôi khuyến khích bạn khám phá giải pháp, tùy chỉnh cho hạ tầng của bạn và chia sẻ trải nghiệm với cộng đồng.

Để bắt đầu xây dựng trợ lý SRE của riêng bạn, tham khảo các tài nguyên sau:

* **Automate tasks in your application using AI agents**
* **Amazon Bedrock AgentCore Samples GitHub repository**
* **Model Context Protocol documentation**
* **LangGraph documentation**

### Giới thiệu về tác giả (About the authors)

**Amit Arora** là Kiến trúc sư Chuyên gia AI và ML tại Amazon Web Services, hỗ trợ khách hàng doanh nghiệp sử dụng các dịch vụ machine learning trên đám mây để nhanh chóng mở rộng đổi mới. Anh cũng là giảng viên thỉnh giảng trong chương trình thạc sĩ khoa học dữ liệu và phân tích tại Đại học Georgetown ở Washington, D.C.

**Dheeraj Oruganty** là Chuyên viên Triển khai (Delivery Consultant) tại Amazon Web Services. Anh đam mê xây dựng các giải pháp Generative AI và Machine Learning sáng tạo mang lại tác động kinh doanh thực. Chuyên môn của anh bao gồm Đánh giá AI tác tử (Agentic AI Evaluations), Benchmarking và Điều phối tác tử (Agent Orchestration), nơi anh tích cực đóng góp vào nghiên cứu thúc đẩy lĩnh vực này. Anh có bằng thạc sĩ Khoa học Dữ liệu từ Đại học Georgetown. Ngoài công việc, anh thích tìm hiểu chuyên sâu về ô tô, xe máy và khám phá thiên nhiên.