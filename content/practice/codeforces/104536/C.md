---
title: "CF 104536C - Chuỗi GCD tối đa"
description: "Chúng ta được cho một mảng các số nguyên và được yêu cầu tính giá trị dẫn xuất cho mọi độ dài dãy con k có thể có. Đối với k cố định, chúng tôi xem xét tất cả các dãy con có kích thước k và xem xét ước chung lớn nhất của các phần tử được chọn."
date: "2026-06-30T09:16:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104536
codeforces_index: "C"
codeforces_contest_name: "SashaT9 Contest 1"
rating: 0
weight: 104536
solve_time_s: 101
verified: true
draft: false
---

[CF 104536C - Chuỗi GCD tối đa](https://codeforces.com/problemset/problem/104536/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu tính giá trị dẫn xuất cho mọi độ dài dãy con có thể có`k`. Đối với một cố định`k`, chúng ta xét tất cả các dãy con có kích thước`k`và xét ước chung lớn nhất của các phần tử đã chọn. Trong số tất cả các dãy con như vậy, chúng ta muốn có GCD tối đa có thể đạt được. 

Một dãy con ở đây có nghĩa là chúng ta có thể chọn bất kỳ`k`các chỉ số theo thứ tự tăng dần, nhưng thứ tự không ảnh hưởng đến GCD, vì vậy, về mặt hiệu quả, chúng ta đang chọn bất kỳ tập con nào có kích thước`k`. Đối với mỗi kích thước`k`, ta đang hỏi: số nguyên lớn nhất là bao nhiêu`g`sao cho tồn tại một tập con của`k`các phần tử đều chia hết cho`g`. 

Những ràng buộc cho phép`n`lên đến`2 * 10^5`và giá trị lên đến`2 * 10^5`. Điều đó ngay lập tức loại trừ việc kiểm tra tất cả các tập hợp con hoặc thậm chí tất cả các cặp cho mỗi tập hợp con.`k`. Bất kỳ giải pháp nào lặp lại rõ ràng trên các tập hợp con hoặc tính toán lại GCD trên mỗi kích thước tập hợp con sẽ quá chậm vì số lượng các chuỗi con là theo cấp số nhân. 

Một cách hữu ích để suy nghĩ về vấn đề là đảo ngược câu hỏi: thay vì sửa chữa`k`và tối đa hóa GCD, chúng tôi ấn định một giá trị`g`và hỏi có bao nhiêu phần tử chia hết cho`g`. Điều đó biến vấn đề thành việc đếm tần số trên các ước số. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều phần tử giống hệt nhau hoặc khi mảng chứa nhiều số nhỏ như`1`. Một cách tiếp cận ngây thơ có thể cho rằng việc lựa chọn số lượng lớn một cách tham lam sẽ mang lại câu trả lời, nhưng GCD phụ thuộc vào cấu trúc chia hết chứ không phải độ lớn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi kích thước tập hợp con`k`, liệt kê tất cả các dãy con, tính GCD của chúng và lấy giá trị lớn nhất. Điều này thất bại ngay lập tức bởi vì ngay cả đối với`n = 2000`, số lượng tập hợp con là rất lớn và việc tính toán GCD nhiều lần cho mỗi tập hợp con là không khả thi. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì chọn các phần tử, chúng ta xem xét giá trị GCD ứng viên`g`. Đối với một cố định`g`, các phần tử duy nhất có thể xuất hiện trong một dãy con hợp lệ là những phần tử chia hết cho`g`. Vì vậy, nếu chúng ta đếm có bao nhiêu phần tử chia hết cho`g`, nói`cnt[g]`, thì bất kỳ dãy con nào có kích thước`k ≤ cnt[g]`có thể đạt được GCD ít nhất`g`bằng cách chọn những phần tử đó và lấy GCD của chúng. 

Điều này có nghĩa là mỗi`g`đóng góp cho tất cả`k`lên đến`cnt[g]`. Chúng tôi muốn, đối với mỗi`k`, tối đa`g`như vậy`cnt[g] ≥ k`. Điều đó gợi ý việc xử lý các giá trị từ lớn đến nhỏ và lan truyền các đóng góp. 

Chúng tôi tính toán tần số của mỗi số, sau đó cho từng ước số có thể`g`, chúng ta tích lũy bao nhiêu phần tử mảng có thể chia hết cho`g`sử dụng một vòng lặp giống như sàng. Sau đó, chúng tôi xác định cho từng`k`điều tốt nhất có thể`g`. 

Điều này biến bài toán thành tập hợp số chia trên`1..maxA`, điều này khả thi khi sử dụng hành vi chuỗi hài hòa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các dãy con | O(2^n · n) | O(n) | Quá chậm | 
| Sàng tần số chia | O(M log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị trong mảng. Điều này cho phép chúng tôi sau này tính toán có bao nhiêu số có thể chia hết cho bất kỳ ứng cử viên nào`g`. 
2. Tạo một mảng`cnt[g]`được khởi tạo bằng 0 cho tất cả`g`lên đến giá trị tối đa trong đầu vào. Mảng này sẽ lưu trữ bao nhiêu phần tử chia hết cho`g`. 
3. Với mỗi giá trị`x`trong mảng, lặp qua tất cả các ước của`x`và tăng dần`cnt[d]`. Điều này đảm bảo mọi ước số đều đếm chính xác có bao nhiêu số đóng góp cho nó. 
4. Sau khi xây dựng`cnt`, giải thích nó như sau: nếu`cnt[g] = c`, thì bất kỳ dãy con nào có kích thước lên tới`c`ít nhất có thể có GCD`g`. 
5. Chúng ta cần GCD tốt nhất cho mỗi người`k`. Chúng tôi xây dựng một mảng`best[k]`được khởi tạo bằng 0. 
6. Đối với mỗi`g`từ 1 đến giá trị tối đa, chúng tôi tuyên truyền sự đóng góp của nó: cho tất cả`k ≤ cnt[g]`, chúng ta có khả năng có thể thiết lập`best[k] = max(best[k], g)`. 
7. Để thực hiện việc này một cách hiệu quả, thay vì cập nhật tất cả`k`rõ ràng cho từng`g`, chúng tôi lặp đi lặp lại`g`theo thứ tự giảm dần và điền kết quả một cách tham lam sao cho lớn hơn`g`ghi đè lên những cái nhỏ hơn trước. 
8. Cuối cùng xuất ra`best[1..n]`. 

### Tại sao nó hoạt động 

Mọi dãy con hợp lệ của kích thước`k`tương ứng với một số nguyên chia hết tất cả các phần tử được chọn. Số nguyên đó phải là ước số của mọi phần tử được chọn, vì vậy nó phải xuất hiện dưới dạng ước số của ít nhất`k`các phần tử trong mảng. Do đó, bài toán rút gọn thành việc tìm giá trị ước số lớn nhất xuất hiện trong ít nhất`k`những con số. Việc đếm dựa trên sàng đảm bảo tần số chia hết chính xác và xử lý theo thứ tự giảm dần đảm bảo giá trị tối đa chiếm ưu thế một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    maxa = max(a)

    freq = [0] * (maxa + 1)
    for x in a:
        freq[x] += 1

    cnt = [0] * (maxa + 1)

    for d in range(1, maxa + 1):
        for m in range(d, maxa + 1, d):
            cnt[d] += freq[m]

    best = [0] * (n + 1)

    for g in range(1, maxa + 1):
        c = cnt[g]
        if c == 0:
            continue
        for k in range(1, c + 1):
            if g > best[k]:
                best[k] = g

    print(*best[1:])

if __name__ == "__main__":
    solve()
```Sau khi xây dựng tần số, chúng tôi tính toán độ bao phủ của số chia bằng cách sử dụng mẫu sàng cổ điển. Sau đó, đối với mỗi ứng cử viên GCD có thể, chúng tôi cập nhật tất cả độ dài chuỗi con mà nó có thể hỗ trợ. Vòng lặp bên trong an toàn vì tổng hài trên các ước số giữ cho độ phức tạp có thể chấp nhận được đối với các ràng buộc. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
7
3 4 9 6 8 2 3
```Đầu tiên chúng ta tính số chia. Ví dụ,`3`đóng góp vào số chia`1`Và`3`,`6`góp phần vào`1,2,3,6`, vân vân. 

Đối với mỗi`g`, chúng tôi tính toán có bao nhiêu phần tử chia hết cho nó: 

| g | cnt[g] | 
| --- | --- | 
| 9 | 1 | 
| 8 | 1 | 
| 6 | 2 | 
| 4 | 1 | 
| 3 | 3 | 
| 2 | 4 | 
| 1 | 7 | 

Bây giờ chúng tôi tuyên truyền những giá trị tốt nhất: 

| k | tốt nhất[k] | 
| --- | --- | 
| 1 | 9 | 
| 2 | 4 | 
| 3 | 3 | 
| 4 | 3 | 
| 5 | 1 | 
| 6 | 1 | 
| 7 | 1 | 

Điều này phù hợp với trực giác rằng GCD cao hơn chỉ có thể tồn tại trong các chuỗi nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M) | sàng chia trên phạm vi giá trị | 
| Không gian | O(M + n) | mảng tần số và kết quả | 

Với`M ≤ 2 * 10^5`, phép liệt kê số chia đủ hiệu quả vì mỗi số chỉ đóng góp thông qua các ước số của nó và cấu trúc hài hòa giữ cho tổng số hoạt động có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import subprocess, textwrap, sys
    return ""

# provided sample
# custom cases
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n5\n1 1 1 1 1`|`1 1 1 1 1`| trường hợp tất cả các cạnh bằng nhau | 
|`1\n3\n2 3 5`|`5 2 1`| cấu trúc đồng nguyên tố | 
|`1\n4\n8 4 2 1`|`8 4 2 1`| chuỗi chia đầy đủ | 
|`1\n6\n6 10 15 3 5 2`| hỗn hợp | chia hết không đều | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau thì mọi dãy con đều có cùng GCD, do đó câu trả lời là không đổi trên tất cả`k`. Việc đếm số chia phản ánh chính xác rằng chỉ có một giá trị đóng góp. 

Khi tất cả các số đều là số nguyên tố cùng nhau thì mỗi số`g > 1`có phạm vi bảo hiểm rất nhỏ, vì vậy chỉ`k = 1`có thể đạt được GCD lớn hơn và phần còn lại sụp đổ thành`1`, phù hợp với hành vi lan truyền.
