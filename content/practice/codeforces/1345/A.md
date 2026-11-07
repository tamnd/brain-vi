---
title: "CF 1345A - Mảnh ghép"
description: "Chúng ta được yêu cầu xác định xem liệu một lưới các mảnh ghép hình có thể được lắp ráp với một thiết kế mảnh đặc biệt hay không. Mỗi phần có chính xác ba tab và một trống. Các mảnh có thể được xoay theo bất kỳ hướng nào."
date: "2026-06-11T15:02:28+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1345
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 639 (Div. 2)"
rating: 800
weight: 1345
solve_time_s: 108
verified: true
draft: false
---

[CF 1345A - Mảnh ghép](https://codeforces.com/problemset/problem/1345/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu một lưới các mảnh ghép hình có thể được lắp ráp với một thiết kế mảnh đặc biệt hay không. Mỗi phần có chính xác ba tab và một trống. Các mảnh có thể được xoay theo bất kỳ hướng nào. Một giải pháp tồn tại nếu chúng ta có thể điền vào một$n \times m$lưới sao cho mỗi tab vừa khít với một khoảng trống dọc theo các cạnh được chia sẻ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp hai số nguyên,$n$Và$m$, chỉ định số lượng hàng và cột trong lưới. Đầu ra phải là "CÓ" nếu câu đố có thể được giải theo các điều kiện này và "KHÔNG" nếu ngược lại. 

Với những hạn chế$1 \le n,m \le 10^5$và lên đến$1000$trong các trường hợp thử nghiệm, chúng tôi không thể mô phỏng câu đố hoặc cố gắng đặt từng mảnh riêng lẻ. Bất kỳ thuật toán brute-force nào thao tác từng ô sẽ yêu cầu$O(n \cdot m)$các hoạt động có thể đạt tới$10^{10}$trong trường hợp xấu nhất. Điều này vượt quá giới hạn thời gian thực tế, vì vậy chúng ta cần một cách tiếp cận toán học hoặc tổ hợp. 

Các trường hợp cạnh không rõ ràng phát sinh khi lưới rất hẹp hoặc rất nhỏ. Ví dụ, một$1 \times m$hoặc$n \times 1$lưới hoạt động khác với lưới lớn hơn vì các phần ở các cạnh không yêu cầu tất cả các cạnh phải khớp nhau. Cụ thể, một$1 \times 3$lưới có thể giải được, trong khi đó$100000 \times 100000$lưới thì không, mặc dù cả hai đều là hình chữ nhật, bởi vì các ràng buộc bên trong không thể được thỏa mãn chỉ với một ô trống trên mỗi phần. Một cách tiếp cận bất cẩn mà bỏ qua tính chẵn lẻ hoặc kết nối bên trong sẽ tạo ra "CÓ" không chính xác cho các lưới vuông lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cố gắng đặt các mảnh theo mọi hướng và kiểm tra từng cặp liền kề để tìm các tab và khoảng trống phù hợp. Điều này đúng với các lưới nhỏ, nhưng số lượng thao tác là$O(n \cdot m)$mỗi trường hợp thử nghiệm. Với giới hạn tối đa, điều này sẽ yêu cầu$10^{10}$hoạt động không thể thực hiện được trong thời hạn. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc của các mảnh. Mỗi phần có một ô trống và ba tab. Trong bất kỳ lưới nào lớn hơn$2 \times 2$, một số mảnh nhất thiết sẽ có nhiều hơn một hàng xóm ở các phía khác nhau, tạo ra xung đột. Ví dụ: một mảnh trong lưới hình vuông có bốn ô lân cận sẽ cần bốn ô trống để khớp, nhưng nó chỉ có một ô trống. Do đó, các lưới duy nhất có thể giải được là một chiều (một hàng hoặc một cột) hoặc một$2 \times 2$hình vuông nơi mỗi mảnh có thể làm hài lòng tất cả hàng xóm. 

Vấn đề được rút gọn thành một kiểm tra đơn giản: nếu lưới là một hàng hoặc một cột, thì nó luôn có thể giải được vì mỗi phần chỉ cần khớp với một hàng xóm dọc theo hàng hoặc cột. Nếu cả hai chiều đều chẵn và bằng hai thì a$2 \times 2$hình vuông có thể giải được. Tất cả các cấu hình khác đều không thể thực hiện được vì ít nhất một phần sẽ có nhiều hàng xóm hơn các phần trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Quá chậm | 
| Toán học/Quan sát | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, đọc số nguyên$n$Và$m$. 
3. Nếu một trong hai$n = 1$hoặc$m = 1$, in "CÓ" vì luôn có thể tập hợp một hàng hoặc cột. 
4. Ngược lại nếu$n = 2$Và$m = 2$, in "CÓ" vì$2 \times 2$lưới cho phép mỗi phần khớp với tất cả các phần lân cận. 
5. Nếu không, hãy in "KHÔNG" vì không có cấu hình nào thỏa mãn các ràng buộc chuyển tab sang trống bên trong. 

Tính đúng đắn đến từ tính bất biến là mỗi phần có đúng một ô trống. Bất kỳ mảnh nào có nhiều hơn một hàng xóm đều yêu cầu nhiều khoảng trống hơn mức có sẵn. Chỉ có hàng đơn, cột đơn và$2 \times 2$vuông tôn trọng bất biến này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n == 1 or m == 1:
        print("YES")
    elif n == 2 and m == 2:
        print("YES")
    else:
        print("NO")
```Giải pháp đọc tất cả các trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng đầu vào nhanh. Các kiểm tra có điều kiện trực tiếp thực hiện quan sát toán học xuất phát ở trên. sử dụng`or`cho phép bất kỳ lưới một chiều nào và việc kiểm tra đẳng thức sẽ xử lý$2 \times 2$trường hợp cạnh. Tất cả các đầu vào khác đều bị từ chối trong thời gian không đổi. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
1 3
100000 100000
2 2
```| Bước | n | m | Điều kiện phù hợp | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | n=1 | CÓ | 
| 2 | 100000 | 100000 | không | KHÔNG | 
| 3 | 2 | 2 | n=2 & m=2 | CÓ | 

Hàng đầu tiên thể hiện lưới một hàng, có thể giải được. Hàng thứ hai là một lưới hình vuông lớn, không thành công vì các phần bên trong không thể đáp ứng yêu cầu về khoảng trống tab. Hàng thứ ba là một$2 \times 2$Grid, hoạt động như mong đợi. 

Một đầu vào khác:```
2
1 1
2 3
```| Bước | n | m | Điều kiện phù hợp | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | n=1 | CÓ | 
| 2 | 2 | 3 | không | KHÔNG | 

Trường hợp một mảnh xác nhận rằng lưới tầm thường được chấp nhận. các$2 \times 3$hình chữ nhật không thành công vì các phần bên trong không thể khớp với nhiều hơn một phần lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một vài lần kiểm tra liên tục. | 
| Không gian | O(1) | Không cần thêm dung lượng lưu trữ tỷ lệ với n hoặc m. | 

Được cho$t \le 1000$Và$n, m \le 10^5$, giải pháp chạy ở dưới$10^3$hoạt động cho mỗi trường hợp thử nghiệm và phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 1 or m == 1:
            print("YES")
        elif n == 2 and m == 2:
            print("YES")
        else:
            print("NO")
    return out.getvalue().strip()

# Provided samples
assert run("3\n1 3\n100000 100000\n2 2\n") == "YES\nNO\nYES", "sample 1"

# Custom cases
assert run("2\n1 1\n2 3\n") == "YES\nNO", "single piece and rectangle"
assert run("2\n1 100000\n100000 1\n") == "YES\nYES", "extreme single row/column"
assert run("1\n2 2\n") == "YES", "minimum 2x2"
assert run("1\n3 2\n") == "NO", "non-square small rectangle"
assert run("1\n100000 100000\n") == "NO", "maximum size square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | Lưới một mảnh | 
| 2 3 | KHÔNG | Hình chữ nhật nơi các phần bên trong không thể khớp với nhau | 
| 1 100000 | CÓ | Hàng đơn, chiều rộng tối đa | 
| 100000 1 | CÓ | Cột đơn, chiều cao tối đa | 
| 2 2 | CÓ | Bình phương giải được tối thiểu | 
| 3 2 | KHÔNG | Hình chữ nhật nhỏ, không thể | 
| 100000 100000 | KHÔNG | Lưới vuông tối đa | 

## Vỏ cạnh 

Lưới nhỏ nhất,$1 \times 1$, được xử lý bởi`n == 1`điều kiện và kết quả đầu ra "CÓ". Đối với một lưới hẹp nhưng dài như$1 \times 100000$, thuật toán vẫn đưa ra "CÓ" vì mỗi phần chỉ có một phần lân cận. các$2 \times 2$hình vuông kích hoạt điều kiện thứ hai. Những hình vuông lớn như$100000 \times 100000$thất bại vì tính bất biến của một ô trống trên mỗi mảnh không thể thỏa mãn các hàng xóm bên trong. Mỗi trường hợp cạnh được khớp trực tiếp bằng một trong các kiểm tra có điều kiện, đảm bảo tính chính xác.
