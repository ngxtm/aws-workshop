---
title : "Blog 1"
date :  2025-09-10 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

## Xây dựng các trợ lý kỹ thuật độ tin cậy trang web đa tác nhân với Amazon Bedrock AgentCore

*bởi Amit Arora và Dheeraj Oruganty | vào ngày 26 THÁNG 9 2025 | trong [Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon Bedrock Agents](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/amazon-bedrock-agents/), [Amazon Bedrock Knowledge Bases](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/amazon-bedrock-knowledge-bases/), [Amazon Machine Learning](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/), [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/machine-learning/category/post-types/technical-how-to/)*

Các kỹ sư độ tin cậy trang web (SRE) đối mặt với thách thức ngày càng phức tạp trong các hệ thống phân tán hiện đại. Trong các sự cố sản xuất, họ phải nhanh chóng tương quan dữ liệu từ nhiều nguồn—nhật ký, số liệu, sự kiện Kubernetes và sổ tay vận hành—để xác định nguyên nhân gốc rễ và triển khai giải pháp. Các công cụ giám sát truyền thống cung cấp dữ liệu thô nhưng thiếu trí thông minh để tổng hợp thông tin trên các hệ thống đa dạng này, thường để SRE phải tự ghép nối câu chuyện đằng sau các lỗi hệ thống.

Với giải pháp AI tạo sinh, SRE có thể đặt câu hỏi về cơ sở hạ tầng của họ bằng ngôn ngữ tự nhiên. Ví dụ, họ có thể hỏi "Tại sao các pod payment-service bị lỗi liên tục?" hoặc "Điều gì gây ra sự tăng đột biến độ trễ API?" và nhận được những thông tin toàn diện, có thể hành động, kết hợp trạng thái cơ sở hạ tầng, phân tích nhật ký, số liệu hiệu suất và quy trình khắc phục từng bước. Khả năng này biến đổi phản ứng sự cố từ một quy trình thủ công, tốn thời gian thành một cuộc điều tra hiệu quả về thời gian, cộng tác.

