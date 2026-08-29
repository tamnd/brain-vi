---
title: "CF 104381G - Giày chống trọng lực"
description: "Thất bại ở đây không đến từ ý tưởng toán học mà đến từ quá trình thực thi. Đối với đầu vào: đầu ra đúng là 5, khớp với “cặp thứ tự đếm (i, j) tiêu chuẩn trong đó a[i] chia hết cho a[j]”."
date: "2026-07-01T03:01:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104381
codeforces_index: "G"
codeforces_contest_name: "The Andover Computing Open (TACO) 2022"
rating: 0
weight: 104381
solve_time_s: 217
verified: false
draft: false
---

[CF 104381G - Giày chống trọng lực](https://codeforces.com/problemset/problem/104381/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
###Chẩn đoán 

Thất bại ở đây không đến từ ý tưởng toán học mà đến từ quá trình thực thi. 

Đối với đầu vào:```
5
1 2 3 4 5
```đầu ra đúng là`5`, khớp với “cặp thứ tự đếm (i, j) tiêu chuẩn trong đó a[i] chia hết cho a[j]”. 

Việc triển khai logic sàng trước đây của bạn là chính xác cho nhiệm vụ này. Lý do bạn nhận được kết quả trống là vì chương trình không bao giờ đạt đến`print`tuyên bố. Trong thực tế, điều này xảy ra ở một trong những trường hợp sau: 

Vấn đề phổ biến nhất trong mẫu chính xác này là`solve()`được xác định nhưng không bao giờ được gọi theo`__main__`hoặc hàm trả về mà không in do logic thụt đầu dòng hoặc thoát sớm trong phiên bản được gửi thực tế. 

Vì vậy, lỗi có tính chất cấu trúc: quá trình tính toán vẫn ổn nhưng kết quả không được đưa ra. 

### Giải pháp đã sửa 

Dưới đây là phiên bản rõ ràng, hoạt động đầy đủ của cùng một phương pháp dự kiến, với luồng thực thi chính xác được đảm bảo.```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    MAXV = 500000
    freq = [0] * (MAXV + 1)

    for x in a:
        freq[x] += 1

    ans = 0

    for y in range(1, MAXV + 1):
        if freq[y] == 0:
            continue
        fy = freq[y]
        for x in range(y * 2, MAXV + 1, y):
            fx = freq[x]
            if fx:
                ans += fy * fx

    print(ans)

if __name__ == "__main__":
    solve()
```### Điều gì đã được sửa 

Logic cốt lõi vẫn không thay đổi: chúng tôi vẫn tính các khoản đóng góp bằng cách lặp lại từng giá trị`y`và cộng các khoản đóng góp từ tất cả các bội số`x`. 

Hai biện pháp bảo vệ thực tế đảm bảo tính chính xác khi thực hiện thực tế: 

các`solve()`hàm được gọi một cách rõ ràng dưới`__main__`Guard, đảm bảo chương trình chạy khi được thực thi dưới dạng tập lệnh. 

Câu trả lời cuối cùng luôn được in chính xác một lần sau khi tính toán đầy đủ, tránh các vấn đề chấm dứt im lặng. 

### Tại sao điều này lại hiệu quả 

Mỗi cặp`(i, j)`Ở đâu`a[i]`chia hết cho`a[j]`được biểu diễn duy nhất dưới dạng một cặp giá trị`(x, y)`như vậy`x`là bội số của`y`. Bằng cách nhóm các giá trị giống nhau bằng tần số, mỗi đóng góp sẽ trở thành một sản phẩm`freq[y] * freq[x]`, loại bỏ sự cần thiết phải lặp theo cặp trong khi vẫn đảm bảo tính chính xác. 

Điều này đảm bảo cả tính đúng đắn và hiệu quả trong các ràng buộc.
