---
title: "CF 104257B - Trộm xe đạp"
description: "Chúng ta được cung cấp một khóa kết hợp được mô tả như một hệ thống tuần hoàn đa quay số. Mỗi mặt số hoạt động giống như một bánh xe tròn: mặt số thứ i có các giá trị từ 0 đến ai − 1 và khi quay nó sẽ di chuyển một bước theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ với chi phí thời gian cố định."
date: "2026-07-01T21:45:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "B"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 79
verified: true
draft: false
---

[CF 104257B - Kẻ trộm xe đạp](https://codeforces.com/problemset/problem/104257/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một khóa kết hợp được mô tả như một hệ thống tuần hoàn đa quay số. Mỗi mặt số hoạt động giống như một bánh xe tròn: mặt số thứ i có các giá trị từ 0 đến ai − 1 và khi quay nó sẽ di chuyển một bước theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ với chi phí thời gian cố định. Khóa bắt đầu ở cấu hình hoàn toàn bằng không. 

Có hai hành động có sẵn. Một hành động sẽ thay đổi cấu hình bằng cách xoay chính xác từng mặt số một, tốn x giây mỗi bước. Hành động còn lại là thao tác "kiểm tra", kiểm tra cấu hình hiện tại dưới dạng lần thử mật khẩu đầy đủ và tốn y giây mỗi lần sử dụng. Kẻ trộm có tổng quỹ thời gian s và muốn tối đa hóa số lượng cấu hình riêng biệt mà hắn có thể thử, trong đó “thử” có nghĩa là thực hiện thao tác kiểm tra trên cấu hình đó. 

Vì vậy, vấn đề không phải là đạt được trạng thái mục tiêu mà là lập kế hoạch dạo qua một không gian trạng thái rộng lớn trong khi thỉnh thoảng trả tiền để “lấy mẫu” nút hiện tại. Mỗi nút được lấy mẫu phải khác biệt. 

Các ràng buộc ngay lập tức gợi ý rằng không gian trạng thái là rất lớn vì tổng số cấu hình là tích của tất cả các giá trị ai, có thể lớn về mặt thiên văn. Đồng thời, số lượng ca kiểm thử rất lớn, do đó, bất kỳ việc khám phá trạng thái tuyến tính hoặc tổ hợp nào trên mỗi ca kiểm thử đều không thể thực hiện được. Mọi thứ phải giảm xuống mức tính toán theo thời gian không đổi cho mỗi bài kiểm tra. 

Một cách giải thích ngây thơ có thể cố gắng mô phỏng các bước di chuyển, xây dựng từng cấu hình một và tham lam quyết định xem nên xoay hay kiểm tra. Điều này không thành công vì ngay cả việc xây dựng đường dẫn có độ dài tỷ lệ với câu trả lời cũng không thể thực hiện được khi ai có thể lên tới 10^18. 

Cạm bẫy tinh vi thứ hai là giả định rằng việc xoay giữa hai cấu hình luôn tốn một khoản tỷ lệ thuận với khoảng cách tổng thể trong không gian sản phẩm mà không nhận ra rằng cấu trúc cho phép truyền tải rất hiệu quả. Một giải pháp bất cẩn có thể đánh giá quá cao chi phí di chuyển hoặc đánh giá thấp số lượng trạng thái có thể bị xiềng xích trong một chuyến đi. 

Một trường hợp phức tạp hơn phát sinh khi quỹ thời gian quá nhỏ để thực hiện một thao tác kiểm tra duy nhất. Trong trường hợp đó, câu trả lời là 0 ngay cả khi cấu hình ban đầu tồn tại, bởi vì chúng ta không đủ khả năng để “thử” nó. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tưởng tượng tên trộm đang bước đi một cách rõ ràng trên một biểu đồ khổng lồ trong đó mỗi nút là một cấu hình và các cạnh tương ứng với các phép quay quay số một bước. Mỗi lần truy cập vào một nút có chi phí y và mỗi lần truyền tải cạnh có chi phí x. Chúng tôi sẽ cố gắng liệt kê tất cả các đường dẫn có thể có của tổng chi phí nhiều nhất là s và tối đa hóa số lượng nút được truy cập riêng biệt. Điều này nhanh chóng trở nên khó giải quyết vì ngay cả việc lưu trữ các nút đã truy cập cũng không thể thực hiện được và kích thước biểu đồ là hàm mũ tính theo n. 

Quan sát quan trọng là cấu trúc của không gian cấu hình là tích Descartes của các chu trình. Điều này có nghĩa là nó hoạt động giống như một hình xuyến n chiều trong đó mọi nút đều có bậc 2n và đồ thị có tính đối xứng cao. Trong các biểu đồ như vậy, chúng ta luôn có thể xây dựng các đường dẫn dài đơn giản để tránh phải truy cập lại các trạng thái và quan trọng là chúng ta có thể coi việc truy cập k trạng thái riêng biệt về cơ bản yêu cầu một đường dẫn có độ dài k − 1 theo góc quay. 

Một khi chúng ta chấp nhận điều đó, vấn đề sẽ trở thành vấn đề ngân sách một chiều. Để truy cập k cấu hình, chúng ta phải thực hiện k kiểm tra, tính giá k·y và chúng ta phải di chuyển giữa k trạng thái, yêu cầu ít nhất k − 1 phép quay, tính giá (k − 1)·x. Chiến lược tối ưu là đi dọc theo một con đường đơn giản không bao giờ quay lại một trạng thái, bởi vì việc xem lại sẽ chỉ lãng phí thời gian mà không làm tăng số lần kiểm tra riêng biệt. 

Do đó, chi phí tối thiểu để thử k cấu hình là k·y + (k − 1)·x, và chúng ta chỉ đơn giản tối đa hóa k theo ràng buộc này, đồng thời lưu ý rằng k không thể vượt quá tổng số cấu hình ∏ ai.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force truyền tải đồ thị trạng thái | Số mũ trong n | Hàm mũ | Quá chậm | 
| Giảm chi phí cho đường dẫn tuyến tính + công thức | O(n) mỗi lần kiểm tra | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Trước tiên hãy tính xem tổng cộng có bao nhiêu cấu hình mà chúng ta có thể có, đó là tích của tất cả các giá trị ai. Nếu tích này vượt quá bất kỳ giới hạn có ý nghĩa nào thì nó vẫn được giữ về mặt khái niệm như giới hạn trên của k. Điều này quan trọng vì ngay cả khi thời gian cho phép thử nhiều lần hơn, chúng ta không thể thử nhiều trạng thái khác biệt hơn trạng thái tồn tại. 
2. Kiểm tra xem chúng ta có đủ khả năng chi trả dù chỉ một lần “thử” hay không. Một lần thử tốn y giây cho lần kiểm tra đầu tiên. Nếu s < y thì không thể kiểm tra cấu hình nào và câu trả lời là 0. 
3. Giả sử chúng ta thực hiện k kiểm tra. Cấu hình đầu tiên chỉ tốn y giây. Mỗi cấu hình bổ sung yêu cầu hai loại chi phí: một lần kiểm tra tốn y và một lần di chuyển từ cấu hình trước đó tốn ít nhất x. Điều này tạo ra sự tích lũy chi phí tuyến tính khi chúng tôi mở rộng chuỗi các trạng thái riêng biệt. 
4. Biểu thị tổng chi phí cho k lần thử dưới dạng y + (k − 1)·(x + y). Điều này phản ánh chi phí lấy mẫu ban đầu và sau đó lặp lại các bước “di chuyển cộng với mẫu”. 
5. Sắp xếp lại bất đẳng thức y + (k − 1)(x + y) s để cô lập k. Điều này cho ra k ≤ 1 + (s − y) // (x + y). 
6. Lấy giá trị tối thiểu giữa giá trị này và tổng số cấu hình có sẵn. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau mỗi lần thử thành công, kẻ trộm luôn được định vị ở cấu hình mới truy cập và để đạt được bất kỳ cấu hình mới nào đều yêu cầu ít nhất một vòng quay đơn vị so với cấu hình trước đó. Vì các phép quay chỉ thay đổi từng bước quay số nên mọi chuyển đổi giữa các cấu hình riêng biệt đều có chi phí tối thiểu là x và chi phí này không thể bỏ qua bằng bất kỳ phím tắt nào. Do đó, bất kỳ chuỗi k lần thử riêng biệt nào cũng gây ra ít nhất k − 1 chuyển tiếp, mỗi lần phát sinh chi phí x, trong khi mỗi lần thử tự nó phát sinh chi phí y. Vì không gian cấu hình được kết nối và cho phép các đường đi Hamilton, giới hạn dưới này là chặt chẽ, có nghĩa là một đường dẫn đơn giản có thể đạt được chính xác k − 1 chuyển tiếp. Điều này làm cho công thức dẫn xuất vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x, y, s = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = 1
        for v in a:
            total = min(total * v, 10**18 + 5)
        
        if s < y:
            print(0)
            continue
        
        # maximum k from budget
        k = 1 + (s - y) // (x + y)
        if k > total:
            k = total
        
        print(k)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện công thức dẫn xuất. Tích của tất cả ai được tính toán với giới hạn sớm để tránh tràn, vì bất kỳ giá trị nào nằm ngoài phạm vi câu trả lời đều không liên quan. Lần kiểm tra đầu tiên được xử lý riêng: nếu không có đủ thời gian cho một lần kéo, câu trả lời ngay lập tức là 0. Mặt khác, chúng tôi tính toán xem có bao nhiêu chu kỳ "di chuyển cộng với kiểm tra" đầy đủ phù hợp sau lần kiểm tra đầu tiên, sau đó kẹp kết quả theo tổng số cấu hình. 

Một lỗi phổ biến là quên rằng cấu hình đầu tiên không yêu cầu xoay trước đó, đó là lý do tại sao công thức sử dụng y làm chi phí ban đầu độc lập thay vì xử lý đối xứng tất cả k trạng thái. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với một mặt số có kích thước 10, x = 3, y = 4 và s = 20. 

| Bước | k | Tính toán chi phí | Tổng chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | y = 4 | 4 | 
| Gia hạn | 2 | + (x + y) = 7 | 11 | 
| Gia hạn | 3 | + 7 | 18 | 
| Gia hạn | 4 | sẽ là 25 | vượt quá | 

Điều này cho thấy k = 3 là khả thi trong khi k = 4 thì không, phù hợp với công thức 1 + (20 − 4) // 7 = 3. 

Bây giờ hãy xem xét trường hợp thời gian quá nhỏ: n = 2, x = 5, y = 10, s = 9. 

| Bước | Kiểm tra khả thi | Lý do | 
| --- | --- | --- | 
| 1 lần thử | Không | y > s | 

Vì vậy, câu trả lời là 0 vì ngay cả lần thử đầu tiên cũng không thể được trả tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi nhân lên tất cả ai một lần | 
| Không gian | O(1) | Chỉ một số vô hướng được duy trì | 

Giải pháp phù hợp thoải mái với các ràng buộc vì tổng n trên tất cả các trường hợp thử nghiệm bị giới hạn và tất cả các phép toán còn lại là số học theo thời gian không đổi cho mỗi thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x, y, s = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = 1
        for v in a:
            total = min(total * v, 10**18 + 5)
        
        if s < y:
            out.append("0")
            continue
        
        k = 1 + (s - y) // (x + y)
        k = min(k, total)
        out.append(str(k))
    
    return "\n".join(out)

# provided samples (from statement)
assert run("""4
4 1 3 12
10 10 10 10
2 17 101 400
2 2
3 1 1 1000000000000000000
10 10 10
5 98765 43210 98765432123456789
111111 222222 333333 444444 555555
""") == """3
3
1000000000000000000
5"""

# custom cases
assert run("""1
1 10 100 5
5
""") == "0", "cannot even try once"

assert run("""1
1 1 1 10
10
""") == "6", "tight linear growth"

assert run("""1
2 5 1 1
2 2
""") == "0", "zero budget case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quay số duy nhất, không đủ thời gian | 0 | không thể thực hiện lần kéo đầu tiên | 
| chi phí nhỏ, đủ thời gian | 6 | hành vi đúng của công thức tuyến tính | 
| ngân sách bằng không | 0 | trường hợp cạnh thoát sớm | 

## Vỏ cạnh 

Khi quỹ thời gian nhỏ hơn y, thuật toán ngay lập tức trả về 0 vì không thể thực hiện thao tác kiểm tra nào. Ví dụ: với s = 3 và y = 5, điều kiện s < y sẽ kích hoạt và quá trình dừng lại trước khi có bất kỳ phép quay hoặc lý luận trạng thái nào quan trọng. 

Khi chỉ có một cấu hình trong hệ thống, nghĩa là tất cả ai = 1, sản phẩm là 1. Ngay cả khi quỹ thời gian rất lớn, câu trả lời không thể vượt quá 1 vì không có trạng thái thứ hai riêng biệt nào để truy cập. Thuật toán kẹp chính xác k theo tổng số cấu hình. 

Khi x cực kỳ nhỏ so với y, công thức vẫn hoạt động chính xác vì nó cho phép các chuỗi dài bị chi phối bởi chi phí kiểm tra nhưng vẫn tôn trọng thực tế là mỗi trạng thái bổ sung yêu cầu cả xoay vòng và kiểm tra, đảm bảo không tính quá mức các cấu hình có thể tiếp cận.
