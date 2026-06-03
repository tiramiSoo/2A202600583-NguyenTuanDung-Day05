# WORKSHOP: MỔ APP AI THẬT — MOMO MONI
*Trắc nghiệm tính năng: Trợ thủ tài chính, phân tích chi tiêu, chatbot*

---

## 1. Dùng thử: Promise vs Reality

* **Product hứa gì?** Trợ lý thông minh tự động theo dõi, phân tích chi tiêu và đóng vai trò như một "trợ thủ tài chính" tương tác bằng ngôn ngữ tự nhiên.
* **User nào được hứa sẽ được giúp?** Người dùng muốn quản lý tài chính cá nhân một cách thông minh, cần hỏi đáp nhanh để kiểm soát dòng tiền mà không muốn tự lục tìm lịch sử giao dịch[cite: 2].
* **Kỳ vọng vào AI:** Không chỉ báo cáo số tổng, AI phải có khả năng truy vấn sâu và liệt kê chi tiết các giao dịch khi được hỏi (ví dụ: *"Tháng này mình đã chi bao nhiêu tiền ăn?"* -> Trả ra tổng tiền và danh sách các quán ăn đã chi trả).
* **Điểm gãy khi dùng thật (Evidence):**[cite: 2]
  * **Input/Prompt đã thử:** Hỏi chatbot Moni về chi tiết các giao dịch cụ thể trong tháng (ví dụ: *"Liệt kê cho mình các khoản đã chi cho cà phê tuần này"*).
  * **Hành vi quan sát được:** Moni chỉ tổng hợp và trả ra được tổng số tiền đã chi tiêu chung chung (ví dụ: *"Bạn đã chi X đồng"*), nhưng khi người dùng yêu cầu xem chi tiết các giao dịch cấu thành nên số tiền đó thì AI không cung cấp được.
  * **Điểm gãy UX:** Phá vỡ lời hứa về một "trợ lý thông minh". User nhận được số tổng nhưng nếu muốn kiểm tra lại xem AI có tính toán sai hay không thì buộc phải thoát màn hình chat, vào mục Lịch sử giao dịch của MoMo để tự lọc và đối chiếu thủ công.

---

## 2. Vẽ 4 Paths & Đặt trên Automation Ladder

Hiện tại, tính năng này đang dừng lại ở **Level 2: Hỗ trợ thấp (Assisted)** trên *Automation Ladder* vì AI mới chỉ làm được phần bề nổi (tính tổng), còn phần việc nặng (truy vấn chi tiết) vẫn bắt con người phải tự làm thủ công.

Để sửa lỗi này, luồng tính năng cần được thiết kế lại lên **Level 3: Cộng tác (Human-in-the-loop)** với 4 Paths rõ ràng như sau[cite: 2]:

### 🟢 Happy Path
* **Khi AI đúng và tự tin:** User hỏi tổng chi tiêu và chi tiết. AI hiển thị số tổng kèm theo một danh sách thu gọn (List View) chứa tối đa 3-5 giao dịch chi tiết gần nhất ngay trong ô chat[cite: 2].

### 🟡 Low-confidence Path
* **Khi AI không chắc chắn:** User hỏi một câu mơ hồ như *"Tuần này mình tiêu gì nhiều nhất?"*. AI tính ra được số tổng nhưng không chắc user muốn xem chi tiết theo danh mục (Ăn uống, Mua sắm) hay theo dòng thời gian[cite: 2].
* **Giải pháp hệ thống:** Moni hiển thị số tổng kèm câu hỏi: *"Mình thấy bạn chi nhiều nhất cho Ăn uống (X đồng). Bạn có muốn xem chi tiết danh sách các quán bạn đã ăn tuần này không?"* kèm nút `[Xem chi tiết]`[cite: 2].

### 🔴 Failure Path
* **Khi AI sai/Không thể phản hồi:** Hệ thống gặp lỗi đồng bộ dữ liệu hoặc không thể bóc tách dữ liệu chi tiết từ ví để hiển thị trong phòng chat[cite: 2].
* **User biết bằng cách nào:** Thay vì im lặng hoặc báo lỗi hệ thống chung chung, Moni trả ra số tổng kèm theo một nút bấm cứu cánh trực quan: `[Xem chi tiết trong Lịch sử giao dịch ➡️]`[cite: 2].