Trong bài viết này, chúng tôi trình bày cách xây dựng một trợ lý SRE đa tác nhân sử dụng [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/), [LangGraph](https://langchain-ai.github.io/langgraph/) và [Giao thức Ngữ cảnh Mô hình (MCP)](https://modelcontextprotocol.io/). Hệ thống này triển khai các tác nhân AI chuyên biệt cộng tác để cung cấp trí thông minh ngữ cảnh sâu mà các nhóm SRE hiện đại cần cho phản ứng sự cố và quản lý cơ sở hạ tầng hiệu quả. Chúng tôi hướng dẫn bạn qua toàn bộ quá trình triển khai, từ thiết lập môi trường demo đến triển khai trên Amazon Bedrock AgentCore Runtime để sử dụng sản xuất.

## Tổng quan giải pháp

Giải pháp này sử dụng kiến trúc đa tác nhân toàn diện giải quyết các thách thức của hoạt động SRE hiện đại thông qua tự động hóa thông minh. Giải pháp bao gồm bốn tác nhân AI chuyên biệt làm việc cùng nhau dưới sự giám sát của một tác nhân giám sát để cung cấp phân tích cơ sở hạ tầng toàn diện và hỗ trợ phản ứng sự cố.

Các ví dụ trong bài viết này sử dụng dữ liệu được tạo tổng hợp từ môi trường demo của chúng tôi. Các máy chủ backend mô phỏng các cụm Kubernetes thực tế, nhật ký ứng dụng, số liệu hiệu suất và sổ tay vận hành. Trong triển khai sản xuất, các máy chủ giả này sẽ được thay thế bằng kết nối đến các hệ thống cơ sở hạ tầng thực tế, dịch vụ giám sát và kho tài liệu của bạn.

Kiến trúc trình bày một số khả năng chính:

- **Truy vấn cơ sở hạ tầng bằng ngôn ngữ tự nhiên** – Bạn có thể đặt các câu hỏi phức tạp về cơ sở hạ tầng của mình bằng tiếng Anh đơn giản và nhận được phân tích chi tiết kết hợp dữ liệu từ nhiều nguồn
- **Cộng tác đa tác nhân** – Các tác nhân chuyên biệt cho Kubernetes, nhật ký, số liệu và quy trình vận hành làm việc cùng nhau để cung cấp thông tin toàn diện
- **Tổng hợp dữ liệu thời gian thực** – Các tác nhân truy cập dữ liệu cơ sở hạ tầng trực tiếp thông qua API tiêu chuẩn và trình bày các phát hiện tương quan
- **Thực thi sổ tay tự động** – Các tác nhân truy xuất và hiển thị các quy trình vận hành từng bước cho các kịch bản sự cố phổ biến
- **Ghi nhận nguồn** – Mọi phát hiện đều bao gồm ghi nhận nguồn rõ ràng cho mục đích xác minh và kiểm toán

Sơ đồ sau minh họa kiến trúc giải pháp.

![Kiến trúc AWS AgentCore cho thấy quy trình làm việc của tác nhân hỗ trợ SRE với các thành phần giám sát và xác thực API](/images/translated-blogs/blog1/pic1.png)

Kiến trúc trình bày cách tác nhân hỗ trợ SRE tích hợp liền mạch với các thành phần Amazon Bedrock AgentCore:

- **Giao diện khách hàng** – Nhận cảnh báo về thời gian phản hồi API giảm sút và trả về phản hồi tác nhân toàn diện
- **Amazon Bedrock AgentCore Runtime** – Quản lý môi trường thực thi cho giải pháp SRE đa tác nhân
- **Tác nhân hỗ trợ SRE** – Hệ thống cộng tác đa tác nhân xử lý sự cố và điều phối phản hồi
- **Amazon Bedrock AgentCore Gateway** – Định tuyến yêu cầu đến các công cụ chuyên biệt thông qua giao diện OpenAPI:
  - API Kubernetes để lấy sự kiện cụm
  - API nhật ký để phân tích mẫu nhật ký
  - API số liệu để phân tích xu hướng hiệu suất
  - API sổ tay để tìm kiếm quy trình vận hành
- **Amazon Bedrock AgentCore Memory** – Lưu trữ và truy xuất ngữ cảnh phiên và tương tác trước đó để đảm bảo tính liên tục
- **Amazon Bedrock AgentCore Identity** – Xử lý xác thực cho quyền truy cập công cụ sử dụng tích hợp [Amazon Cognito](https://aws.amazon.com/cognito)
- **Amazon Bedrock AgentCore Observability** – Thu thập và trực quan hóa dấu vết tác nhân để giám sát và gỡ lỗi
- **Amazon Bedrock LLMs** – Cung cấp trí thông minh tác nhân thông qua các mô hình ngôn ngữ lớn (LLM) Claude của Anthropic

Giải pháp đa tác nhân sử dụng mẫu tác nhân giám sát nơi một người điều phối trung tâm phối hợp năm tác nhân chuyên biệt:

- **Tác nhân giám sát** – Phân tích các truy vấn đến và tạo kế hoạch điều tra, định tuyến công việc cho các chuyên gia phù hợp và tổng hợp kết quả thành báo cáo toàn diện
- **Tác nhân cơ sở hạ tầng Kubernetes** – Xử lý điều phối container và hoạt động cụm, điều tra lỗi pod, vấn đề triển khai, hạn chế tài nguyên và sự kiện cụm
- **Tác nhân nhật ký ứng dụng** – Xử lý dữ liệu nhật ký để tìm thông tin liên quan, xác định mẫu và bất thường, và tương quan sự kiện trên nhiều dịch vụ
- **Tác nhân số liệu hiệu suất** – Giám sát số liệu hệ thống và xác định vấn đề hiệu suất, cung cấp phân tích thời gian thực và xu hướng lịch sử
- **Tác nhân sổ tay vận hành** – Cung cấp quyền truy cập vào các quy trình được ghi chép, hướng dẫn xử lý sự cố và quy trình leo thang dựa trên tình huống hiện tại

## Sử dụng các nguyên thủy Amazon Bedrock AgentCore

Giải pháp này thể hiện sức mạnh của Amazon Bedrock AgentCore bằng cách sử dụng nhiều nguyên thủy cốt lõi. Giải pháp hỗ trợ hai nhà cung cấp cho các LLM của Anthropic. Amazon Bedrock hỗ trợ Claude 3.7 Sonnet của Anthropic cho triển khai tích hợp AWS, và API Anthropic hỗ trợ Claude 4 Sonnet của Anthropic cho quyền truy cập API trực tiếp.

Thành phần Amazon Bedrock AgentCore Gateway chuyển đổi các API backend của tác nhân SRE (Kubernetes, nhật ký ứng dụng, số liệu hiệu suất và sổ tay vận hành) thành các công cụ Giao thức Ngữ cảnh Mô hình (MCP). Điều này cho phép các tác nhân được xây dựng bằng framework mã nguồn mở hỗ trợ MCP (như LangGraph trong bài viết này) truy cập liền mạch các API cơ sở hạ tầng.

Bảo mật cho toàn bộ giải pháp được cung cấp bởi Amazon Bedrock AgentCore Identity. Nó hỗ trợ xác thực đầu vào để kiểm soát truy cập an toàn cho các tác nhân kết nối với gateway, và xác thực đầu ra để quản lý xác thực với các máy chủ backend, cung cấp quyền truy cập API an toàn mà không cần mã hóa cứng thông tin xác thực.

Môi trường thực thi serverless để triển khai tác nhân SRE trong sản xuất được cung cấp bởi Amazon Bedrock AgentCore Runtime. Nó tự động mở rộng từ 0 để xử lý các cuộc điều tra sự cố đồng thời trong khi duy trì sự cô lập phiên hoàn toàn. Amazon Bedrock AgentCore Runtime hỗ trợ cả OAuth và [AWS Identity and Access Management](https://aws.amazon.com/iam) (IAM) để xác thực tác nhân. Các ứng dụng gọi tác nhân phải có quyền IAM và chính sách ủy thác phù hợp. Để biết thêm thông tin, xem [Identity and access management for Amazon Bedrock AgentCore.](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/security-iam.html)

Amazon Bedrock AgentCore Memory biến đổi tác nhân SRE từ một hệ thống không trạng thái thành một trợ lý học tập thông minh cá nhân hóa các cuộc điều tra dựa trên sở thích người dùng và ngữ cảnh lịch sử. Thành phần bộ nhớ cung cấp ba chiến lược riêng biệt:

- **Chiến lược sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Lưu trữ sở thích người dùng cá nhân cho phong cách điều tra, kênh truyền thông, quy trình leo thang và định dạng báo cáo. Ví dụ, Alice (một SRE kỹ thuật) nhận phân tích hệ thống chi tiết với các bước xử lý sự cố, trong khi Carol (một giám đốc điều hành) nhận tóm tắt tập trung vào tác động kinh doanh.
- **Chiến lược kiến thức cơ sở hạ tầng** (`/sre/infrastructure/{user_id}/{session_id}`) – Tích lũy chuyên môn miền qua các cuộc điều tra, cho phép các tác nhân học hỏi từ các phát hiện trước đó. Khi tác nhân Kubernetes xác định một mẫu rò rỉ bộ nhớ, kiến thức này trở nên có sẵn cho các cuộc điều tra tương lai, cho phép xác định nguyên nhân gốc rễ nhanh hơn.
- **Chiến lược bộ nhớ điều tra** (`/sre/investigations/{user_id}/{session_id}`) – Duy trì ngữ cảnh lịch sử của các sự cố trước đó và giải pháp của chúng. Điều này cho phép giải pháp đề xuất các phương pháp khắc phục đã được chứng minh và tránh các mẫu chống lại đã thất bại trước đó.

Thành phần bộ nhớ thể hiện giá trị của nó thông qua các cuộc điều tra cá nhân hóa. Khi cả Alice và Carol điều tra "Thời gian phản hồi API đã giảm sút 3 lần trong giờ qua," họ nhận được các phát hiện kỹ thuật giống hệt nhau nhưng trình bày hoàn toàn khác nhau.

Alice nhận được phân tích kỹ thuật:

```python
memory_client.retrieve_user_preferences(user_id="Alice")
# Trả về: {"investigation_style": "detailed_systematic_analysis", "reports": "technical_exposition_with_troubleshooting_steps"}
```

Carol nhận được tóm tắt điều hành:

```python
memory_client.retrieve_user_preferences(user_id="Carol") 
# Trả về: {"investigation_style": "business_impact_focused","reports": "executive_summary_without_technical_details"}
```

## Thêm khả năng quan sát cho tác nhân SRE

Thêm khả năng quan sát cho một tác nhân SRE được triển khai trên Amazon Bedrock AgentCore Runtime rất đơn giản sử dụng nguyên thủy Amazon Bedrock AgentCore Observability. Điều này cho phép giám sát toàn diện thông qua [Amazon CloudWatch](https://aws.amazon.com/cloudwatch) với số liệu, dấu vết và nhật ký. Thiết lập khả năng quan sát yêu cầu ba bước:

1. Thêm các gói OpenTelemetry vào `pyproject.toml` của bạn:

```toml
dependencies = [
    # ... các phụ thuộc khác ...
    "opentelemetry-instrumentation-langchain",
    "aws-opentelemetry-distro~=0.10.1",
]
```

2. [Cấu hình khả năng quan sát cho các tác nhân của bạn](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability-configure.html) để kích hoạt số liệu trong CloudWatch.
3. Khởi động container của bạn sử dụng tiện ích `opentelemetry-instrument` để tự động đo lường ứng dụng của bạn.

Lệnh sau được thêm vào Dockerfile cho tác nhân SRE:

```dockerfile
# Chạy ứng dụng với đo lường OpenTelemetry 
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Như được hiển thị trong ảnh chụp màn hình sau, với khả năng quan sát được kích hoạt, bạn có được khả năng hiển thị những điều sau:

- **Số liệu gọi LLM** – Sử dụng token, độ trễ và hiệu suất mô hình trên các tác nhân
- **Dấu vết thực thi công cụ** – Thời lượng và tỷ lệ thành công cho mỗi lệnh gọi công cụ MCP
- **Hoạt động bộ nhớ** – Mẫu truy xuất và hiệu quả lưu trữ
- **Theo dõi yêu cầu từ đầu đến cuối** – Luồng yêu cầu hoàn chỉnh từ truy vấn người dùng đến phản hồi cuối cùng

![Bảng điều khiển quan sát AWS CloudWatch cho tác nhân SRE hiển thị số liệu phiên, số lượng dấu vết và xu hướng sử dụng token FM](/images/translated-blogs/blog1/gif1.gif)

Nguyên thủy khả năng quan sát tự động nắm bắt các số liệu này mà không cần thay đổi mã bổ sung, cung cấp khả năng giám sát cấp sản xuất sẵn có.

## Quy trình từ phát triển đến sản xuất

Tác nhân SRE tuân theo quy trình triển khai có cấu trúc bốn bước từ phát triển cục bộ đến sản xuất, với các quy trình chi tiết được ghi chép trong [Development to Production Flow](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#development-to-production-deployment-flow) trong kho GitHub đi kèm:

![Quy trình triển khai có cấu trúc bốn bước](/images/translated-blogs/blog1/pic2.png)

Quy trình triển khai duy trì tính nhất quán qua các môi trường: mã tác nhân cốt lõi (`sre_agent/`) không thay đổi, và thư mục `deployment/` chứa các tiện ích cụ thể cho triển khai. Cùng một tác nhân hoạt động cục bộ và trong sản xuất thông qua cấu hình môi trường, với Amazon Bedrock AgentCore Gateway cung cấp quyền truy cập công cụ MCP qua các giai đoạn phát triển và triển khai khác nhau.

## Hướng dẫn triển khai

Trong phần sau, chúng tôi tập trung vào cách Amazon Bedrock AgentCore Gateway, Memory và Runtime làm việc cùng nhau để xây dựng giải pháp cộng tác đa tác nhân này và triển khai nó từ đầu đến cuối với hỗ trợ MCP và trí thông minh bền vững.

Chúng tôi bắt đầu bằng cách thiết lập kho lưu trữ và thiết lập môi trường runtime cục bộ với khóa API, nhà cung cấp LLM và cơ sở hạ tầng demo. Sau đó, chúng tôi đưa các thành phần AgentCore cốt lõi trực tuyến bằng cách tạo gateway cho quyền truy cập API tiêu chuẩn, cấu hình xác thực và thiết lập kết nối công cụ. Chúng tôi thêm trí thông minh thông qua AgentCore Memory, tạo chiến lược cho sở thích người dùng và lịch sử điều tra trong khi tải persona cho phản ứng sự cố cá nhân hóa. Cuối cùng, chúng tôi cấu hình các tác nhân riêng lẻ với công cụ chuyên biệt, tích hợp khả năng bộ nhớ, điều phối quy trình làm việc cộng tác và triển khai lên AgentCore Runtime với khả năng quan sát đầy đủ.

Hướng dẫn chi tiết cho từng bước được cung cấp trong kho lưu trữ:

- **[Use Case Setup Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#use-case-setup)** – Triển khai backend và thiết lập phát triển
- **[Deployment Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent/docs/deployment-guide.md)** – Container hóa sản xuất và triển khai Amazon Bedrock AgentCore Runtime

### Điều kiện tiên quyết

Bạn có thể tìm thấy yêu cầu chuyển tiếp cổng và hướng dẫn thiết lập khác trong phần [Prerequisites](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#prerequisites) của tệp README.

## Chuyển đổi API thành công cụ MCP với Amazon Bedrock AgentCore Gateway

Amazon Bedrock AgentCore Gateway thể hiện sức mạnh của tiêu chuẩn hóa giao thức bằng cách chuyển đổi các API backend hiện có thành các công cụ MCP mà các framework tác nhân có thể sử dụng. Chuyển đổi này xảy ra liền mạch, chỉ yêu cầu các đặc tả OpenAPI.

### Tải lên các đặc tả OpenAPI

Quy trình gateway bắt đầu bằng cách tải lên các đặc tả API hiện có của bạn lên [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3). Script [create_gateway.sh](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/create_gateway.sh) tự động xử lý việc tải lên bốn đặc tả API (Kubernetes, Logs, Metrics và Runbooks) lên bucket S3 được cấu hình của bạn với metadata và loại nội dung thích hợp. Các đặc tả này sẽ được sử dụng để tạo mục tiêu điểm cuối API trong gateway.

### Tạo nhà cung cấp định danh và gateway

Xác thực được xử lý liền mạch thông qua Amazon Bedrock AgentCore Identity. Script [main.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/main.py) tạo cả nhà cung cấp thông tin xác thực và gateway:

```python
# Tạo AgentCore Gateway với ủy quyền JWT
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
    
    # Xây dựng cấu hình auth cho Cognito
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

### Triển khai các mục tiêu điểm cuối API với nhà cung cấp thông tin xác thực

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

    # Cấu hình nhà cung cấp thông tin xác thực khóa API
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

### Xác thực công cụ MCP đã sẵn sàng cho framework tác nhân

Sau triển khai, Amazon Bedrock AgentCore Gateway cung cấp một điểm cuối `/mcp` tiêu chuẩn được bảo mật với token JWT. Kiểm tra triển khai với [mcp_cmds.sh](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/mcp_cmds.sh) tiết lộ sức mạnh của chuyển đổi này:

```
Tóm tắt công cụ:
================
Tổng số công cụ tìm thấy: 21

Tên công cụ:
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

## Khả năng tương thích framework tác nhân phổ quát

Gateway tiêu chuẩn hóa MCP này giờ đây có thể được cấu hình làm máy chủ HTTP-Streamable cho các máy khách MCP, bao gồm AWS Strands, framework phát triển tác nhân của Amazon, LangGraph, framework được sử dụng trong triển khai tác nhân SRE của chúng tôi, và CrewAI, một framework cộng tác đa tác nhân.

Ưu điểm của phương pháp này là các API hiện có không yêu cầu sửa đổi—chỉ cần đặc tả OpenAPI. Amazon Bedrock AgentCore Gateway xử lý những điều sau:

- **Chuyển đổi giao thức** – Giữa REST API sang MCP
- **Xác thực** – Xác thực token JWT và chèn thông tin xác thực
- **Bảo mật** – Kết thúc TLS và kiểm soát truy cập
- **Tiêu chuẩn hóa** – Đặt tên công cụ nhất quán và xử lý tham số

Điều này có nghĩa là bạn có thể lấy các API cơ sở hạ tầng hiện có (Kubernetes, giám sát, ghi nhật ký, tài liệu) và ngay lập tức làm cho chúng có sẵn cho các framework tác nhân AI hỗ trợ MCP—thông qua một giao diện đơn lẻ, an toàn, tiêu chuẩn.

## Triển khai trí thông minh bền vững với Amazon Bedrock AgentCore Memory

Trong khi Amazon Bedrock AgentCore Gateway cung cấp quyền truy cập API liền mạch, Amazon Bedrock AgentCore Memory biến đổi tác nhân SRE từ một hệ thống không trạng thái thành một trợ lý học tập thông minh. Triển khai bộ nhớ thể hiện cách một vài dòng mã có thể cho phép cá nhân hóa tinh vi và giữ lại kiến thức qua phiên.

### Khởi tạo chiến lược bộ nhớ

Thành phần bộ nhớ tác nhân SRE được xây dựng trên mô hình dựa trên sự kiện của Amazon Bedrock AgentCore Memory với định tuyến namespace tự động. Trong quá trình khởi tạo, giải pháp tạo ba chiến lược bộ nhớ với các mẫu namespace cụ thể:

```python
from sre_agent.memory.client import SREMemoryClient
from sre_agent.memory.strategies import create_memory_strategies

# Khởi tạo client bộ nhớ
memory_client = SREMemoryClient(
    memory_name="sre_agent_memory",
    region="us-east-1"
)

# Tạo ba chiến lược bộ nhớ chuyên biệt
strategies = create_memory_strategies()
for strategy in strategies:
    memory_client.create_strategy(strategy)
```

Ba chiến lược mỗi chiến lược phục vụ các mục đích riêng biệt:

- **Sở thích người dùng** (`/sre/users/{user_id}/preferences`) – Phong cách điều tra cá nhân và sở thích giao tiếp
- **Kiến thức cơ sở hạ tầng**: `/sre/infrastructure/{user_id}/{session_id}` – Chuyên môn miền tích lũy qua các cuộc điều tra
- **Tóm tắt điều tra**: `/sre/investigations/{user_id}/{session_id}` – Các mẫu sự cố lịch sử và giải pháp

### Tải persona người dùng và sở thích

Giải pháp được cấu hình sẵn với các persona người dùng thể hiện các cuộc điều tra cá nhân hóa. Script [manage_memories.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/scripts/manage_memories.py) tải các persona này:

```python
# Tải Alice - Kỹ sư SRE Kỹ thuật
alice_preferences = {
    "investigation_style": "detailed_systematic_analysis",
    "communication": ["#alice-alerts", "#sre-team"],
    "escalation": {"contact": "alice.manager@company.com", "threshold": "15min"},
    "reports": "technical_exposition_with_troubleshooting_steps",
    "timezone": "UTC"
}

# Tải Carol - Giám đốc điều hành/Giám đốc
carol_preferences = {
    "investigation_style": "business_impact_focused",
    "communication": ["#carol-executive", "#strategic-alerts"],
    "escalation": {"contact": "carol.director@company.com", "threshold": "5min"},
    "reports": "executive_summary_without_technical_details",
    "timezone": "EST"
}

# Lưu trữ sở thích sử dụng client bộ nhớ
memory_client.store_user_preference("Alice", alice_preferences)
memory_client.store_user_preference("Carol", carol_preferences)
```

### Định tuyến namespace tự động trong hành động

Sức mạnh của Amazon Bedrock AgentCore Memory nằm ở định tuyến namespace tự động của nó. Khi tác nhân SRE tạo sự kiện, nó chỉ cần cung cấp `actor_id`—Amazon Bedrock AgentCore Memory tự động xác định namespace mà sự kiện thuộc về:

```python
# Trong quá trình điều tra, tác nhân giám sát lưu trữ ngữ cảnh
memory_client.create_event(
    memory_id="sre_agent_memory-abc123",
    actor_id="Alice",  # AgentCore Memory định tuyến điều này tự động
    session_id="investigation_2025_01_15",
    messages=[("investigation_started", "USER")]
)

# Hệ thống bộ nhớ tự động:
# 1. Kiểm tra namespace chiến lược
# 2. Khớp actor_id "Alice" với /sre/users/Alice/preferences
# 3. Lưu trữ sự kiện trong Chiến lược Sở thích Người dùng
# 4. Làm cho sự kiện có sẵn cho các truy xuất trong tương lai
```

### Xác thực trải nghiệm điều tra cá nhân hóa

Tác động của thành phần bộ nhớ trở nên rõ ràng khi cả Alice và Carol điều tra cùng một vấn đề. Sử dụng các phát hiện kỹ thuật giống hệt nhau, giải pháp tạo ra các bản trình bày hoàn toàn khác nhau của cùng nội dung cơ bản.

Báo cáo kỹ thuật của Alice chứa phân tích hệ thống chi tiết cho các nhóm kỹ thuật:

```
Tóm tắt Điều tra Kỹ thuật

Nguyên nhân Gốc rễ: Rò rỉ bộ nhớ bộ xử lý thanh toán gây ra lỗi OOM

Phân tích:
- Tần suất khởi động lại Pod tăng 300% lúc 14:23 UTC
- Sử dụng bộ nhớ đạt đỉnh 8.2GB (80% giới hạn container)
- Độ trễ thu gom rác JVM tăng lên 2.3s

Bước Tiếp theo:
1. Triển khai phân tích heap dump (`kubectl exec payment-pod -- jmap`)
2. Xem xét các triển khai mã gần đây về các thay đổi quản lý bộ nhớ
3. Xem xét tăng giới hạn bộ nhớ và triển khai tắt máy nhẹ nhàng
```

Tóm tắt điều hành của Carol chứa tập trung vào tác động kinh doanh cho các bên liên quan điều hành:

```
Đánh giá Tác động Kinh doanh
Trạng thái: QUAN TRỌNG - Xử lý thanh toán khách hàng bị suy giảm
Tác động: Tỷ lệ thất bại giao dịch 23%, doanh thu $47K có nguy cơ
Dòng thời gian: Phát hiện vấn đề 14:23 UTC, ETA giải quyết 45 phút
Hành động Kinh doanh: 
- Khởi tạo giao tiếp khách hàng qua trang trạng thái
- Cảnh báo nhóm tài chính về theo dõi tác động doanh thu
- Leo thang lên Phó Chủ tịch Kỹ thuật nếu không giải quyết trước 15:15 UTC
```

Thành phần bộ nhớ cho phép cá nhân hóa này trong khi liên tục học hỏi từ mỗi cuộc điều tra, xây dựng kiến thức tổ chức cải thiện phản ứng sự cố theo thời gian.

## Triển khai lên sản xuất với Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore giúp việc triển khai các tác nhân hiện có lên sản xuất trở nên đơn giản. Quy trình bao gồm ba bước chính: container hóa tác nhân của bạn, triển khai lên Amazon Bedrock AgentCore Runtime và gọi tác nhân đã triển khai.

### Container hóa tác nhân của bạn

Amazon Bedrock AgentCore Runtime yêu cầu container ARM64. Mã sau đây hiển thị [Dockerfile](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/Dockerfile) hoàn chỉnh:

```dockerfile
# Sử dụng hình ảnh cơ sở Python ARM64 của uv
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:python3.12-bookworm-slim

WORKDIR /app

# Sao chép các tệp uv
COPY pyproject.toml uv.lock ./

# Cài đặt phụ thuộc
RUN uv sync --frozen --no-dev

# Sao chép module tác nhân SRE
COPY sre_agent/ ./sre_agent/

# Đặt biến môi trường
# Lưu ý: Đặt DEBUG=true để kích hoạt ghi nhật ký và dấu vết gỡ lỗi
ENV PYTHONPATH="/app" \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Mở cổng
EXPOSE 8080

# Chạy ứng dụng với đo lường OpenTelemetry
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Các tác nhân hiện có chỉ cần một wrapper FastAPI (`agent_runtime:app`) để trở nên tương thích với Amazon Bedrock AgentCore, và chúng tôi thêm `opentelemetry-instrument` để kích hoạt khả năng quan sát thông qua Amazon Bedrock AgentCore.

### Triển khai lên Amazon Bedrock AgentCore Runtime

Triển khai lên Amazon Bedrock AgentCore Runtime rất đơn giản với script [deploy_agent_runtime.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/deployment/deploy_agent_runtime.py):

```python
import boto3

# Tạo client AgentCore
client = boto3.client('bedrock-agentcore', region_name=region)

# Biến môi trường cho tác nhân của bạn
env_vars = {
    'GATEWAY_ACCESS_TOKEN': gateway_access_token,
    'LLM_PROVIDER': llm_provider,
    'ANTHROPIC_API_KEY': anthropic_api_key  # nếu sử dụng Anthropic
}

# Triển khai container lên AgentCore Runtime
response = client.create_agent_runtime(
    agentRuntimeName=runtime_name,
    agentRuntimeArtifact={
        'containerConfiguration': {
            'containerUri': container_uri  # URI container ECR của bạn
        }
    },
    networkConfiguration={"networkMode": "PUBLIC"},
    roleArn=role_arn,
    environmentVariables=env_vars
)

print(f"ARN Runtime Tác nhân: {response['agentRuntimeArn']}")
```

Amazon Bedrock AgentCore xử lý cơ sở hạ tầng, mở rộng và quản lý phiên tự động.

### Gọi tác nhân đã triển khai của bạn

Gọi tác nhân đã triển khai của bạn cũng đơn giản với [invoke_agent_runtime.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/deployment/invoke_agent_runtime.py):

```python
# Chuẩn bị truy vấn của bạn với user_id và session_id cho cá nhân hóa bộ nhớ
payload = json.dumps({
    "input": {
        "prompt": "Thời gian phản hồi API đã giảm sút 3 lần trong giờ qua",
        "user_id": "Alice",  # Người dùng cho điều tra cá nhân hóa
        "session_id": "investigation-20250127-123456"  # Phiên cho ngữ cảnh
    }
})

# Gọi tác nhân đã triển khai
response = agent_core_client.invoke_agent_runtime(
    agentRuntimeArn=runtime_arn,
    runtimeSessionId=session_id,
    payload=payload,
    qualifier="DEFAULT"
)

# Nhận phản hồi
response_data = json.loads(response['response'].read())
print(response_data)  # Phản hồi đầy đủ bao gồm đầu ra với điều tra của tác nhân
```

### Lợi ích chính của Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore Runtime cung cấp các lợi ích chính sau:

- **Không cần quản lý cơ sở hạ tầng** – Không có máy chủ, bộ cân bằng tải hoặc mở rộng để cấu hình
- **Cô lập phiên tích hợp** – Mỗi cuộc trò chuyện được cô lập hoàn toàn
- **Tích hợp AWS IAM** – Kiểm soát truy cập an toàn mà không cần xác thực tùy chỉnh
- **Mở rộng tự động** – Mở rộng từ 0 đến hàng ngàn phiên đồng thời

Quy trình triển khai hoàn chỉnh, bao gồm xây dựng container và xử lý quyền AWS, được ghi chép trong [Deployment Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/docs/deployment-guide.md).

## Các trường hợp sử dụng thực tế

Hãy khám phá cách tác nhân SRE xử lý các kịch bản phản ứng sự cố phổ biến với một cuộc điều tra thực tế.

Khi đối mặt với một vấn đề sản xuất, bạn có thể truy vấn hệ thống bằng ngôn ngữ tự nhiên. Giải pháp sử dụng Amazon Bedrock AgentCore Memory để cá nhân hóa cuộc điều tra dựa trên vai trò và sở thích của bạn:

```bash
export USER_ID=Alice
sre-agent --prompt "Thời gian phản hồi API đã giảm sút 3 lần trong giờ qua"
```

Người giám sát truy xuất sở thích của Alice từ bộ nhớ (phong cách phân tích hệ thống chi tiết) và tạo kế hoạch điều tra phù hợp với vai trò của cô ấy là SRE Kỹ thuật:

```
Kế hoạch Điều tra
1. Sử dụng metrics_agent để phân tích số liệu hiệu suất API bao gồm thời gian phản hồi, tỷ lệ lỗi và sử dụng tài nguyên để xác định mức độ và mẫu chậm lại
2. Sử dụng logs_agent để kiểm tra nhật ký ứng dụng về lỗi liên quan đến phản hồi API chậm, tập trung vào vấn đề kết nối cơ sở dữ liệu và lỗi bộ nhớ
3. Sử dụng kubernetes_agent để kiểm tra trạng thái pod và hạn chế tài nguyên, đặc biệt cho các pod web-service và database, tìm kiếm trạng thái CrashLoopBackOff và ConfigMap bị thiếu
Độ phức tạp: Đơn giản
Tự động thực thi: Có
Các tác nhân liên quan: Tác nhân Số liệu, Tác nhân Nhật ký, Tác nhân Kubernetes
```

Các tác nhân điều tra tuần tự theo kế hoạch, mỗi tác nhân đóng góp phân tích chuyên biệt của họ. Sau đó, giải pháp tổng hợp các phát hiện này thành một tóm tắt điều hành toàn diện:

```
Tóm tắt Điều hành

Thông tin Chính
- Nguyên nhân Gốc rễ: Lỗi dịch vụ cơ sở dữ liệu do ConfigMap 'database-config' bị thiếu trong namespace production, khiến pod cơ sở dữ liệu bị lỗi liên tục
- Tác động: Suy giảm hiệu suất nghiêm trọng với thời gian phản hồi API tăng từ 150ms lên 5000ms (chậm hơn 33 lần)
- Mức độ nghiêm trọng: Cao - Không khả dụng cơ sở dữ liệu, cạn kiệt bộ nhớ (100%) và bão hòa CPU (95%) gây ra tỷ lệ lỗi 75%

Bước Tiếp theo
1. Ngay lập tức (< 1 giờ): Tạo/cập nhật ConfigMap 'database-config' trong namespace production và khởi động lại pod cơ sở dữ liệu
2. Ngắn hạn (< 24 giờ): 
   - Sửa quyền trên thư mục '/var/lib/postgresql/data'
   - Tăng không gian heap Java cho web-service để giải quyết OutOfMemoryErrors
   - Tối ưu hóa phương thức UserService.loadAllUsers gây ra vấn đề bộ nhớ
3. Dài hạn (< 1 tuần): 
   - Triển khai giám sát tài nguyên với cảnh báo cho CPU (>80%), bộ nhớ (>90%)
   - Tối ưu hóa các truy vấn cơ sở dữ liệu chậm, đặc biệt "SELECT * FROM users WHERE status='active'"
   - Mở rộng tài nguyên hoặc triển khai tự động mở rộng cho web-service

Cảnh báo Quan trọng
- Pod cơ sở dữ liệu (database-pod-7b9c4d8f2a-x5m1q) ở trạng thái CrashLoopBackOff
- Web-service gặp OutOfMemoryErrors trong UserService.loadAllUsers(UserService.java:45)
- Node-3 gặp áp lực bộ nhớ (>85% sử dụng)
- Web-app-deployment cho thấy lỗi kiểm tra sẵn sàng với lỗi 503

Các Bước Xử lý Sự cố
1. Xác minh trạng thái ConfigMap: `kubectl get configmap database-config -n production`
2. Kiểm tra nhật ký pod cơ sở dữ liệu: `kubectl logs database-pod-7b9c4d8f2a-x5m1q -n production`
3. Tạo/cập nhật ConfigMap: `kubectl create configmap database-config --from-file=database.conf -n production`
4. Sửa quyền thư mục dữ liệu: `kubectl exec database-pod-7b9c4d8f2a-x5m1q -n production -- chmod -R 700 /var/lib/postgresql/data`
5. Khởi động lại pod cơ sở dữ liệu: `kubectl delete pod database-pod-7b9c4d8f2a-x5m1q -n production`
```

Cuộc điều tra này thể hiện cách các nguyên thủy Amazon Bedrock AgentCore làm việc cùng nhau:

- **Amazon Bedrock AgentCore Gateway** – Cung cấp quyền truy cập an toàn vào các API cơ sở hạ tầng thông qua các công cụ MCP
- **Amazon Bedrock AgentCore Identity** – Xử lý xác thực đầu vào và đầu ra
- **Amazon Bedrock AgentCore Runtime** – Lưu trữ giải pháp đa tác nhân với mở rộng tự động
- **Amazon Bedrock AgentCore Memory** – Cá nhân hóa trải nghiệm của Alice và lưu trữ kiến thức điều tra cho các sự cố tương lai
- **Amazon Bedrock AgentCore Observability** – Nắm bắt số liệu và dấu vết chi tiết trong CloudWatch để giám sát và gỡ lỗi

Tác nhân SRE thể hiện điều phối tác nhân thông minh, với người giám sát định tuyến công việc cho các chuyên gia dựa trên kế hoạch điều tra. Khả năng bộ nhớ của giải pháp đảm bảo mỗi cuộc điều tra xây dựng kiến thức tổ chức và cung cấp trải nghiệm cá nhân hóa dựa trên vai trò và sở thích của người dùng.

Cuộc điều tra này thể hiện một số khả năng chính:

- **Tương quan nhiều nguồn** – Nó kết nối các vấn đề cấu hình cơ sở dữ liệu với suy giảm hiệu suất API
- **Điều tra tuần tự** – Các tác nhân làm việc có hệ thống thông qua kế hoạch điều tra trong khi cung cấp cập nhật trực tiếp
- **Ghi nhận nguồn** – Các phát hiện bao gồm công cụ cụ thể và nguồn dữ liệu
- **Thông tin có thể hành động** – Nó cung cấp dòng thời gian rõ ràng của các sự kiện và các bước phục hồi được ưu tiên
- **Phát hiện lỗi dây chuyền** – Nó có thể giúp cho thấy cách một lỗi lan truyền qua hệ thống

## Tác động kinh doanh

Các tổ chức triển khai hỗ trợ SRE được hỗ trợ bởi AI báo cáo cải thiện đáng kể trong các số liệu vận hành chính. Các cuộc điều tra ban đầu trước đây mất 30–45 phút giờ đây có thể được hoàn thành trong 5–10 phút, cung cấp cho SRE ngữ cảnh toàn diện trước khi đi sâu vào phân tích chi tiết. Sự giảm đáng kể này trong thời gian điều tra được chuyển trực tiếp thành giải quyết sự cố nhanh hơn và giảm thời gian ngừng hoạt động.

Giải pháp cải thiện cách SRE tương tác với cơ sở hạ tầng của họ. Thay vì điều hướng nhiều bảng điều khiển và công cụ, các kỹ sư có thể đặt câu hỏi bằng ngôn ngữ tự nhiên và nhận được thông tin tổng hợp từ các nguồn dữ liệu liên quan. Việc giảm chuyển đổi ngữ cảnh này cho phép các nhóm duy trì tập trung trong các sự cố quan trọng và giảm tải nhận thức trong các cuộc điều tra.

Có lẽ quan trọng nhất, giải pháp dân chủ hóa kiến thức trên toàn nhóm. Tất cả các thành viên trong nhóm có thể truy cập các kỹ thuật điều tra toàn diện giống nhau, giảm sự phụ thuộc vào kiến thức bộ lạc và gánh nặng trực tuyến. Phương pháp nhất quán được cung cấp bởi giải pháp đảm bảo các phương pháp điều tra vẫn thống nhất trên các thành viên trong nhóm và các loại sự cố, cải thiện độ tin cậy tổng thể và giảm khả năng bỏ lỡ bằng chứng.

Các báo cáo điều tra được tạo tự động cung cấp tài liệu có giá trị cho các đánh giá sau sự cố và giúp các nhóm học hỏi từ mỗi sự cố, xây dựng kiến thức tổ chức theo thời gian. Hơn nữa, giải pháp mở rộng các khoản đầu tư cơ sở hạ tầng AWS hiện có, làm việc cùng với các dịch vụ như Amazon CloudWatch, [AWS Systems Manager](https://aws.amazon.com/systems-manager/) và các công cụ vận hành AWS khác để cung cấp một hệ thống thông minh vận hành thống nhất.

## Mở rộng giải pháp

Kiến trúc mô-đun giúp việc mở rộng giải pháp cho nhu cầu cụ thể của bạn trở nên đơn giản.

Ví dụ, bạn có thể thêm các tác nhân chuyên biệt cho miền của bạn:

- **Tác nhân bảo mật** – Cho kiểm tra tuân thủ và phản ứng sự cố bảo mật
- **Tác nhân cơ sở dữ liệu** – Cho xử lý sự cố và tối ưu hóa cụ thể cơ sở dữ liệu
- **Tác nhân mạng** – Cho gỡ lỗi kết nối và cơ sở hạ tầng

Bạn cũng có thể thay thế các API demo bằng kết nối đến các hệ thống thực tế của bạn:

- **Tích hợp Kubernetes** – Kết nối với API cụm của bạn cho trạng thái pod, triển khai và sự kiện
- **Tổng hợp nhật ký** – Tích hợp với dịch vụ quản lý nhật ký của bạn (Elasticsearch, Splunk, CloudWatch Logs)
- **Nền tảng số liệu** – Kết nối với dịch vụ giám sát của bạn (Prometheus, Datadog, CloudWatch Metrics)
- **Kho lưu trữ sổ tay** – Liên kết với tài liệu vận hành và sổ tay của bạn được lưu trữ trong wiki, kho Git hoặc cơ sở kiến thức

## Dọn dẹp

Để tránh phát sinh chi phí trong tương lai, sử dụng script dọn dẹp để xóa các tài nguyên AWS có thể tính phí được tạo trong demo:

```bash
# Dọn dẹp hoàn chỉnh - xóa tài nguyên AWS và tệp cục bộ
./scripts/cleanup.sh
```

Script này tự động thực hiện các hành động sau:

- Dừng các máy chủ backend
- Xóa gateway và các mục tiêu của nó
- Xóa tài nguyên Amazon Bedrock AgentCore Memory
- Xóa Amazon Bedrock AgentCore Runtime
- Xóa các tệp được tạo (URI gateway, token, ARN tác nhân, ID bộ nhớ)

Để biết hướng dẫn dọn dẹp chi tiết, tham khảo [Cleanup Instructions](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/scripts/cleanup.sh).

## Kết luận

Tác nhân SRE thể hiện cách các hệ thống đa tác nhân có thể biến đổi phản ứng sự cố từ một quy trình thủ công, tốn thời gian thành một cuộc điều tra hiệu quả về thời gian, cộng tác, cung cấp cho SRE những thông tin họ cần để giải quyết các vấn đề nhanh chóng và tự tin.

Bằng cách kết hợp cơ sở hạ tầng cấp doanh nghiệp của Amazon Bedrock AgentCore với quyền truy cập công cụ tiêu chuẩn trong MCP, chúng tôi đã tạo ra một nền tảng có thể thích ứng khi cơ sở hạ tầng của bạn phát triển và các khả năng mới xuất hiện.

Triển khai hoàn chỉnh có sẵn trong [kho GitHub](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent) của chúng tôi, bao gồm các môi trường demo, hướng dẫn cấu hình và ví dụ mở rộng. Chúng tôi khuyến khích bạn khám phá giải pháp, tùy chỉnh nó cho cơ sở hạ tầng của bạn và chia sẻ kinh nghiệm của bạn với cộng đồng.

Để bắt đầu xây dựng trợ lý SRE của riêng bạn, tham khảo các tài nguyên sau:

- [Automate tasks in your application using AI agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)
- [Amazon Bedrock AgentCore Samples GitHub repository](https://github.com/aws-samples/amazon-bedrock-agentcore-samples)
- [Model Context Protocol documentation](https://modelcontextprotocol.io/)
- [LangGraph documentation](https://langchain-ai.github.io/langgraph/)

## Về các tác giả

![AMit Arora](/images/translated-blogs/blog1/pic3.jpg)
**Amit Arora** là Kiến trúc sư Chuyên gia AI và ML tại Amazon Web Services, giúp các khách hàng doanh nghiệp sử dụng các dịch vụ học máy dựa trên đám mây để mở rộng nhanh các đổi mới của họ. Ông cũng là giảng viên thỉnh giảng trong chương trình khoa học dữ liệu và phân tích MS tại Đại học Georgetown ở Washington, D.C.

![AMit Arora](/images/translated-blogs/blog1/pic4.jpg)
**Dheeraj Oruganty** là Tư vấn Giao hàng tại Amazon Web Services. Ông đam mê xây dựng các giải pháp AI Tạo sinh và Học máy sáng tạo tạo ra tác động kinh doanh thực sự. Chuyên môn của ông trải rộng từ Đánh giá AI Đại lý, Đo điểm chuẩn và Điều phối Đại lý, nơi ông tích cực đóng góp vào nghiên cứu thúc đẩy lĩnh vực này. Ông có bằng thạc sĩ về Khoa học Dữ liệu từ Đại học Georgetown. Ngoài công việc, ông thích nghiên cứu về xe hơi, xe máy và khám phá thiên nhiên.
