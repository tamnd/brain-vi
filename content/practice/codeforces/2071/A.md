---
title: "CF 2071A - Vở kịch không bao giờ kết thúc"
description: "Ba người chơi liên tục chơi các trận đấu trong đó có đúng hai người đang hoạt động và một người đang xem. Việc phân công vai trò sẽ tiến triển một cách xác định so với trận đấu trước đó bằng cách sử dụng hai quy tắc. Nếu ai đó đã chơi hai trận liên tiếp, họ buộc phải ngồi ngoài tiếp theo."
date: "2026-06-08T06:50:51+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2071
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1007 (Div. 2)"
rating: 800
weight: 2071
solve_time_s: 73
verified: true
draft: false
---

[CF 2071A - Cuộc chơi không bao giờ kết thúc](https://codeforces.com/problemset/problem/2071/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ba người chơi liên tục chơi các trận đấu trong đó có đúng hai người đang hoạt động và một người đang xem. Việc phân công vai trò sẽ tiến triển một cách xác định so với trận đấu trước đó bằng cách sử dụng hai quy tắc. Nếu ai đó đã chơi hai trận liên tiếp, họ buộc phải ngồi ngoài tiếp theo. Ngược lại, người thắng và khán giả trước sẽ trở thành người chơi của trận tiếp theo, còn người thua sẽ trở thành khán giả. 

Chúng tôi không được yêu cầu mô phỏng danh tính của người chơi. Điều quan trọng duy nhất là vị trí của một người chơi cụ thể: khán giả của trận đấu đầu tiên. Đối với mỗi chỉ số truy vấn k, chúng ta phải xác định xem chính người chơi này có thể là khán giả trong trận đấu thứ k trong bất kỳ diễn biến hợp lệ nào của trò chơi hay không. 

Các ràng buộc cho phép k lên tới 10^9, do đó, bất kỳ cách tiếp cận nào mô phỏng rõ ràng việc khớp từng bước đều không khả thi ngay lập tức. Mô phỏng tuyến tính sẽ yêu cầu tới một tỷ lần chuyển đổi cho mỗi trường hợp thử nghiệm, vượt xa giới hạn thời gian ngay cả đối với 1000 truy vấn. 

Cấu trúc ẩn quan trọng là mặc dù kết quả trận đấu đưa ra sự phân nhánh (vì người chiến thắng có thể khác nhau), vai trò của khán giả sẽ phát triển với một ràng buộc định kỳ cứng nhắc gây ra bởi quy tắc “không chơi ba lần liên tiếp”. Điều này buộc hệ thống rơi vào một chu kỳ ngắn đối với khán giả của trận đấu đầu tiên. 

Các trường hợp cạnh chủ yếu là về giá trị k nhỏ. Với k = 1 câu trả lời gần như là có. Với k = 2, khán giả của trận đấu đầu tiên không thể tiếp tục là khán giả vì có đúng hai đấu thủ phải thi đấu nên khán giả buộc phải vào trận ngay lập tức. Bất kỳ lý do nào bỏ qua sự tham gia bắt buộc này sẽ bị phá vỡ ngay lập tức tại k = 2. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các trạng thái trận đấu có thể xảy ra trong khi theo dõi xem khán giả ban đầu có còn giữ vai trò đó hay không. Mỗi trạng thái phụ thuộc vào hai hoặc ba cấu hình trận đấu cuối cùng và danh tính của khán giả. Bởi vì người chiến thắng không cố định nên điều này trở thành một quá trình phân nhánh có thể bùng nổ theo cấp số nhân về số lượng trận đấu. 

Ngay cả khi chúng tôi đơn giản hóa và chỉ theo dõi một chuỗi xác định duy nhất, chúng tôi vẫn cần tính toán tối đa k lần chuyển đổi cho mỗi truy vấn. Với k lên đến 10^9 thì điều này là không thể. 

Quan sát quan trọng là chúng tôi không cần theo dõi danh tính đầy đủ của tất cả người chơi. Chúng ta chỉ cần biết liệu một người chơi được chỉ định có thể đảm nhận vai trò khán giả tại thời điểm k trong ít nhất một lần tiến hóa hợp lệ hay không. Điều này làm giảm vấn đề phân tích khả năng tiếp cận trong một không gian trạng thái rất nhỏ được xác định bởi các mô hình tham gia gần đây. 

Quy tắc ngăn cản bất kỳ người chơi nào chơi ba trận liên tiếp buộc phải thay đổi vai trò định kỳ. Sau khi mở ra một số chuyển đổi đầu tiên, hệ thống ổn định thành mô hình lặp lại về sự tham gia của khán giả với giai đoạn 3. Kiểm tra trực tiếp các trình tự hợp lệ cho thấy khán giả của trận đấu 1 chỉ có thể xuất hiện lại với tư cách là khán giả ở các vị trí phù hợp với 1 modulo 3. 

Điều này chuyển đổi vấn đề từ mô phỏng sang số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Quan sát chu kỳ | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quan sát một vài trận đấu đầu tiên và chỉ theo dõi vai trò khán giả của khán giả trận đấu đầu tiên. Chúng tôi không theo dõi danh tính đầy đủ của người chơi, chỉ theo dõi xem người chơi đó có còn là khán giả hay bị buộc phải chơi hay không. 
2. Từ trận 1 đến trận 2, khán giả phải tham gia vì mỗi trận chỉ có một người chơi không hoạt động và các ràng buộc về vai trò buộc phải luân chuyển. Điều này loại bỏ k = 2 như một vị trí khán giả hợp lệ. 
3. Tiếp tục lý luận thêm vài bước nữa. Bởi vì bất kỳ người chơi nào chơi hai lần đều phải ngồi ngoài tiếp theo, hệ thống buộc phải thực hiện một vòng quay cứng nhắc trong đó mỗi người chơi luân phiên giữa tối đa hai lần tham gia liên tiếp, sau đó là buộc phải nghỉ giải lao. 
4. Khi mẫu này được mở rộng đủ các bước, chúng tôi quan sát thấy một cấu trúc lặp lại trong đó vai trò của khán giả sẽ quay vòng sau mỗi 3 trận đấu. 
5. Kết luận rằng khán giả của trận đấu đầu tiên xuất hiện đúng ở trận đấu k trong đó k ≡ 1 (mod 3). 

Tại sao nó hoạt động: trạng thái của hệ thống liên quan đến danh tính khán giả được xác định hoàn toàn bởi lịch sử giới hạn có độ dài 2 do quy tắc “không có ba trò chơi liên tiếp”. Điều này tạo ra một máy tự động hữu hạn chỉ có một vài trạng thái có thể tiếp cận được. Bất kỳ máy tự động hữu hạn nào trên một chuỗi vai trò đang phát triển cuối cùng đều phải trở thành định kỳ và phép liệt kê trực tiếp cho thấy khoảng thời gian là 3 với căn chỉnh ban đầu ở chỉ số 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    if k % 3 == 1:
        print("YES")
    else:
        print("NO")
```Việc thực hiện áp dụng trực tiếp kết quả định kỳ. Mỗi trường hợp thử nghiệm là độc lập và chỉ yêu cầu một thao tác modulo duy nhất. 

Điểm tinh tế duy nhất là sự liên kết. Trận đấu đầu tiên được lập chỉ mục là k = 1, và ở vị trí này, khán giả gần như là cùng một người chơi, do đó lớp đồng dư chính xác là 1 modulo 3 thay vì 0 modulo 3. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Với k = 1, 2, 333, 1e9, ta đánh giá k modulo 3. 

| k | k mod 3 | Kết quả | 
| --- | --- | --- | 
| 1 | 1 | CÓ | 
| 2 | 2 | KHÔNG | 
| 333 | 0 | KHÔNG | 
| 1000000000 | 1 | CÓ | 

Trường hợp đầu tiên xác nhận điều kiện cơ bản. Trường hợp thứ hai là buộc phải nghỉ thi đấu sau trận đầu tiên. Trường hợp thứ ba và thứ tư chứng minh cấu trúc tuần hoàn trên các chỉ số lớn, trong đó chỉ có vị trí môđun là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một lần kiểm tra modulo theo thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có trạng thái bổ sung ngoài các biến đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì t tối đa là 1000 và mỗi thao tác là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        k = int(input())
        out.append("YES" if k % 3 == 1 else "NO")
    return "\n".join(out)

# provided samples
assert run("4\n1\n2\n333\n1000000000\n") == "YES\nNO\nNO\nYES"

# minimum case
assert run("1\n1\n") == "YES"

# second position always fails
assert run("1\n2\n") == "NO"

# periodic checks
assert run("3\n3\n4\n5\n") == "NO\nYES\nNO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | trường hợp cơ sở k = 1 | 
| 2 | KHÔNG | ràng buộc chuyển tiếp ngay lập tức | 
| 3,4,5 | KHÔNG, CÓ, KHÔNG | tính nhất quán định kỳ | 

## Vỏ cạnh 

Với k = 1, khán giả gần như giống với khán giả ban đầu, vì vậy đầu ra là CÓ mà không áp dụng bất kỳ chuyển đổi nào. Quy tắc modulo chính xác mang lại 1 mod 3 = 1. 

Với k = 2, hệ thống buộc khán giả trước đó phải tham gia trò chơi tích cực, do đó câu trả lời phải là KHÔNG. Quy luật k% 3 = 2 khẳng định ngay điều này. 

Với k = 3, mặc dù có thể xảy ra nhiều diễn biến nhưng vai trò của khán giả đã chuyển khỏi khán giả ban đầu, vì vậy câu trả lời vẫn là KHÔNG. Điều kiện modulo k % 3 = 0 loại trừ chính xác nó. 

Với k = 4, chu trình sẽ thiết lập lại sự căn chỉnh, đưa hệ thống trở lại trạng thái mà người xem ban đầu có thể xuất hiện trở lại với tư cách là người xem, khớp với k % 3 = 1.