### 🔵 Correction Path
* **Khi user tương tác/sửa lỗi:** Nếu danh sách chi tiết AI liệt kê bị thiếu hoặc user muốn xem đầy đủ hơn, user bấm vào nút cứu cánh `[Xem tất cả]`. Hệ thống sẽ điều hướng (Deep-link) thẳng user tới màn hình Lịch sử giao dịch đã được **tự động lọc sẵn** theo đúng mốc thời gian và từ khóa mà user vừa hỏi trong chat[cite: 2]. Hành vi này được lưu log lại để tối ưu tốc độ gọi API dữ liệu lần sau[cite: 2].

---

## 3. Viết Finding thành Quyết định Sản phẩm

> **Khi user** hỏi chatbot Moni chi tiết về các giao dịch cấu thành nên một khoản chi cụ thể[cite: 2],
>
> **AI/Product** chỉ trả ra được duy nhất con số tổng hợp mà không thể liệt kê dữ liệu chi tiết[cite: 2],
>
> **Hậu quả là** trải nghiệm bị đứt gãy, user không thể kiểm chứng tính chính xác của số liệu, cảm thấy AI "vô dụng" và phải quay lại cách làm truyền thống là tự mò lịch sử giao dịch[cite: 2].
>
> **Lỗi thuộc layer:** Data-Tool (Mô hình AI thiếu kết nối/quyền truy cập vào API lịch sử giao dịch chi tiết hoặc thiếu component hiển thị danh sách)[cite: 2].
>
> **Nên sửa bằng:** Nâng cấp hạ tầng kết nối dữ liệu. Cung cấp cho LLM công cụ (Function Calling) để gọi API lịch sử giao dịch theo tham số (người nhận, thời gian, số tiền) và thiết kế thêm UI Component dạng danh sách cuộn (Carousel/List View) ngay trong màn hình chat[cite: 2].

---

## 4. Sketch As-is / To-be Workflow

### Sơ đồ luồng hiện tại (As-is)
[User hỏi: "Tuần này mình chuyển tiền cho những ai?"]
│
▼
[AI Moni chỉ tính toán và trả ra: "Bạn đã chuyển tổng cộng X đồng."]
│
❌ (Điểm gãy: Không có danh sách chi tiết, không có nút bấm chuyển hướng)[cite: 2]
│
▼
[User phải thoát chat -> Vào Lịch sử -> Tự cài bộ lọc ngày -> Tự tìm kiểm] -> (UX tệ)


### Sơ đồ luồng đề xuất (To-be)
[User hỏi: "Tuần này mình chuyển tiền cho những ai?"]
│
▼
[AI gọi Tool/API Lịch sử giao dịch] -> [Tính ra số tổng + Trích xuất 3 giao dịch lớn nhất]
│
▼
[Moni hiển thị kết quả trong Chatbox]:
"Tổng cộng: X đồng. Chi tiết gồm:

Chuyển khoản cho A: Y đồng

Thanh toán quán B: Z đồng
[Xem tất cả giao dịch tuần này ➡️]" (Deep-link sang trang Lịch sử có sẵn bộ lọc)
│
✔️ (User nắm trọn thông tin ngay tại chỗ hoặc chuyển hướng đúng mục tiêu trong 1 chạm)[cite: 2]


---

## 5. Thay đổi trong tài liệu SPEC

* **Quyết định thay đổi trong SPEC:**[cite: 2]
  * Bổ sung tính năng **Function Calling / Tool Use** cho Chatbot: Cho phép AI nhận diện các Intent liên quan đến "chi tiết", "liệt kê", "danh sách" để kích hoạt lệnh truy vấn xuống Database lịch sử giao dịch.
  * Bổ sung cấu trúc dữ liệu đầu ra (Output Format): Định nghĩa lại cấu trúc câu trả lời của AI bao gồm `summary_amount` (số tổng) và một mảng `transaction_list` (tối đa 5 item chi tiết chứa: tên, ngày, số tiền).
  * Thêm component hiển thị mới trên UI Chat: `Transaction-List-Card` kèm nút bấm Deep-lin