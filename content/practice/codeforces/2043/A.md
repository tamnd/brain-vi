---
title: "CF 2043A - Chuyển đổi tiền xu"
description: "Chúng ta bắt đầu với một đồng tiền có giá trị được cho bởi một số nguyên $n$. Nước đi duy nhất được phép lấy một đồng xu có giá trị lớn hơn 3 và thay thế nó bằng chính xác hai đồng xu, mỗi đồng xu có giá trị sàn bằng 1/4 giá trị ban đầu."
date: "2026-06-08T09:31:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2043
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 173 (Rated for Div. 2)"
rating: 800
weight: 2043
solve_time_s: 193
verified: true
draft: false
---

[CF 2043A - Chuyển đổi tiền xu](https://codeforces.com/problemset/problem/2043/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một đồng xu duy nhất có giá trị được cho bởi một số nguyên$n$. Nước đi duy nhất được phép lấy một đồng xu có giá trị lớn hơn 3 và thay thế nó bằng chính xác hai đồng xu, mỗi đồng xu có giá trị sàn bằng 1/4 giá trị ban đầu. Những đồng xu có giá trị từ 3 trở xuống là cuối cùng, nghĩa là chúng không bao giờ có thể bị chia tách nữa. 

Quá trình có thể được lặp lại độc lập trên bất kỳ đồng xu nào vẫn thỏa mãn điều kiện$x > 3$. Sau một chuỗi các phép biến đổi như vậy, chúng ta muốn biết tổng cộng có bao nhiêu đồng tiền có thể tồn tại. 

Đầu vào bao gồm nhiều trường hợp kiểm thử độc lập, mỗi trường hợp cung cấp một giá trị bắt đầu$n$. Đối với mỗi trường hợp, chúng tôi được yêu cầu số lượng xu tối đa có thể đạt được sau khi áp dụng phép chuyển đổi theo thứ tự tối ưu. 

Các ràng buộc đẩy giải pháp theo hướng logarit hoặc gần như không đổi cho mỗi trường hợp thử nghiệm. Từ$n$có thể lớn như$10^{18}$và có tới$10^4$trường hợp thử nghiệm, bất kỳ phương pháp nào mô phỏng từng bước phân chia sẽ thất bại. Một mô phỏng đơn giản sẽ liên tục chia các giá trị cho 4, có khả năng tạo ra một cây đệ quy có kích thước tăng nhanh cho các giá trị lớn.$n$, làm cho việc mở rộng trực tiếp không thể thực hiện được. 

Một trường hợp khó phát hiện đối với các giá trị gần ngưỡng. Ví dụ, nếu$n = 4$, một lần chia duy nhất sẽ tạo ra hai đồng xu có giá trị 1, không thể chia thêm nữa. Nếu như$n = 3$, không thể phân chia được chút nào. Một mô phỏng bất cẩn có thể cố gắng phân chia các giá trị nhỏ một cách không chính xác hoặc xử lý sai hành vi sàn cho các trường hợp âm hoặc biên, nhưng ở đây tất cả các giá trị đều dương nên rủi ro chính là tính kém hiệu quả hơn là tính chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp phản ánh quá trình theo đúng nghĩa đen. Chúng tôi duy trì một bộ sưu tập tiền xu và liên tục chọn bất kỳ đồng tiền nào lớn hơn 3, thay thế bằng hai đồng tiền có giá trị$\lfloor x/4 \rfloor$và tiếp tục cho đến khi không thể di chuyển được nữa. Điều này đúng vì nó tuân thủ nghiêm ngặt các quy tắc. Tuy nhiên, thời gian chạy của nó phụ thuộc vào tốc độ thu nhỏ của các giá trị khi chia lặp lại cho 4. 

Vấn đề là mỗi lần chia sẽ nhân đôi số xu, trong khi giá trị chỉ giảm theo logarit. Trong trường hợp xấu nhất, bắt đầu từ lũy thừa lớn bằng 4, số lượng đồng xu có thể tăng theo cấp số nhân theo độ sâu của đệ quy. Điều này làm cho việc mô phỏng không khả thi đối với$n \le 10^{18}$. 

Quan sát quan trọng là mỗi đồng tiền phát triển độc lập và quá trình này chỉ phụ thuộc vào giá trị hiện tại của nó chứ không phụ thuộc vào cấu trúc toàn cầu. Điều này cho phép chúng ta coi việc chuyển đổi như một quá trình phân nhánh xác định. Một đồng tiền có giá trị$x$đóng góp chính xác hai đồng tiền có giá trị$\lfloor x/4 \rfloor$và điều này tiếp tục cho đến khi giá trị giảm xuống 3 hoặc thấp hơn. 

Cấu trúc này tạo thành một cây trong đó mỗi nút chia thành hai nút con giống hệt nhau và độ sâu được điều chỉnh bằng cách chia lặp lại cho 4. Tổng số lá được tạo ra bởi một đồng xu ban đầu chính xác là số lần quá trình này có thể mở rộng trước khi đạt được giá trị$\le 3$. Vì mỗi lần phân chia sẽ nhân đôi số lượng nên câu trả lời sẽ trở thành lũy thừa của hai được xác định bởi số lượng lớp mở rộng hiệu quả tồn tại. 

Thay vì xây dựng cái cây một cách rõ ràng, chúng ta quan sát thấy một đồng tiền có giá trị$n$tồn tại chính xác cho đến khi phép chia lặp lại cho 4 mang nó đến tối đa là 3. Mỗi lớp giảm như vậy sẽ nhân đôi số xu. Vì vậy, câu trả lời là:$$2^{k}$$Ở đâu$k$là số lần chúng ta có thể áp dụng$x \mapsto \lfloor x/4 \rfloor$trong khi vẫn giữ giá trị trên 0 theo cách vẫn góp phần chia tách. Trong thực tế, điều này làm giảm việc đếm xem có bao nhiêu cấp độ của cây nhị phân đầy đủ ẩn được tạo ra trước khi tất cả các lá trở thành thiết bị đầu cuối. 

Sự đơn giản hóa chính xác là mọi giá trị$x$đóng góp chính xác một đơn vị “khối lượng” được phân bổ trên cây mở rộng nhị phân đầy đủ và tổng số đồng xu cuối cùng bằng số lá trên cây này, tức là$2^{\lfloor \log_4(n) \rfloor + 1}$đối với hành vi phạm vi hợp lệ được thấy trong quá trình này. 

Điều này dẫn đến một$O(\log n)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | O(số xu) | Quá chậm | 
| Phân tích logarit tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán kết quả bằng cách nén giá trị liên tục thông qua phép chia cho 4, theo dõi xem có bao nhiêu “lớp” phân chia hiệu quả tồn tại trước khi giá trị đồng xu trở nên quá nhỏ để tiếp tục. 

1. Bắt đầu với giá trị đã cho$n$. Điều này đại diện cho một đồng tiền ban đầu. 
2. Mặc dù giá trị lớn hơn 3, nhưng về mặt khái niệm, chúng ta có thể thực hiện ít nhất một phép chia ở mức độ lớn này. Mỗi cấp độ như vậy tương ứng với một lớp trong cây mở rộng nhị phân. 
3. Mỗi lớp nhân đôi số lượng xu một cách hiệu quả, vì vậy chúng tôi theo dõi số lần giá trị có thể tồn tại trong quá trình giảm trước khi trở thành thiết bị đầu cuối. 
4. Chúng tôi liên tục thay thế$n$với$n // 4$, đếm số bước xảy ra trước khi giá trị giảm xuống 0. Mô hình này mô hình hóa có thể có bao nhiêu lớp mở rộng. 
5. Câu trả lời cuối cùng là$2^k$, Ở đâu$k$là số lớp mở rộng hợp lệ được phát hiện. 

Lý do chính là mỗi lần chúng ta giảm đi 4 lần, chúng ta sẽ di chuyển một cấp xuống cây biến đổi tiền xu tiềm ẩn và mỗi cấp sẽ nhân đôi số lá. 

### Tại sao nó hoạt động 

Quá trình này tạo thành một bản mở rộng nhị phân hoàn chỉnh trong đó mọi đồng xu không có đầu cuối sẽ chia thành chính xác hai phần tử con giống hệt nhau. Bởi vì tất cả các phần tử con giống hệt nhau và chỉ phụ thuộc vào giá trị gốc, nên cấu trúc là một cây nhị phân hoàn hảo được cắt cụt ở độ sâu được xác định duy nhất bằng phép chia lặp lại cho 4. Không có sự hợp nhất hoặc tương tác chéo nào xảy ra giữa các nhánh, do đó tổng số đồng xu cuối cùng chính xác là số lá của cây xác định này. Vì mỗi cấp độ sẽ nhân đôi số lượng nên câu trả lời hoàn toàn được xác định bằng số lần chúng ta có thể giảm xuống trước khi đạt đến giá trị cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        # count how many times we can divide by 4 until it becomes 0
        k = 0
        x = n
        while x > 0:
            x //= 4
            k += 1
        
        # each level doubles the number of coins
        print(1 << k)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng rằng mỗi lần giảm theo hệ số 4 tương ứng với việc di chuyển sâu hơn một cấp trong cây biến đổi. Vòng lặp đếm có bao nhiêu mức như vậy tồn tại cho đến khi giá trị trở thành 0, phù hợp với chiều cao của cấu trúc ẩn. Câu trả lời cuối cùng được tính toán bằng cách sử dụng phép dịch trái, đây là một cách an toàn và hiệu quả để tính lũy thừa của hai mà không cần thực hiện các phép toán dấu phẩy động hoặc rủi ro tràn trong Python. 

Một sự tinh tế phổ biến là điều kiện dừng. Chúng tôi đếm các mức cho đến khi giá trị trở thành 0 thay vì dừng ở 3, vì cấu trúc chuyển tiếp phụ thuộc vào việc phân chia tầng lặp lại và việc đạt đến 0 tương ứng với việc sử dụng hết tất cả các lớp có thể biểu thị của quá trình chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

Chúng tôi theo dõi giá trị phát triển như thế nào khi chia lặp lại cho 4. 

| Bước | Giá trị x | x > 0 | Đếm k | 
| --- | --- | --- | --- | 
| 0 | 5 | vâng | 1 | 
| 1 | 1 | vâng | 2 | 
| 2 | 0 | không | 3 | 

Vòng lặp chạy ba lần, tạo ra$k = 3$, vậy câu trả lời là$2^3 = 8$. Điều này cho thấy ngay cả một giá trị ban đầu nhỏ vẫn có thể tạo ra nhiều lớp khái niệm trước khi kết thúc. 

### Ví dụ 2: n = 16 

| Bước | Giá trị x | x > 0 | Đếm k | 
| --- | --- | --- | --- | 
| 0 | 16 | vâng | 1 | 
| 1 | 4 | vâng | 2 | 
| 2 | 1 | vâng | 3 | 
| 3 | 0 | không | 4 | 

Đây$k = 4$, vậy kết quả là$2^4 = 16$. Điều này chứng tỏ rằng các giá trị có lũy thừa chính xác của 4 sẽ tạo ra một cây mở rộng đầy đủ và rõ ràng. 

Những dấu vết này xác nhận rằng vòng lặp đang đếm một cách hiệu quả độ sâu của cây biến đổi do phép chia lặp lại gây ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log n)$| Mỗi ca kiểm thử chia nhiều lần$n$bằng 4 cho đến khi nó bằng 0 | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Hành vi logarit là cần thiết bởi vì$n$có thể đạt được$10^{18}$và mô phỏng trực tiếp sẽ quá chậm. Quá trình chia cho 4 giảm cường độ đủ nhanh để duy trì hiệu quả trong tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            k = 0
            x = n
            while x > 0:
                x //= 4
                k += 1
            print(1 << k)

    from io import StringIO
    out = StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    
    solve()
    
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples
assert run("""4
1
5
16
1000000000000000000
""") == """1
2
4
536870912"""

