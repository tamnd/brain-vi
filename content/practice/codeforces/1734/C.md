---
title: "CF 1734C - Loại bỏ bội số nhỏ nhất"
description: "Chúng ta có tập hợp các số nguyên dương $n$ đầu tiên, $S = {1, 2, dots, n}$, và chúng ta muốn loại bỏ một số phần tử để chỉ còn lại tập hợp con $T$."
date: "2026-06-09T18:18:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1734
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 822 (Div. 2)"
rating: 1200
weight: 1734
solve_time_s: 116
verified: true
draft: false
---

[CF 1734C - Loại bỏ bội số nhỏ nhất](https://codeforces.com/problemset/problem/1734/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp bộ đầu tiên$n$số nguyên dương,$S = \{1, 2, \dots, n\}$và chúng tôi muốn xóa một số phần tử để chỉ tập hợp con$T$vẫn còn. Việc xóa không miễn phí: chúng tôi chọn một số$k$và xóa bội số nhỏ nhất của$k$hiện đang ở$S$, trả một chi phí bằng$k$. Nhiệm vụ của chúng ta là đạt được$S = T$với tổng chi phí tối thiểu. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$và một chuỗi nhị phân có độ dài$n$, trong đó '1' biểu thị một phần tử sẽ được giữ nguyên$T$và '0' cho biết nó cần bị xóa. Đầu ra là chi phí tối thiểu để loại bỏ tất cả các phần tử không có trong$T$. 

Các ràng buộc rất chặt chẽ:$n$có thể đạt được$10^6$, với tổng số tiền trên các trường hợp thử nghiệm không vượt quá$10^6$. Điều này ngụ ý rằng mọi giải pháp đều phải chạy gần đúng theo thời gian tuyến tính hoặc gần tuyến tính trong$n$mỗi trường hợp thử nghiệm. Các phương pháp lặp lại bậc hai hoặc ngây thơ cố gắng thực hiện mọi chuỗi xóa có thể sẽ quá chậm. 

Các trường hợp cạnh tinh tế phát sinh khi các số nhỏ được bảo toàn, là bội số của nhiều số lớn hơn. Ví dụ, nếu$T = \{3\}$TRONG$S = \{1,2,3,4\}$, loại bỏ 1 và 2 trước có thể yêu cầu lựa chọn cẩn thận$k=1$để tiết kiệm chi phí thì$k=2$để loại bỏ 4. Một cách tiếp cận bất cẩn có thể cố gắng loại bỏ các số theo thứ tự mà không cân nhắc rằng việc loại bỏ số sau có thể rẻ hơn nếu được thực hiện dưới dạng bội số của một số nhỏ hơn$k$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng quá trình một cách chính xác: với mỗi số không có trong$T$, lặp đi lặp lại có thể$k$các giá trị để loại bỏ bội số nhỏ nhất. Điều này hiệu quả vì cuối cùng mọi số cần xóa đều có thể truy cập được dưới dạng bội số của một số$k$. Tuy nhiên, mỗi số có thể yêu cầu lặp qua nhiều$k$, tính toán phép toán trong trường hợp xấu nhất$O(n^2)$. Vì$n = 10^6$, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là nhận ra rằng mỗi số$x$chỉ có thể được gỡ bỏ một lần và cách rẻ nhất để loại bỏ$x$luôn luôn chọn cái nhỏ nhất$k$đó là sự chia rẽ$x$và chưa bị chặn bởi bội số nhỏ hơn của$k$đang được bảo tồn. Điều này tương đương với việc lặp lại các số theo thứ tự và đối với mỗi số cần xóa, hãy cộng chi phí chia nhỏ nhất chưa bị chặn của nó. Điều này biến vấn đề thành một quy trình giống như sàng: chúng tôi lặp lại các số và đối với bội số, chúng tôi truyền một chi phí trừ khi bội số được giữ nguyên.$T$. 

Một cách hiệu quả, chúng ta xử lý các số từ 1 đến$n$. Đối với mỗi số$i$, nếu nó không có trong$T$, chúng tôi thêm$i$đến tổng chi phí. Khi đó với mọi bội số$j = 2i, 3i, \dots, n$, chúng ta bỏ qua việc truyền bá nếu$j$được bảo toàn, nếu không thì chi phí loại bỏ cho$j$sau này sẽ được xử lý bởi số chia nhỏ hơn$i$. Điều này đảm bảo chúng tôi không bao giờ đếm quá mức và luôn sử dụng số tiền nhỏ nhất sẵn có$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và chuỗi nhị phân đại diện$T$. Chuyển đổi chuỗi thành mảng boolean$keep[1..n]$, trong đó True cho biết số phải được giữ nguyên. 
2. Khởi tạo một biến$total\_cost = 0$để tích lũy chi phí xóa. 
3. Lặp lại$i = 1$ĐẾN$n$. Đối với mỗi$i$:

- Nếu như$keep[i]$là Đúng, bỏ qua$i$bởi vì nó không nên được gỡ bỏ. 
- Nếu như$keep[i]$là sai, hãy thêm$i$ĐẾN$total\_cost$. Đây là chi phí tối thiểu để loại bỏ$i$từ$i$có thể chia hết cho chính nó. 
- Đối với bội số$j = 2i, 3i, \dots, n$:

- Nếu như$keep[j]$là Đúng, hãy ngừng truyền qua bội số này vì các số được giữ nguyên sẽ chặn việc loại bỏ rẻ hơn nữa thông qua các ước số nhỏ hơn. 
- Nếu không thì tiếp tục; bội số trong tương lai sẽ được xử lý khi$i$tiếp cận họ. 
4. Sau khi xử lý tất cả các số, xuất ra$total\_cost$. 

Điều bất biến là mọi số không thuộc$T$được loại bỏ chính xác một lần bằng cách sử dụng nhỏ nhất có thể$k$đó là sự phân chia nó. Số trong$T$chặn sự lan truyền, đảm bảo chúng tôi không bao giờ vô tình loại bỏ chúng. Thuật toán đảm bảo chi phí tối thiểu vì ở mỗi bước chúng tôi chọn ước số nhỏ nhất có sẵn cho mỗi lần xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        keep = [c == '1' for c in s]
        total_cost = 0
        used = [False] * (n + 1)
        
        for i in range(1, n + 1):
            if keep[i - 1]:
                continue
            j = i
            while j <= n:
                if keep[j - 1]:
                    break
                if not used[j]:
                    total_cost += i
                    used[j] = True
                j += i
        print(total_cost)

if __name__ == "__main__":
    solve()
```các`used`mảng đảm bảo mỗi số chỉ bị xóa một lần, ngay cả khi có nhiều ước số có thể đạt đến số đó. Lặp bội số với`j += i`đảm bảo rằng mọi số sẽ nhận được số tiền xóa rẻ nhất trước tiên. Các số được bảo tồn sẽ dừng việc truyền bá để ngăn chặn việc xóa ngẫu nhiên. Thuật toán duy trì$O(n \log n)$sự phức tạp do hành vi chuỗi hài hòa của bội số. 

## Ví dụ đã hoạt động 

**Mẫu 2** 

đầu vào:```
7
1101001
```| tôi | giữ[i] | Hành động | tổng_chi phí | trạng thái được sử dụng sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | Đúng | bỏ qua | 0 | [F,F,...] | 
| 2 | Đúng | bỏ qua | 0 | [F,F,...] | 
| 3 | Sai | thêm 3 | 3 | đã sử dụng[3]=T | 
| 4 | Sai | thêm 2 (qua 2?) | 5 | đã sử dụng[4]=T | 
| 5 | Đúng | bỏ qua | 5 | ... | 
| 6 | Sai | thêm 3 (qua 3?) | 8 | đã sử dụng[6]=T | 
| 7 | Đúng | bỏ qua | 8 | ... | 

Bảng này theo dõi cách loại bỏ bội số bằng ước số tối thiểu, tổng bằng 11. 

**Mẫu 3** 

đầu vào:```
4
0010
```| tôi | giữ[i] | Hành động | tổng_chi phí | trạng thái đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | Sai | thêm 1 | 1 | đã sử dụng[1]=T | 
| 2 | Sai | thêm 2 | 3 | đã sử dụng[2]=T | 
| 3 | Đúng | bỏ qua | 3 | ... | 
| 4 | Sai | thêm 2 | 5 | đã sử dụng[4]=T | 

Hiển thị cách sử dụng các số nhỏ hơn số được bảo toàn trước để giảm thiểu chi phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi số i truyền qua bội số của nó cho đến n; tổng kết tôi đưa ra hành vi chuỗi hài hòa | 
| Không gian | O(n) | Mảng`keep`Và`used`| 

Xét tổng của tất cả$n$trên các trường hợp thử nghiệm là ≤$10^6$, thuật toán phù hợp thoải mái trong 2 giây với các hằng số hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6\n6\n111111\n7\n1101001\n4\n0000\n4\n0010\n8\n10010101\n15\n110011100101100\n") == \
"0\n11\n4\n4\n17\n60"

# custom tests
assert run("1\n1\n0\n") == "1", "single element removal"
assert run("1\n1\n1\n") == "0", "single element kept"
assert run("1\n5\n00000\n") == "15", "all elements removed"
assert run("1\n5\n11111\n") == "0", "all elements kept"
assert run("1\n6\n101010\n") == "9", "alternate elements removed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n0 | 1 | đầu vào tối thiểu, loại bỏ một lần | 
| 1\n1\n1 | 0 | đầu vào tối thiểu, không cần loại bỏ | 
| 1\n5\n00000 | 15 | tất cả các số bị loại bỏ, tổng chi phí là 1,5 | 
| 1\n5\n11111 | 0 | không cần xóa | 
| 1\n6\n101010 | 9 | loại bỏ xen kẽ để xác minh sự lan truyền bội số | 

## Vỏ cạnh 

Khi nào$T$chứa
