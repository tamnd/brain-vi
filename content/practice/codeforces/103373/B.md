---
title: "CF 103373B - Tổng phần"
description: "Chúng ta được cho nhiều số nguyên dương và với mỗi số đó chúng ta phải quyết định xem các ước số thực sự của nó “giàu” đến mức nào. Đối với một số $n$, chúng ta xem xét tất cả các ước của nó ngoại trừ chính $n$, tính tổng chúng lại và so sánh tổng đó với $n$."
date: "2026-07-03T12:36:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103373
codeforces_index: "B"
codeforces_contest_name: "2021 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103373
solve_time_s: 47
verified: true
draft: false
---

[CF 103373B - Tổng phần](https://codeforces.com/problemset/problem/103373/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho nhiều số nguyên dương và với mỗi số đó chúng ta phải quyết định xem các ước số thực sự của nó “giàu” đến mức nào. Đối với một số$n$, chúng tôi xem xét tất cả các ước của nó ngoại trừ$n$chính nó, tổng hợp chúng lại và so sánh số tiền đó với$n$. Nếu tổng lớn hơn thì gọi là số nhiều, nếu nhỏ hơn thì gọi là thiếu, nếu bằng nhau thì gọi là số hoàn hảo. 

Đầu vào về cơ bản là một loạt truy vấn lớn. Mỗi truy vấn là một số nguyên lên tới một triệu và có thể có tới một triệu truy vấn như vậy. Đầu ra là một chuỗi phân loại theo số, vì vậy thách thức chính không phải là tính tổng một ước số duy nhất mà thực hiện cực kỳ nhanh qua nhiều truy vấn lặp lại. 

Các ràng buộc ngay lập tức loại trừ việc tính toán lại các ước số từ đầu cho mỗi truy vấn. Một ước số ngây thơ quét lên đến$n$trong trường hợp xấu nhất sẽ quá chậm đối với một truy vấn và được nhân với$10^6$truy vấn nó trở nên không thể. Thậm chí quét tới$\sqrt{n}$mỗi truy vấn vẫn còn quá nặng ở quy mô này. 

Trường hợp cạnh tinh tế xuất phát từ các số hoàn hảo trong đó tổng bằng chính xác số đó. Ví dụ: 6, 28, 496. Việc triển khai bất cẩn mà quên loại trừ chính số đó sẽ luôn phân loại các số là "dầu" không chính xác, vì tổng ước số đầy đủ luôn bao gồm$n$. 

Một trường hợp đặc biệt khác là các truy vấn lặp lại. Vì đầu vào có thể chứa cùng một số nhiều lần nên việc tính lại tổng ước số của nó mỗi lần là công việc lãng phí và có thể sẽ là TLE. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi số$n$, lặp từ 1 đến$n-1$, kiểm tra tính chia hết và tích lũy tổng các ước số. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về tổng phần. Tuy nhiên, mỗi truy vấn có giá$O(n)$, và với$n$lên đến$10^6$Và$10^6$truy vấn, tổng số thao tác sẽ giảm xuống$10^{12}$, vượt xa giới hạn khả thi. 

Một cải tiến tiêu chuẩn là chỉ kiểm tra các ước số tối đa$\sqrt{n}$, cộng cả hai$d$Và$n/d$khi$d$chia rẽ$n$. Điều này làm giảm một truy vấn xuống$O(\sqrt{n})$, nhưng với$10^6$truy vấn này vẫn theo thứ tự$10^9$hoạt động nằm ở ranh giới và không an toàn trong Python. 

Quan sát quan trọng là phạm vi giá trị nhỏ và cố định: tất cả các số nằm trong khoảng từ 1 đến$10^6$. Thay vì tính toán lại tổng các ước số cho mỗi truy vấn, chúng ta có thể tính toán trước tổng các ước số thích hợp cho mọi số trong phạm vi này một lần bằng cách sử dụng phép tích lũy giống như sàng. Mỗi số nguyên$d$đóng góp vào tất cả các bội số của$d$, vì vậy chúng ta có thể lặp lại các ước số và truyền bá các đóng góp một cách hiệu quả. Điều này biến bài toán thành một sàng tổng chia cổ điển, giảm hoàn toàn việc tính toán lặp lại. 

Sau khi quá trình tính toán trước hoàn tất, mỗi truy vấn sẽ trở thành một phép tra cứu và so sánh mảng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(T \cdot n)$|$O(1)$| Quá chậm | 
| Căn bậc hai cho mỗi truy vấn |$O(T \sqrt{n})$|$O(1)$| Quá chậm | 
| Sàng tính toán trước |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tổng các ước số thực sự cho mỗi số lên tới$10^6$. Thay vì xử lý từng số một cách độc lập, chúng tôi đảo ngược quan điểm và để mỗi ước số có thể “đóng góp về phía trước”. 

1. Tạo một mảng`s`kích thước$N+1$, được khởi tạo bằng 0, ở đâu`s[x]`sẽ lưu trữ tổng các ước số thích hợp của$x$. Chúng tôi không bao gồm$x$chính nó bằng tổng của chính nó, vì vậy chúng tôi đảm bảo rằng chúng tôi không bao giờ thêm một số vào chỉ mục của chính nó. 
2. Với mọi số nguyên$d$từ 1 đến$N/2$, chúng tôi lặp lại tất cả các bội số$m = 2d, 3d, 4d, \dots$. Với mỗi bội số như vậy$m$, chúng tôi thêm$d$vào trong`s[m]`. Điều này hoạt động vì$d$là ước thực sự của mọi bội số lớn hơn chính nó. 
3. Sau quá trình này,`s[n]`chứa chính xác tổng của tất cả các ước thực sự của$n$, vì mọi ước số$d < n$đóng góp đúng một lần vào bội số của nó. 
4. Đối với mỗi truy vấn$n_i$, chúng tôi so sánh`s[n_i]`với$n_i$. Nếu như`s[n_i] > n_i`, chúng tôi sản xuất dồi dào. Nếu như`s[n_i] < n_i`, chúng tôi sản xuất thiếu. Nếu không thì chúng tôi xuất ra hoàn hảo. 

Tại sao nó hoạt động được là do một bất biến đếm rõ ràng. Mỗi cặp$(d, m)$Ở đâu$d$chia rẽ$m$Và$d < m$được thêm chính xác một lần khi chúng tôi xử lý$d$. Không có sự trùng lặp vì mỗi ước số chịu trách nhiệm về tất cả các bội số của nó một cách độc lập và không có ước số nào đóng góp vào một số lớn hơn ranh giới phạm vi theo cách vi phạm tính chính xác. Kết quả là mỗi số tích lũy chính xác tổng các ước số thực sự của nó và không có gì khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

# precompute sum of proper divisors
s = [0] * (MAXN + 1)

for d in range(1, MAXN // 2 + 1):
    for m in range(2 * d, MAXN + 1, d):
        s[m] += d

t = int(input())
nums = list(map(int, input().split()))

out = []
for x in nums:
    if s[x] > x:
        out.append("abundant")
    elif s[x] < x:
        out.append("deficient")
    else:
        out.append("perfect")

sys.stdout.write("\n".join(out))
```Chi tiết triển khai cốt lõi là vòng sàng. Vòng lặp bên ngoài chọn một ứng cử viên của số chia và vòng lặp bên trong phân phối nó cho tất cả các bội số. Bắt đầu vòng lặp bên trong tại`2*d`là rất quan trọng vì chúng tôi loại trừ rõ ràng số đó khỏi tổng ước số của chính nó. 

Một điểm tinh tế khác là chúng tôi tính toán trước một lần cho tới$10^6$bất kể kích thước đầu vào. Điều này đảm bảo công việc tốn kém sẽ được khấu hao và mỗi truy vấn sẽ trở thành thời gian không đổi. 

Bước so sánh là trực tiếp vì`s[x]`đã loại trừ rồi`x`bản thân bằng cách xây dựng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng các giá trị mẫu 12, 21 và 28. 

Đối với 12, các ước số thích hợp của nó là 1, 2, 3, 4, 6. Quá trình tính toán trước được xây dựng`s[12] = 16`. Với 21, các ước là 1, 3, 7 cho`s[21] = 11`. Với 28, các ước là 1, 2, 4, 7, 14 cho`s[28] = 28`. 

| Số | Tích lũy các ước số thích hợp | s(n) | So sánh | 
| --- | --- | --- | --- | 
| 12 | 1, 2, 3, 4, 6 | 16 | 16 > 12 | 
| 21 | 1, 3, 7 | 11 | 11 < 21 | 
| 28 | 1, 2, 4, 7, 14 | 28 | 28 = 28 | 

Dấu vết này xác nhận rằng sàng tích lũy chính xác các đóng góp của số chia và việc phân loại đó chỉ phụ thuộc vào so sánh thời gian không đổi cuối cùng. 

Ví dụ nhỏ thứ hai là 6. Trong quá trình tiền xử lý, các đóng góp đến từ 1, 2 và 3, tạo ra`s[6] = 6`, đặt nó vào danh mục hoàn hảo một cách chính xác. Điều này xác nhận rằng thuật toán xử lý chính xác các cấu trúc ước số đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + T)$| Mỗi số nguyên phân phối các đóng góp cho bội số của nó và chuỗi hài giới hạn các phép toán tổng; mỗi truy vấn là O(1) | 
| Không gian |$O(N)$| Chúng tôi lưu trữ tổng chia cho tất cả các số nguyên lên đến$10^6$| 

Chi phí tiền xử lý nằm trong giới hạn cho$N = 10^6$, và sau đó mỗi cái lên tới$10^6$truy vấn chỉ là một so sánh. Điều này thoải mái phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    MAXN = 10**6
    s = [0] * (MAXN + 1)

    for d in range(1, MAXN // 2 + 1):
        for m in range(2 * d, MAXN + 1, d):
            s[m] += d

    t = int(input())
    nums = list(map(int, input().split()))

    res = []
    for x in nums:
        if s[x] > x:
            res.append("abundant")
        elif s[x] < x:
            res.append("deficient")
        else:
            res.append("perfect")

    return "\n".join(res)

# provided sample
assert run("3\n12 21 28") == "abundant\ndeficient\nperfect"

# minimum case
assert run("1\n1") == "deficient"

# perfect number check
assert run("2\n6 28") == "perfect\nperfect"

# abundant small case
assert run("1\n12") == "abundant"

# mixed case
assert run("5\n2 3 4 5 6") == "deficient\ndeficient\ndeficient\ndeficient\nperfect"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | thiếu sót | trường hợp biên nhỏ nhất | 
| 6, 28 | hoàn hảo | số hoàn hảo đã biết | 
| 12 | dồi dào | ví dụ phong phú nhỏ nhất | 
| 2,3,4,5,6 | hỗn hợp | phân loại đúng đắn | 

## Vỏ cạnh 

Đối với đầu vào`1`, sàng cho`s[1] = 0`vì 1 không có ước số thực sự. Thuật toán so sánh 0 < 1 và đưa ra kết quả thiếu chính xác. Điều này nắm bắt được trường hợp ranh giới trong đó tập hợp số chia trống. 

Vì`6`, tiền xử lý thêm các đóng góp từ 1, 2 và 3, đưa ra`s[6] = 6`. Việc kiểm tra đẳng thức kích hoạt hoàn hảo, xác nhận rằng chúng tôi không bao giờ vô tình bao gồm 6 trong tổng của chính nó. 

Vì`28`, các đóng góp từ 1, 2, 4, 7 và 14 tích lũy chính xác đến 28. Vì mỗi ước số được xử lý độc lập nên không có phép tính kép và thuật toán phân loại nó một cách đáng tin cậy là hoàn hảo.
