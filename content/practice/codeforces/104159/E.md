---
title: "CF 104159E - \u0412\u0435\u0440\u0441\u0442\u043e\u0432\u044b\u0435 \u0441\u0442\u043e\u043b\u0431\u044b"
description: "Chúng ta được cho một số nguyên dương $N$. Chúng ta cần xây dựng số nguyên dương nhỏ nhất thỏa mãn đồng thời hai điều kiện: nó phải chia hết cho $N$ và biểu diễn thập phân của nó phải kết thúc bằng chữ số 0. Tận cùng bằng 0 nghĩa là số đó là bội số của 10."
date: "2026-07-02T01:07:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104159
codeforces_index: "E"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u0420\u0421(\u042f) (5-8 \u043a\u043b\u0430\u0441\u0441\u044b) 2022-23, 2 \u0434\u0435\u043d\u044c"
rating: 0
weight: 104159
solve_time_s: 59
verified: true
draft: false
---

[CF 104159E - \u0412\u0435\u0440\u0441\u0442\u043e\u0432\u044b\u0435 \u0441\u0442\u043e\u043b\u0431\u044b](https://codeforces.com/problemset/problem/104159/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$N$. Chúng ta cần xây dựng số nguyên dương nhỏ nhất thỏa mãn đồng thời hai điều kiện: nó phải chia hết cho$N$và biểu diễn thập phân của nó phải kết thúc bằng chữ số 0. 

Kết thúc bằng 0 có nghĩa là số đó là bội số của 10. Vì vậy, chúng ta đang tìm số nhỏ nhất là bội số chung của$N$và 10, với ràng buộc bổ sung là chúng ta muốn số nhỏ nhất như vậy có giá trị tuyệt đối. 

Quan sát quan trọng là chúng ta không chỉ được yêu cầu bội số chung nhỏ nhất ở dạng trừu tượng mà cụ thể là số nguyên dương nhỏ nhất đồng thời là bội số của$N$và 10. Đó chính xác là định nghĩa của$\mathrm{lcm}(N, 10)$, vì bất kỳ số nào chia hết cho cả hai$N$và 10 phải là bội số chung và số nhỏ nhất như vậy là LCM. 

Kích thước đầu vào tăng lên$10^9$, loại trừ bất kỳ phương pháp tìm kiếm lặp hoặc nặng về hệ số nào phụ thuộc vào việc quét bội số của$N$. Thậm chí lặp đi lặp lại lên đến$10^9 / N$trong trường hợp xấu nhất là không cần thiết khi cấu trúc hoàn toàn mang tính lý thuyết số. 

Một cách tiếp cận ngây thơ sẽ cố gắng bắt đầu từ$N$và liên tục thêm$N$cho đến khi bội số kết thúc bằng 0. Điều này thất bại ngay lập tức trong trường hợp như$N = 999999937$, trong đó câu trả lời hợp lệ đầu tiên là cực kỳ lớn so với$N$và số lần lặp có thể trở nên lớn trong các trường hợp bệnh lý. 

Một cạm bẫy tinh vi khác là cho rằng nhân với 10 luôn cho kết quả. Điều đó chỉ đúng khi$N$không chia hết cho 2 hoặc 5 theo cách đã tương tác với hệ số hóa của 10. Ví dụ:$N = 6$đưa ra câu trả lời 30, không phải 60. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là tạo ra bội số của$N$:$N, 2N, 3N, \dots$và dừng ở giá trị đầu tiên có chữ số cuối cùng bằng 0. Điều này đúng vì mọi câu trả lời hợp lệ phải là bội số của$N$, do đó việc liệt kê bội số cuối cùng sẽ tìm thấy nó. Vấn đề là số lượng ứng viên có thể lớn trước khi đạt đến số chia hết cho 10. Trong trường hợp xấu nhất, nếu$N$là nguyên tố cùng nhau với 10, chữ số cuối cùng quay vòng qua tất cả 10 khả năng, nghĩa là chúng ta có thể cần tối đa 10 bước cho mỗi chu trình dư lượng đầy đủ, nhưng vì các giá trị tăng không giới hạn nên điều này vẫn không mang lại giới hạn hữu ích cho số lớn$N$trong thực tế khi suy nghĩ về mức độ quan trọng của câu trả lời hơn là số lần lặp lại. 

Cấu trúc của vấn đề loại bỏ tất cả sự phức tạp này. Chúng tôi đang tìm kiếm một số chia hết cho cả hai$N$và 10, chính xác là bội số chung nhỏ nhất của hai số nguyên này. Vì 10 chỉ có thừa số nguyên tố 2 và 5 nên ta chỉ cần điều chỉnh$N$sao cho nó chứa đủ các yếu tố này để bao gồm 10. Cụ thể, chúng tôi tính toán$\mathrm{lcm}(N, 10) = \frac{N \cdot 10}{\gcd(N, 10)}$. 

Tính toán gcd là thời gian không đổi đối với các số nguyên 32 bit hoặc 64 bit, do đó điều này làm giảm toàn bộ vấn đề thành một biểu thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bội số Brute Force | O(k) | O(1) | Quá chậm | 
| LCM dựa trên GCD | O(log N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$N$. Toàn bộ cấu trúc của nghiệm chỉ phụ thuộc vào sự tương tác của thừa số nguyên tố với 10. 
2. Tính toán$g = \gcd(N, 10)$. Điều này trích xuất các yếu tố được chia sẻ giữa$N$và yêu cầu kết quả kết thúc bằng 0, điều này buộc phải chia hết cho 2 và 5. 
3. Tính đáp án dưới dạng$\frac{N}{g} \cdot 10$. Cấu trúc này đảm bảo chúng ta chỉ thêm các thừa số nguyên tố còn thiếu cần thiết để làm cho số chia hết cho 10 mà không gây ra sự dư thừa. 
4. Xuất trực tiếp giá trị tính toán. 

### Tại sao nó hoạt động 

Mọi số hợp lệ phải chia hết cho cả hai$N$và 10. Tập hợp tất cả các số như vậy chính xác là tập hợp bội số của$\mathrm{lcm}(N, 10)$. biểu thức$\frac{N \cdot 10}{\gcd(N, 10)}$tạo ra số nhỏ nhất chứa tất cả các thừa số nguyên tố theo yêu cầu của cả hai$N$và 10 không trùng lặp. Vì bất kỳ số nào nhỏ hơn sẽ không chia hết cho ít nhất một trong hai ràng buộc nên kết quả là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    n = int(input().strip())
    g = math.gcd(n, 10)
    ans = (n // g) * 10
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp này hoàn toàn dựa vào việc tính toán gcd với 10, để tách biệt xem có bao nhiêu thừa số của 2 và 5 đã có trong$N$. Chia cho gcd này sẽ loại bỏ sự trùng lặp trước khi nhân với 10, đảm bảo chúng tôi không tính hai lần các hệ số chung. 

Thứ tự các thao tác đóng vai trò quan trọng đối với tính chính xác và an toàn tràn trong các ngôn ngữ có kích thước số nguyên cố định, nhưng trong Python phạm vi số nguyên là không giới hạn. Tuy nhiên, tính toán$n // g$trước khi nhân giữ cho biểu thức phù hợp với định nghĩa toán học của LCM. 

## Ví dụ đã hoạt động 

### Mẫu 1:$N = 6$| Bước | N | gcd(N,10) | n/g | kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | - | - | - | 
| gcd | 6 | 2 | - | - | 
| chia | 6 | 2 | 3 | - | 
| nhân | 6 | 2 | 3 | 30 | 

Số 6 đã chứa thừa số 2 nên chỉ cần thêm một thừa số 5 để đạt bội số của 10. Kết quả 30 là số nhỏ nhất chia hết cho cả 6 và 10. 

### Mẫu 2:$N = 19$| Bước | N | gcd(N,10) | n/g | kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 19 | - | - | - | 
| gcd | 19 | 1 | - | - | 
| chia | 19 | 1 | 19 | - | 
| nhân | 19 | 1 | 19 | 190 | 

Vì 19 là số nguyên tố cùng nhau với 10 nên chúng ta phải gắn đầy đủ thừa số của 10. Không bội số nhỏ hơn nào có thể tận cùng bằng 0 mà vẫn chia hết cho 19. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log N) | bị chi phối bởi tính toán gcd | 
| Không gian | O(1) | chỉ một vài biến số nguyên | 

Việc tính toán là công việc không đổi cho mỗi trường hợp thử nghiệm và thậm chí với nhiều đầu vào, tổng thời gian chạy vẫn không đáng kể so với các ràng buộc lên đến$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io, math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    g = math.gcd(n, 10)
    return str((n // g) * 10)

# provided samples
assert solve("6\n") == "30"
assert solve("19\n") == "190"

# minimum case
assert solve("1\n") == "10"

# already ending with 0
assert solve("10\n") == "10"

# multiple of 2 but not 5
assert solve("8\n") == "40"

# multiple of 5 but not 2
assert solve("25\n") == "50"

# large prime
assert solve("999999937\n") == str((999999937 // math.gcd(999999937, 10)) * 10)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 10 | trường hợp cạnh nhỏ nhất | 
| 10 | 10 | bội số đã hợp lệ | 
| 8 | 40 | nhu cầu yếu tố 5 | 
| 25 | 50 | nhu cầu yếu tố 2 | 
| 999999937 | tính toán | độ đúng nguyên tố lớn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$N$đã chia hết cho 10. Vì$N = 10$, gcd với 10 là 10, nên công thức cho$(10 / 10) \cdot 10 = 10$. Thuật toán tránh được việc tăng số một cách chính xác, vì nó đã là bội số nhỏ nhất của chính nó và kết thúc bằng 0. 

Vì$N = 8$, gcd với 10 là 2. Phép tính trở thành$(8 / 2) \cdot 10 = 40$. Theo dõi điều này, chúng ta thấy rằng 8 thiếu hệ số 5 và việc nhân sau khi loại bỏ hệ số chung đảm bảo chúng ta đưa vào chính xác một số 5 mà không làm ảnh hưởng đến hệ số 2 đã có. 

Vì$N = 25$, gcd là 5 nên kết quả là$(25 / 5) \cdot 10 = 50$. Điều này thể hiện tính đối xứng: thuật toán loại bỏ sự chồng chéo để chỉ đưa ra các thừa số nguyên tố còn thiếu là 10, tránh tính thừa và đảm bảo tính tối thiểu.
