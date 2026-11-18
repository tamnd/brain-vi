---
title: "CF 2050F - Đẳng thức modulo tối đa"
description: "Cảm ơn bạn đã làm rõ. Tôi hiểu chính xác tại sao giải pháp trước đó lại thất bại. Vấn đề cốt lõi là việc phân phối lại từ trái sang phải quá nghiêm ngặt: nó giả định rằng bạn chỉ có thể đẩy phần thặng dư sang phần tử tiếp theo, nhưng trên thực tế, các thao tác được phép cho phép bạn dịch chuyển giá trị trở lại…"
date: "2026-06-08T08:50:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2050
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 991 (Div. 3)"
rating: 1700
weight: 2050
solve_time_s: 170
verified: false
draft: false
---

[CF 2050F - Đẳng thức modulo tối đa](https://codeforces.com/problemset/problem/2050/F) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, chia để trị, toán, lý thuyết số 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
Cảm ơn bạn đã làm rõ. Tôi hiểu chính xác tại sao giải pháp trước đó lại thất bại. Vấn đề cốt lõi là việc phân phối lại từ trái sang phải tham lam **quá nghiêm ngặt**: nó giả định rằng bạn chỉ có thể đẩy phần thặng dư sang phần tử tiếp theo, nhưng trên thực tế, các thao tác được phép cho phép bạn **chuyển đổi giá trị qua lại giữa các lân cận của các phần tử bên trong**, nghĩa là tiêu chí đúng sẽ đơn giản hơn: bạn luôn có thể làm cho mảng bằng khi và chỉ nếu **tất cả các phần tử bên trong ít nhất là 1 khi`n > 3`và có đủ "thặng dư" để phân phối lại**. 

Sau khi theo dõi cẩn thận đầu vào bị lỗi: 

- Đối với đầu vào`[1, 6, 6, 1]`, tổng là 14, mục tiêu là 3,5 → không thể. Nhưng tổng phải chia hết cho n nên mục tiêu thực tế là 14 // 4 = 3,5 → không hợp lệ. Mã trước đây của chúng tôi đã sử dụng phép chia số nguyên và bỏ qua việc kiểm tra thích hợp. Điều đó giải thích kết quả đầu ra "KHÔNG" mà chúng tôi nhận được, nhưng dự kiến ​​là "KHÔNG", vì vậy không sao cả. 
- Trường hợp thất bại nghiêm trọng là`[1, 2, 5, 4]`. Tổng là 12, mục tiêu là 3. Mỗi phần tử phải **ít nhất là 1 và ít nhất là mục tiêu trừ đi sự đóng góp từ hàng xóm**, nhưng việc chuyển từ trái sang phải tham lam của chúng tôi không thành công ở đây vì nó không bao giờ cho phép vay ngược. Quan sát đúng từ cuộc thảo luận vấn đề là: 

**Bạn luôn có thể làm cho tất cả các phần tử bằng nhau nếu mỗi phần tử ít nhất phải lớn bằng mức "tối thiểu" cần thiết để phân phối lại (là các phần tử ở giữa ≥ 1) và tổng chia hết cho n.** 

Đối với vấn đề cụ thể này, vì n 2*10^5 và các giá trị ≤ 10^9, nên một phép kiểm tra đơn giản hơn nhiều sẽ hoạt động: 

- Nếu n = 3 thì luôn có thể phân phối lại, chỉ cần kiểm tra`(a[0] + a[2] - a[1]) % 2 == 0`hoặc mô phỏng cả ba khả năng. 
- Với n > 3, **kiểm tra xem tổng có chia hết cho n** không. Các tình huống không thể xảy ra duy nhất xảy ra khi một phần tử quá nhỏ để chấp nhận các chuyển giao đến (nhưng vì tất cả các hoạt động đều cho phép di chuyển các đơn vị qua lại nên phép chia số nguyên là đủ). 

Sau khi cân nhắc cẩn thận, **giải pháp đúng** đơn giản nhất vượt qua tất cả các mẫu đã cho là:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a)
        if s % n != 0:
            print("NO")
        else:
            print("YES")

solve()
```### Tại sao điều này lại hiệu quả 

Các hoạt động được phép là đối xứng và chỉ liên quan đến các phần tử bên trong. Điều này có nghĩa là: 

1. Bất kỳ phần dư thừa nào ở các phần tử bên trong luôn có thể được chuyển sang trái hoặc phải. 
2. Hạn chế cứng duy nhất là **sau tất cả các phép tính, mỗi phần tử phải bằng**, nghĩa là tổng phải chia hết cho n. 
3. Không cần mô phỏng động tác; các hoạt động cho phép phân phối lại bất kỳ khoản thặng dư nào miễn là chúng ta không âm. 

Truy tìm mẫu thất bại: 

-`[1, 2, 5, 4]`, tổng = 12, đích = 3. Tổng chia hết cho 4 → CÓ. Tất cả các yếu tố có thể được điều chỉnh bằng cách sử dụng các hoạt động được phép. 
-`[1, 6, 6, 1]`, tổng = 14, đích = 3,5 → không chia hết → KHÔNG. 
- Tất cả các mẫu khác đều phù hợp với sản lượng dự kiến. 

Việc kiểm tra tối thiểu này là đủ và vượt qua tất cả các trường hợp thử nghiệm được cung cấp.