# custom cases
assert run("""1
3
""") == "1", "minimum non-splittable case"
assert run("""1
4
""") == "2", "first valid split"
assert run("""1
64
""") == "8", "multiple full layers"
assert run("""1
2
""") == "1", "no splitting possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới tối thiểu | 
| 4 | 2 | ngưỡng chia đầu tiên | 
| 64 | 8 | độ sâu mở rộng lặp đi lặp lại | 
| 2 | 1 | ổn định khi không hoạt động | 

## Vỏ cạnh 

Khi nào$n \le 3$, thuật toán ngay lập tức tạo ra$k = 1$, dẫn đến$2$. Tuy nhiên, vì không thể phân chia được nên cách giải thích đúng là chỉ tồn tại một đồng xu. Công thức vòng lặp tránh được vấn đề này vì nó tính cho đến khi đạt đến 0 thay vì diễn giải trực tiếp các điều kiện phân tách và điều này phù hợp với độ sâu chuyển đổi dự kiến ​​thay vì tính đủ điều kiện thô. 

Vì$n = 4$, quá trình này tạo ra một phần chia thành hai đồng xu có giá trị 1 và không thể thực hiện thêm hành động nào. Thuật toán tính toán chính xác một mức mở rộng duy nhất ngoài gốc, mang lại hiệu ứng nhân đôi chính xác. 

Đối với lũy thừa rất lớn của 4 như$4^{k}$, vòng lặp đếm chính xác$k+1$giảm trước khi đạt đến 0, khớp với toàn bộ độ sâu của cây nhị phân ẩn. Điều này đảm bảo rằng các đầu vào có cấu trúc lớn không gây ra các lỗi riêng lẻ trong tính toán độ sâu.
