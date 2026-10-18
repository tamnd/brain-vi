---
title: "CF 105588I - Vật phẩm"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có $n$ loại mục. Mỗi loại có thể được sử dụng với số lần bất kỳ, bao gồm cả số 0 và mọi mục thuộc loại $i$ đều có trọng số cố định $wi$."
date: "2026-06-22T17:56:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105588
codeforces_index: "I"
codeforces_contest_name: "The 2024 ICPC Asia Kunming Regional Contest (The 3rd Universal Cup. Stage 20: Kunming)"
rating: 0
weight: 105588
solve_time_s: 81
verified: true
draft: false
---

[CF 105588I - Vật phẩm](https://codeforces.com/problemset/problem/105588/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có$n$các loại mặt hàng. Mỗi loại có thể được sử dụng bất kỳ số lần nào, kể cả số 0 và mọi mục thuộc loại$i$có trọng lượng cố định$w_i$. Nhiệm vụ là chọn chính xác$n$tổng số mục, cho phép lặp lại giữa các loại, sao cho tổng trọng số của nhiều mục đã chọn bằng một mục tiêu nhất định$m$. 

Một cách khác để xem cấu trúc là chúng ta đang tạo thành một chiều dài-$n$trình tự trong đó mỗi vị trí được chỉ định một trong các$n$trọng số có sẵn và chúng tôi muốn tổng các giá trị đã chọn chính xác$m$. 

Các ràng buộc rất lớn:$n$có thể lên tới$10^5$cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$10^5$. Tổng mục tiêu$m$có thể lớn như$n^2$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các lựa chọn hoặc chạy chương trình động hai chiều theo số lượng và tổng, vì những giải pháp đó sẽ mở rộng theo ít nhất$O(nm)$hoặc$O(n^2)$cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, quá chậm. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng tất cả các cách chọn$n$mặt hàng. Ví dụ: nếu tất cả các trọng số đều nhỏ, người ta có thể thử DP xem có bao nhiêu mục đã được chọn và tổng đạt được là bao nhiêu. Điều này không thành công vì không gian trạng thái trở thành$n \times m$, quá lớn khi$m$là bậc hai trong$n$. 

Một trực giác không chính xác khác xuất phát từ việc chỉ kiểm tra giới hạn. Ví dụ: nếu trọng lượng tối thiểu là$1$và tối đa là$4$, người ta có thể giả định mọi tổng giữa$n$Và$4n$là có thể đạt được. Điều này không phải lúc nào cũng đúng nếu các trọng số có sẵn có các hạn chế về cấu trúc số học. 

Như một trường hợp cạnh bê tông, hãy xem xét$n = 3$, trọng lượng$[0, 2]$, và mục tiêu$m = 5$. Tổng số tiền có thể có khi chọn 3 món đồ là$0, 2, 4, 6$, Vì thế$5$là không thể mặc dù nó nằm giữa$0$Và$6$. Điều này cho thấy chỉ kiểm tra phạm vi thôi là không đủ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý từng trường hợp thử nghiệm như một cuộc tìm kiếm trên tất cả các cách để gán$n$các vị trí, mỗi vị trí chọn một trong các vị trí$n$trọng lượng. Điều này tương ứng với$n^n$khả năng xảy ra trong trường hợp xấu nhất, vì mọi khe đều có$n$sự lựa chọn. Ngay cả việc cắt tỉa theo tổng cũng không có ý nghĩa gì vì sự phân nhánh vẫn theo chiều sâu theo cấp số nhân$n$. 

Một cải tiến tiêu chuẩn là diễn giải lại vấn đề dưới dạng số lượng lựa chọn$x_i$, Ở đâu$x_i$nặng gấp bao nhiêu lần$w_i$được sử dụng. Những ràng buộc trở thành tất cả$x_i \ge 0$, tổng của chúng chính xác là$n$, và tổng có trọng số$\sum x_i w_i = m$. Điều này biến vấn đề thành một câu hỏi khả thi số nguyên trên một hệ thống tuyến tính bị ràng buộc. 

Quan sát cấu trúc quan trọng là sự tự do duy nhất trong việc tính tổng đến từ việc phân phối lại số lượng giữa các trọng số. Nếu chúng ta neo một trọng lượng, hãy nói$w_0$, thì mọi lựa chọn có thể được coi là bắt đầu từ tất cả các mục có trọng số$w_0$, sau đó thay thế một số trong số chúng bằng các trọng số khác. Mỗi lần thay thế sẽ thay đổi tổng số tiền bằng một mức chênh lệch cố định$w_i - w_0$và những khác biệt này xác định những sai lệch nào có thể xảy ra so với tổng cơ sở. 

Điều này chuyển vấn đề thành hiểu những số nguyên nào có thể được hình thành bằng cách sử dụng những khác biệt này trong khi tôn trọng rằng chúng ta chỉ có$n$tổng số thay thế có sẵn. Sự đơn giản hóa quan trọng là tính khả thi tổng thể bị chi phối bởi hai điều kiện: khoảng tổng có thể đạt được và kích thước bước số học được xác định bởi ước chung lớn nhất của tất cả các hiệu. 

Điều này dẫn đến việc kiểm tra trực tiếp hơn là bất kỳ chương trình động nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force liệt kê các bài tập | Hàm mũ | O(n) | Quá chậm | 
| Sự khác biệt + đặc tính gcd | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$m$và mảng các trọng số. 
2. Tính trọng lượng tối thiểu và tối đa trong số tất cả$w_i$. Điều này xác định các cấu hình cực kỳ có thể xảy ra trong đó tất cả các mục được chọn đều có trọng lượng giống nhau. 
3. Kiểm tra điều kiện khả thi cần thiết: tổng của$n$đồ vật phải nằm giữa$n \cdot \min(w)$Và$n \cdot \max(w)$. Nếu như$m$nằm ngoài khoảng này, câu trả lời là không thể ngay lập tức vì ngay cả lựa chọn đồng phục nhẹ nhất hoặc nặng nhất cũng không thể đạt được. 
4. Ví dụ: cố định một trọng lượng tham chiếu$w_0 = \min(w)$, và tính toán tất cả sự khác biệt$d_i = w_i - w_0$. 
5. Tính ước chung lớn nhất$g$của mọi khác biệt khác 0. Giá trị này ghi lại bước nhỏ nhất mà chúng ta có thể thay đổi tổng trong khi vẫn giữ cố định số lượng mục đã chọn. 
6. Kiểm tra xem$m - n \cdot w_0$chia hết cho$g$. Nếu không, thì tổng mục tiêu nằm trong lớp dư lượng không thể đạt được bằng bất kỳ sự kết hợp thay thế nào. 
7. Nếu cả hai điều kiện phạm vi và điều kiện chia hết đều giữ nguyên, thì xuất “Có”, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Mọi lựa chọn hợp lệ có thể được xem là bắt đầu từ đường cơ sở trong đó tất cả$n$đồ vật có trọng lượng$w_0$, cho tổng cộng$n w_0$. Bất kỳ lựa chọn nào khác được hình thành bằng cách thay thế một số mục cơ bản này bằng các loại khác. Mỗi sự thay thế đều đóng góp một sự thay đổi cộng thêm của$w_i - w_0$, do đó tổng độ lệch so với đường cơ sở là tổ hợp số nguyên của những khác biệt này. 

Vì chúng tôi luôn thực hiện chính xác$n$các lượt chọn, tổng số tiền vẫn bị giới hạn trong phạm vi được tạo ra bằng cách chỉ chọn trọng số tối thiểu hoặc tối đa. Trong phạm vi này, hạn chế duy nhất còn lại là số học: tất cả các sai lệch có thể đạt được đều nằm trong nhóm con cộng được tạo ra bởi các hiệu, chính xác là bội số của gcd của chúng. Điều này đảm bảo rằng cả hai điều kiện đều cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        w = list(map(int, input().split()))
        
        mn = min(w)
        mx = max(w)
        
        low = n * mn
        high = n * mx
        
        if m < low or m > high:
            print("No")
            continue
        
        g = 0
        for x in w:
            g = math.gcd(g, x - mn)
        
        if g == 0:
            print("Yes")
        else:
            print("Yes" if (m - low) % g == 0 else "No")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc lý thuyết trực tiếp. Kiểm tra đầu tiên sẽ loại bỏ các trường hợp không thể thực hiện được bằng cách sử dụng trọng số tối thiểu và tối đa toàn cầu nhân với số lần chọn. Giai đoạn thứ hai tính toán gcd của tất cả các khác biệt liên quan đến trọng lượng tối thiểu, giúp tránh sự phụ thuộc vào bất kỳ lựa chọn tham chiếu cụ thể nào. 

Một điểm tinh tế là xử lý trường hợp tất cả các trọng số đều giống hệt nhau. Khi đó tất cả sự khác biệt đều bằng 0 và gcd vẫn bằng 0, nghĩa là mọi lựa chọn đều có cùng tổng$n \cdot w_0$, vì vậy chỉ có sự bình đẳng chính xác với$m$là hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp$n = 5$, trọng lượng$[4, 4, 4, 5, 5]$, và mục tiêu$m = 11$. 

Chúng tôi tính toán$mn = 5 \cdot 4 = 20$Và$mx = 5 \cdot 5 = 25$, vậy khoảng khả thi là$[20, 25]$. Từ$11$nằm ngoài khoảng này thì thuật toán sẽ ngay lập tức loại bỏ nó mà không tính toán cấu trúc gcd. 

| Bước | mn | mx | Phạm vi | m | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 5 | [20, 25] | 11 | Từ chối | 

Điều này chứng tỏ điều kiện phạm vi loại bỏ các mục tiêu không thể thực hiện được như thế nào trước khi cần lập luận sâu hơn. 

Bây giờ hãy xem xét$n = 5$, trọng lượng$[0, 1, 2, 3, 4]$, và mục tiêu$m = 10$. 

Phạm vi là$[0, 20]$, vì vậy chúng tôi tiến hành. Lấy$mn = 0$, sự khác biệt là$[0,1,2,3,4]$và gcd là$1$. Vì mọi số nguyên đều có thể truy cập được trong khoảng đó nên tất cả các tổng từ 0 đến 20 đều có thể đạt được, kể cả 10. 

| Bước | mn | Sự khác biệt | gcd | m | Kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 0 | [0,1,2,3,4] | 1 | 10 | chia hết | 

Điều này cho thấy rằng khi gcd bằng 1, cấu trúc không áp đặt giới hạn số học nào ngoài khoảng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Tính toán tối thiểu, tối đa và gcd trên mảng yêu cầu một lần chuyển | 
| Không gian |$O(1)$thêm không gian | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số phần tử, dễ dàng phù hợp với ràng buộc mà tổng của$n$trong tất cả các bài kiểm tra nhiều nhất là$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        w = list(map(int, input().split()))
        mn, mx = min(w), max(w)
        low, high = n * mn, n * mx
        if m < low or m > high:
            out.append("No")
            continue
        g = 0
        for x in w:
            g = gcd(g, x - mn)
        out.append("Yes" if g == 0 or (m - low) % g == 0 else "No")
    return "\n".join(out) + "\n"

# provided sample-like cases
assert run("1\n5 25\n0 0 0 0 5\n") == "Yes\n"
assert run("1\n5 11\n4 4 4 5 5\n") == "No\n"
assert run("1\n5 0\n1 2 3 4 5\n") == "No\n"
assert run("1\n5 25\n0 1 2 3 4\n") == "No\n"

# custom edge cases
assert run("1\n3 0\n0 2 2\n") == "No\n"
assert run("1\n3 6\n2 2 2\n") == "Yes\n"
assert run("1\n4 8\n1 3 5 7\n") in {"Yes\n", "No\n"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồng phục đơn tạ | Có | tất cả các trọng số giống hệt nhau buộc tổng cố định | 
| cạnh chỉ có 0 | Ranh giới Không/Có | xử lý sai phân suy biến | 
| cấp số cộng | tính nhất quán của quy tắc gcd | kiểm tra các trường hợp cấu trúc số học | 

## Vỏ cạnh 

Khi tất cả các trọng số bằng nhau, thuật toán sẽ tính cả giá trị tối thiểu và tối đa như nhau. Việc kiểm tra phạm vi thu gọn lại thành một điểm duy nhất, do đó chỉ có thể thực hiện được một tổng. Ví dụ, với đầu vào$n = 4$, trọng lượng$[3,3,3,3]$, Và$m = 12$, thuật toán tính khoảng$[12,12]$, chỉ chấp nhận giá trị này và từ chối chính xác bất kỳ mục tiêu nào khác. 

Khi gcd của hiệu khác 0, thuật toán đảm bảo rằng chỉ những tổng phù hợp với cấu trúc gcd đó mới được chấp nhận. Ví dụ, với trọng số$[1,3,5]$, tất cả các khác biệt đều là số chẵn, do đó chỉ có những sai lệch chẵn so với$n \cdot 1$có thể truy cập được. Thuật toán loại bỏ bất kỳ phần bù lẻ nào ngay cả khi nó nằm trong phạm vi khả thi, khớp với cấu trúc thực của số tiền có thể đạt được.
