---
title: "CF 105437A - Cắt thành các bộ phận"
description: "Chúng tôi được cấp một tờ giấy duy nhất và chúng tôi liên tục chia nó ra bằng các đường cắt thẳng có chiều dài đầy đủ. Mọi vết cắt đều đi từ trái sang phải trên toàn bộ trang tính hiện tại hoặc từ trên xuống dưới trên toàn bộ trang tính hiện tại."
date: "2026-06-23T03:40:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "A"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 89
verified: true
draft: false
---

[CF 105437A - Cắt thành các bộ phận](https://codeforces.com/problemset/problem/105437/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tờ giấy duy nhất và chúng tôi liên tục chia nó ra bằng các đường cắt thẳng có chiều dài đầy đủ. Mọi vết cắt đều đi từ trái sang phải trên toàn bộ trang tính hiện tại hoặc từ trên xuống dưới trên toàn bộ trang tính hiện tại. Chi tiết quan trọng là các vết cắt luôn trải dài trên toàn bộ tờ giấy, do đó chúng cắt mọi phần hiện có mà chúng giao nhau. 

Sau khi thực hiện một số chuỗi cắt ngang và dọc, tấm giấy sẽ trở thành một mạng lưới gồm các mảnh hình chữ nhật. Câu hỏi đặt ra là cần bao nhiêu lần cắt tổng số để số mảnh thu được chính xác$n$. 

Đầu vào cho một số nguyên duy nhất$n$, và chúng ta phải xuất ra số lần cắt toàn bộ tờ giấy tối thiểu cần thiết sao cho số mảnh cuối cùng bằng$n$. 

Ràng buộc$n \le 10^6$ngay lập tức loại trừ bất kỳ điều gì cố gắng mô phỏng trình tự cắt hoặc tìm kiếm trên cấu hình. Bất kỳ cách tiếp cận nào khám phá ngay cả một phần nhỏ của tất cả các cách để phân chia lưới sẽ bùng nổ vì số lượng phân phối cắt có thể tăng lên kết hợp với số lượng cắt. 

Một điểm tinh tế là mỗi lần cắt sẽ tăng số lượng mảnh theo cấp số nhân tùy theo trạng thái hiện tại chứ không chỉ cộng thêm. Đây chính là lý do khiến cho lý luận trở nên ngây thơ về việc “cứ chia nhỏ cho đến khi đạt được$n$“không đáng tin cậy. 

Một số trường hợp đặc biệt giúp làm rõ những gì dễ hiểu sai. Nếu$n = 2$, một lần cắt là đủ: một lần cắt ngang hoặc dọc sẽ tạo ra chính xác hai mảnh. Nếu như$n = 3$, hai vết cắt là đủ, ví dụ hai vết cắt dọc chia thành ba dải. Nếu như$n = 100$, một cấu trúc đối xứng tồn tại trong đó các vết cắt lặp đi lặp lại dọc theo cả hai hướng tạo ra lưới 10 x 10 sử dụng tổng số 20 vết cắt. 

Một sai lầm phổ biến là cho rằng chúng ta có thể thêm các phần cắt một cách tham lam và luôn kiểm soát số lượng các mảnh một cách tuyến tính. Điều đó không thành công vì khi cả hai hướng được sử dụng, mọi lần cắt mới sẽ nhân lên trên các phân đoạn hiện có. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy luận trực tiếp, chúng ta có thể nghĩ đến việc duy trì một mạng lưới. Giả sử chúng ta làm$h$vết cắt ngang và$v$vết cắt dọc. Mỗi lần cắt ngang làm tăng số dải ngang lên một, vì vậy sau tất cả các lần cắt ngang chúng ta có$h+1$hàng. Tương tự, chúng tôi nhận được$v+1$cột từ các vết cắt dọc. Mỗi hàng giao nhau với mỗi cột nên tổng số mảnh là$(h+1)(v+1)$. 

Điều này làm giảm vấn đề từ quy trình cắt động sang vấn đề phân tích nhân tử thuần túy: chúng tôi muốn biểu diễn$n$là tích của hai số nguyên dương, mỗi số ít nhất bằng 1 và cực tiểu hóa$(h+v)$Ở đâu$h+1$Và$v+1$là những yếu tố đó. 

Cho phép$a = h+1$Và$b = v+1$. Sau đó$ab = n$, và số lần cắt trở thành$h+v = a+b-2$. Vì vậy, nhiệm vụ trở thành tìm một cặp nhân tố của$n$điều đó giảm thiểu$a+b$. 

Ý tưởng mạnh mẽ là thử tất cả các cặp$a, b$như vậy$a \cdot b = n$, tính toán$a+b$, và lấy mức tối thiểu. Điều này đúng vì mọi chiến lược cắt hợp lệ đều tương ứng chính xác với một số hệ số của$n$. Vấn đề là hiệu suất: kiểm tra tất cả các ước số cho đến$n$sẽ tốn kém$O(n)$, quá chậm đối với$10^6$. Tuy nhiên, chúng ta chỉ cần kiểm tra các ước số đến$\sqrt{n}$, vì các cặp thừa số phản ánh xung quanh căn bậc hai. 

Cái nhìn sâu sắc quan trọng là các giải pháp tối ưu ưu tiên các yếu tố cân bằng. Đối với một sản phẩm cố định, tổng được giảm thiểu khi các thừa số càng gần nhau càng tốt, vì vậy chúng ta chỉ cần quét các ước số có thể có tối đa$\sqrt{n}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu (quét hệ số sqrt) | O(\sqrt{n}) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các số nguyên$i$từ 1 đến$\lfloor \sqrt{n} \rfloor$. Chúng ta chỉ xét đến căn bậc hai vì mọi thừa số lớn hơn$\sqrt{n}$có một yếu tố bổ sung đã được nhìn thấy trong phạm vi nhỏ hơn. 
2. Đối với mỗi$i$, kiểm tra xem$i$chia rẽ$n$. Nếu không, hãy tiếp tục đến ứng viên tiếp theo vì nó không thể tạo thành kích thước lưới hợp lệ. 
3. Nếu$i$chia rẽ$n$, tính hệ số cặp$j = n / i$. Điều này mang lại sự phân tách hợp lệ$n = i \cdot j$, tương ứng với một lưới có$i$hàng và$j$cột (hoặc ngược lại). 
4. Chuyển đổi chúng thành các vết cắt: các hàng yêu cầu$i-1$cắt ngang và cột yêu cầu$j-1$vết cắt dọc. Tổng chi phí cho cặp yếu tố này là$i + j - 2$. 
5. Theo dõi giá trị tối thiểu của$i + j - 2$trên tất cả các cặp yếu tố hợp lệ. Điều này thể hiện cấu hình phân chia tốt nhất có thể. 
6. Xuất ra chi phí tối thiểu tìm được. 

### Tại sao nó hoạt động 

Mỗi chuỗi cắt ngang và dọc đều tạo ra một lưới. Số hàng được xác định hoàn toàn bằng vết cắt ngang và số cột được xác định bằng vết cắt dọc, không phụ thuộc vào thứ tự. Do đó, bất kỳ cấu hình cuối cùng nào cũng tương ứng duy nhất với một cặp$(a, b)$với$ab = n$. Vì tất cả các cặp như vậy được liệt kê thông qua việc quét số chia nên chúng tôi không bao giờ bỏ lỡ cấu hình hợp lệ. Việc giảm thiểu việc cắt giảm trên tất cả các cặp đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

best = float('inf')

i = 1
while i * i <= n:
    if n % i == 0:
        j = n // i
        best = min(best, i + j - 2)
    i += 1

print(best)
```Mã trực tiếp thực hiện việc giải thích cặp yếu tố. Vòng lặp kết thúc$i$liệt kê số lượng hàng tiềm năng, trong khi$j$là số cột tương ứng. Mỗi cặp ước số hợp lệ được chuyển đổi thành số lần cắt bằng cách sử dụng$i + j - 2$, xuất phát từ việc trừ đi một vết cắt trên mỗi chiều để chuyển các mảnh thành vết cắt. 

Một chi tiết tinh tế đang khởi tạo`best`đến vô cùng để mọi cặp nhân tố hợp lệ đều được xem xét đúng đắn. Một điểm quan trọng khác là chỉ sử dụng phép chia số nguyên sau khi xác nhận tính chia hết, điều này tránh được các cặp không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 8$Chúng tôi kiểm tra tất cả các ước số lên đến$\sqrt{8}$. 

| tôi | chia 8 | j = 8/i | tôi + j - 2 | 
| --- | --- | --- | --- | 
| 1 | vâng | 8 | 7 | 
| 2 | vâng | 4 | 4 | 
| 3 | không | - | - | 

Tối thiểu là 4. 

Điều này tương ứng với lưới 2 x 4, đạt được bằng 1 lần cắt ngang và 3 lần cắt dọc hoặc ngược lại. Dấu vết cho thấy giải pháp tốt nhất tránh được sự mất cân bằng cực độ như 1 x 8, đòi hỏi phải cắt giảm nhiều hơn. 

### Ví dụ 2:$n = 3$| tôi | chia 3 | j | tôi + j - 2 | 
| --- | --- | --- | --- | 
| 1 | vâng | 3 | 2 | 

Hệ số hợp lệ duy nhất là 1 x 3, cho hai lần cắt. Điều này chứng tỏ rằng khi$n$là số nguyên tố, một chiều buộc phải bằng 1, do đó tất cả các vết cắt phải đi theo một hướng duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(\sqrt{n}) | Chúng tôi kiểm tra tất cả các ước số tiềm năng cho đến căn bậc hai của n | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Sự ràng buộc$n \le 10^6$làm cho$\sqrt{n} \le 1000$, do đó vòng lặp chạy tối đa khoảng một nghìn lần lặp, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    best = float('inf')
    i = 1
    while i * i <= n:
        if n % i == 0:
            j = n // i
            best = min(best, i + j - 2)
        i += 1
    return str(best)

# provided samples
assert run("8\n") == "4", "sample 1"
assert run("3\n") == "2", "sample 2"
assert run("100\n") == "18", "sample 3"

# custom cases
assert run("2\n") == "1", "minimum non-trivial case"
assert run("1\n") == "0", "degenerate single piece (not in constraints but boundary reasoning)"
assert run("16\n") == "6", "square number checks balance"
assert run("999983\n") == str(999983 - 1), "prime large number forces linear split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | phép chia hợp lệ nhỏ nhất | 
| 1 | 0 | logic phân tích biên | 
| 16 | 6 | trường hợp hệ số vuông cân bằng | 
| 999983 | 999982 | trường hợp xấu nhất số nguyên tố lớn | 

## Vỏ cạnh 

cho$n = 2$, cặp nhân tố duy nhất là$1 \cdot 2$. Thuật toán kiểm tra$i = 1$, tìm thấy$j = 2$, và tính toán$1 + 2 - 2 = 1$. Điều này phù hợp với vết cắt duy nhất cần thiết để chia thành hai phần. 

Đối với một số nguyên tố như$n = 3$hoặc một số nguyên tố lớn gần$10^6$, không tồn tại ước số nào khác ngoài 1. Vòng lặp chỉ kích hoạt tại$i = 1$, buộc cấu hình một hàng và$n$cột. Kết quả tính toán trở thành$n - 1$, tương ứng với việc xếp chồng tất cả các vết cắt theo một hướng, phù hợp với việc không thể hình thành một lưới cân bằng. 

Đối với các hình vuông hoàn hảo như$n = 100$, số chia$i = 10$tạo ra một cấu hình đối xứng. Thuật toán đánh giá cả$10 \times 10$và các cặp bất đối xứng khác, nhưng cặp đối xứng giảm thiểu$i + j - 2$, cho kết quả 18. Điều này xác nhận rằng việc tìm kiếm trên các ước số đã nắm bắt chính xác điểm cân bằng tối ưu.
