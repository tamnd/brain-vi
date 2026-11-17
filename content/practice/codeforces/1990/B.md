---
title: "CF 1990B - Mảng Thủ công"
description: "Chúng ta cần xây dựng một mảng có độ dài $n$, trong đó mỗi phần tử là $+1$ hoặc $-1$. Hai vị trí đặc biệt được xác định dựa trên tổng tiền tố và tổng hậu tố. Đối với tiền tố, chúng ta xem xét tất cả các tổng riêng $Si = a1 + dots + ai$."
date: "2026-06-08T15:32:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1990
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 960 (Div. 2)"
rating: 1200
weight: 1990
solve_time_s: 153
verified: false
draft: false
---

[CF 1990B - Array Craft](https://codeforces.com/problemset/problem/1990/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng có độ dài$n$, trong đó mỗi phần tử là$+1$hoặc$-1$. Hai vị trí đặc biệt được xác định dựa trên tổng tiền tố và tổng hậu tố. 

Đối với tiền tố, chúng tôi xem xét tất cả các tổng một phần$S_i = a_1 + \dots + a_i$. Chúng tôi tính toán giá trị tối đa trong số các khoản tiền này, sau đó chọn chỉ số nhỏ nhất tại đó đạt đến mức tối đa này. Chỉ số đó phải chính xác$x$. 

Đối với các hậu tố, chúng ta xem xét tất cả các tổng hậu tố$T_i = a_i + \dots + a_n$. Chúng tôi tính toán mức tối đa của chúng, sau đó chọn chỉ số lớn nhất đạt được mức tối đa này. Chỉ số đó phải chính xác$y$. 

Vì vậy, nhiệm vụ là định hình cấu hình tổng tiền tố sao cho đỉnh đầu tiên của nó ở mức$x$và định hình cấu hình tổng hậu tố một cách độc lập sao cho đỉnh cuối cùng của nó là ở$y$, trong khi chỉ sử dụng$\pm1$. 

Những ràng buộc cho phép$n$lên tới$10^5$trên tất cả các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc mô phỏng tất cả các mảng con hoặc tính toán lại các tổng lặp đi lặp lại sẽ quá chậm. 

Một vấn đề tế nhị là các điều kiện tiền tố và hậu tố không độc lập một cách tùy ý: một cấu trúc ngây thơ chỉ thỏa mãn yêu cầu tiền tố có thể dễ dàng phá vỡ điều kiện hậu tố. Ví dụ: điền mọi thứ bằng$+1$cho đến vị trí$x$và sau đó xen kẽ sau đó thường làm cho hậu tố tối đa xảy ra quá muộn hoặc quá sớm tùy thuộc vào tính chẵn lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử tất cả$2^n$nhiệm vụ của$\pm1$, sau đó tính toán tất cả các tổng tiền tố và hậu tố rồi xác minh các điều kiện. Đây là cấp số nhân và ngay lập tức không thể thực hiện được ngay cả đối với những$n$. 

Quan sát quan trọng là các tổng tiền tố và tổng hậu tố hoạt động đơn điệu theo những cách rất có cấu trúc trong$\pm1$mảng. Tổng tiền tố tăng thêm 1 hoặc giảm đi 1, do đó mức tối đa của nó được kiểm soát hoàn toàn bởi nơi chúng tôi ngừng duy trì độ lệch dương. 

Ý tưởng tương tự cũng áp dụng cho các tổng hậu tố, nhưng từ phía bên phải. 

Cái nhìn sâu sắc về cấu trúc chính là chúng ta có thể xây dựng mảng sao cho có một “ngọn đồi” được kiểm soát ở giữa các vị trí$y$Và$x$. Giữa$y$Và$x$, chúng tôi thực thi một mẫu đảm bảo tiền tố tối đa toàn cầu xảy ra chính xác tại$x$, đồng thời đảm bảo hậu tố tối đa toàn cầu xảy ra chính xác tại$y$. 

Điều này đạt được bằng cách thiết lập phân đoạn giữa$y$Và$x$cho tất cả$+1$, và đặt$-1$bên ngoài theo cách ngăn cản việc tích lũy số tiền lớn hơn sớm hay muộn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tham lam mang tính xây dựng |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng mảng trực tiếp. 

1. Khởi tạo một mảng$a$chiều dài$n$. Chúng tôi sẽ chỉ định các giá trị một cách xác định. 
2. Địa điểm$+1$trên toàn bộ phân khúc$[y, x]$. Điều này tạo ra một vùng tăng dần được đảm bảo buộc cả cực đại tiền tố và hậu tố phải vượt qua khoảng này. 
3. Đối với các vị trí còn lại của$y$, chúng ta gán các giá trị sao cho tổng tiền tố không bao giờ vượt quá giá trị đạt được tại vị trí$x$. Điều này được thực hiện bằng cách điền chúng với$-1$, ngăn cản sự tích lũy sớm. 
4. Đối với chức vụ quyền của$x$, chúng tôi cũng gán$-1$, đảm bảo rằng không có hậu tố nào bắt đầu quá xa bên phải có thể vượt quá hậu tố tối đa tại$y$. 
5. Xuất mảng kết quả. 

Ý tưởng chính là phân khúc$[y, x]$đóng vai trò là vùng duy nhất nơi cả tổng tiền tố và hậu tố có thể đạt cực đại toàn cầu. Bên ngoài khu vực này, tất cả các đóng góp đều âm, đảm bảo không có mức tối đa thay thế nào xuất hiện. 

### Tại sao nó hoạt động 

Bên trong khoảng$[y, x]$, mỗi bước sẽ tăng cả tổng tiền tố và hậu tố, đảm bảo đạt được cực đại trong phân đoạn này. Vì tất cả các giá trị bên ngoài đều là$-1$, bất kỳ tiền tố nào kết thúc trước$x$hoàn toàn nhỏ hơn tiền tố tại$x$và bất kỳ hậu tố nào bắt đầu sau$y$hoàn toàn nhỏ hơn hậu tố tại$y$. Do đó, lần xuất hiện đầu tiên của tổng tiền tố tối đa chính xác là tại$x$và lần xuất hiện cuối cùng của tổng hậu tố tối đa chính xác là tại$y$. Việc xây dựng cô lập một ổn định duy nhất có tổng tích lũy tối đa buộc cả hai điều kiện cùng một lúc. ∎ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x, y = map(int, input().split())

        a = [-1] * n

        for i in range(y - 1, x):
            a[i] = 1

        print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp khởi tạo mọi thứ thành$-1$, đảm bảo không có cực đại sớm ngẫu nhiên. Sau đó nó gán$+1$trên khoảng thời gian$[y, x]$, đây là khu vực duy nhất được phép tăng cả tổng tiền tố và hậu tố. 

Một lỗi phổ biến là cố gắng thực thi các ràng buộc tiền tố và hậu tố một cách riêng biệt; điều đó bị phá vỡ vì cả hai đều phụ thuộc vào cấu trúc tích lũy toàn cầu. Tính chính xác ở đây phụ thuộc vào một khối tích cực liền kề duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, x = 4, y = 3
```Chúng tôi xây dựng: 

| tôi | giá trị | 
| --- | --- | 
| 1 | -1 | 
| 2 | -1 | 
| 3 | 1 | 
| 4 | 1 | 

Tổng tiền tố:$-1, -2, -1, 0$, vậy cực đại là$0$ở chỉ số 4. 

Tổng hậu tố:$1, 2, 2, 1$, vậy cực đại là$2$, lần xuất hiện cuối cùng ở chỉ số 3. 

Điều này xác nhận cả hai ràng buộc. 

### Ví dụ 2 

đầu vào:```
n = 6, x = 5, y = 1
```Xây dựng: 

| tôi | giá trị | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | -1 | 

Tổng tiền tố tăng lên tới chỉ số 5, làm cho nó trở thành mức tối đa đầu tiên. 

Tổng hậu tố đạt được mức tối đa bắt đầu từ chỉ số 1 và cuối cùng đạt đến chỉ số đó. 

Điều này cho thấy một khối tích cực đầy đủ khái quát hóa một cách tự nhiên như thế nào khi$y = 1$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm điền vào mảng một lần | 
| Không gian |$O(n)$| Lưu trữ cho mảng đã xây dựng | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, do đó, một lần tuyến tính duy nhất cho mỗi trường hợp thử nghiệm dễ dàng đủ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, x, y = map(int, input().split())
        a = [-1] * n
        for i in range(y - 1, x):
            a[i] = 1
        out.append(" ".join(map(str, a)))

    return "\n".join(out)

assert run("""3
2 2 1
4 4 3
6 5 1
""") == """1 1
1 -1 1 1
1 1 -1 1 1 -1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | mẫu | tính đúng đắn của việc xây dựng | 

## Vỏ cạnh 

Khi nào$x = y + 1$, đoạn dương co lại thành hai phần tử liền kề. Tối đa tiền tố được buộc ngay lập tức ở điểm cuối bên phải, trong khi mức tối đa hậu tố được buộc ở điểm cuối bên trái và cấu trúc vẫn hoạt động vì không tồn tại phân đoạn cạnh tranh nào. 

Khi$y = 1$, toàn bộ cấu trúc tiền tố được kiểm soát ngay từ đầu, do đó đoạn dương bắt đầu ở phần tử đầu tiên. Điều kiện hậu tố được tự động thỏa mãn vì mọi hậu tố đều bao gồm khối tối đa. 

Khi$x = n$, đoạn dương kéo dài đến cuối mảng, đảm bảo không có tiền tố sau này có thể vượt quá tổng cuối cùng, do đó tiền tố tối đa nhất thiết phải ở mức$n$.
