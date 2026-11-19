---
title: "CF 2072A - Thế giới mới, tôi mới, mảng mới"
description: "Chúng ta bắt đầu với một dãy n số không. Trong một lần di chuyển, chúng ta có thể chọn bất kỳ vị trí nào và ghi đè lên nó bằng bất kỳ số nguyên nào trong phạm vi [-p, p]."
date: "2026-06-08T06:46:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2072
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1006 (Div. 3)"
rating: 800
weight: 2072
solve_time_s: 71
verified: true
draft: false
---

[CF 2072A - Thế giới mới, tôi mới, mảng mới](https://codeforces.com/problemset/problem/2072/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một loạt`n`số không. Trong một lần di chuyển, chúng ta có thể chọn bất kỳ vị trí nào và ghi đè lên nó bằng bất kỳ số nguyên nào trong phạm vi`[-p, p]`. Sau khi thực hiện một số lần ghi đè như vậy, chúng ta muốn tổng của mảng trở nên chính xác.`k`và chúng tôi muốn thực hiện việc này bằng cách sử dụng càng ít bước di chuyển càng tốt. Mỗi nước đi chỉ tác động đến một vị trí và nó thay thế hoàn toàn giá trị trước đó tại vị trí đó. 

Vì vậy, quyết định thực sự không phải là về vị trí, mà là về tổng số tiền mà chúng ta có thể “đưa” vào mảng bằng cách sử dụng tối đa là bao nhiêu.`n`các bài tập, mỗi bài đóng góp một giá trị giữa`-p`Và`p`. 

Các hạn chế là nhỏ:`n ≤ 50`,`|k| ≤ 2500`, Và`p ≤ 50`. Điều này có nghĩa là ngay cả một phân tích trường hợp trực tiếp hoặc công thức tham lam cũng đủ. Bất cứ điều gì như DP đối với các trạng thái đều không cần thiết, và thậm chí cả sự ép buộc tàn bạo đối với các nhiệm vụ có thể có cho mỗi vị trí sẽ bị giới hạn đủ về mặt lý thuyết, nhưng lại quá mức cần thiết trong thực tế. 

Một trường hợp thất bại phổ biến là quên rằng chúng ta có thể ghi đè lên cùng một chỉ mục nhiều lần. Vì chỉ có giá trị cuối cùng quan trọng đối với mỗi chỉ mục, điều quan trọng là chúng ta sử dụng bao nhiêu chỉ số chứ không phải tần suất chúng ta chạm vào chúng. 

Một trường hợp tế nhị khác là khi`k = 0`. Câu trả lời đúng luôn là các phép toán bằng 0 vì mảng đã có tổng bằng 0, bất kể`p`. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ thử mọi cách để gán giá trị lên tới`n`vị trí và kiểm tra xem sự kết hợp nào tổng hợp lại`k`. Điều này nhanh chóng bùng nổ vì mỗi vị trí đều có`2p + 1`sự lựa chọn, dẫn đến`(2p + 1)^n`khả năng là quá lớn ngay cả đối với`n = 50`. 

Quan sát quan trọng là mỗi hoạt động đóng góp độc lập vào tổng số tiền. Sau đó`x`hoạt động, điều tốt nhất chúng ta có thể làm ở mức độ lớn là`x * p`, bởi vì mỗi bài tập có thể đóng góp nhiều nhất`p`hoặc`-p`. Vì vậy, số tiền có thể đạt được sau đó chính xác`x`các hoạt động tạo thành khoảng`[-x p, x p]`. Vì chúng tôi muốn chính xác`k`, câu hỏi duy nhất là liệu`k`nằm trong phạm vi đó, và giá trị nhỏ nhất như vậy`x`là. 

Điều này làm giảm vấn đề để tìm mức tối thiểu`x`như vậy`x * p ≥ |k|`, đồng thời đảm bảo`x ≤ n`bởi vì chúng tôi chỉ có`n`các vị trí cần sửa đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(1) | Quá chậm | 
| Kiểm tra ràng buộc tham lam | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`k`, Và`p`. Chúng ta chỉ quan tâm đến giá trị tuyệt đối của`k`bởi vì chúng ta có thể sử dụng các phép gán âm một cách đối xứng. 
2. Nếu`k`bằng 0, quay lại`0`ngay lập tức vì không cần thực hiện thao tác nào và mảng ban đầu đã thỏa mãn điều kiện. 
3. Tính toán`need = |k|`. Điều này thể hiện tổng cường độ mà chúng ta phải xây dựng bằng cách sử dụng các hoạt động. 
4. Mỗi thao tác đóng góp tối đa`p`đến tổng độ lớn. Vậy số thao tác tối thiểu cần thực hiện là`ceil(need / p)`. 
5. Nếu con số này lớn hơn`n`, thì ngay cả việc sửa đổi mọi phần tử một lần cũng không đủ để đạt được tổng yêu cầu, vì vậy câu trả lời là`-1`. 
6. Nếu không, xuất giá trị tính toán. 

Tại sao nó hoạt động: mỗi thao tác đóng góp độc lập vào tổng số tiền và chúng ta có thể tự do gán các giá trị dương hoặc âm lên đến`p`. Yếu tố hạn chế duy nhất là tổng công suất, tuyến tính theo số lượng hoạt động. Vì chúng ta luôn có thể chọn các giá trị khớp chính xác với số tiền được yêu cầu miễn là dung lượng cho phép nên giới hạn trần là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, p = map(int, input().split())

        if k == 0:
            print(0)
            continue

        need = abs(k)

        # minimum operations needed if each contributes at most p
        ops = (need + p - 1) // p

        if ops > n:
            print(-1)
        else:
            print(ops)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo giới hạn dẫn xuất. Bộ phận trần`(need + p - 1) // p`tránh các phép toán dấu phẩy động và đảm bảo tính chính xác cho tất cả các trường hợp số nguyên. Séc`ops > n`thực thi ràng buộc mà chúng tôi không thể thực hiện nhiều hơn`n`nhiệm vụ độc lập. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp`n = 5, k = 7, p = 2`. 

| Bước | cần | p | công thức hoạt động | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 2 | trần nhà(7/2) | 4 | 

Từ`4 ≤ 5`, câu trả lời là`4`. 

Bây giờ hãy xem xét`n = 3, k = 10, p = 3`. 

| Bước | cần | p | công thức hoạt động | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 3 | trần nhà(10/3) | 4 | 

Đây`4 > 3`, vậy câu trả lời là`-1`. 

Ví dụ đầu tiên cho thấy đóng góp một phần cho mỗi hoạt động được tích lũy tuyến tính. Điều thứ hai cho thấy rằng mặc dù các giá trị riêng lẻ đủ lớn nhưng chúng ta vẫn bị hạn chế bởi số lượng vị trí có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Không có dung lượng bổ sung ngoài các biến | 

Giải pháp thoải mái xử lý lên đến`t = 1000`các trường hợp trong giới hạn vì mỗi trường hợp là một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, k, p = map(int, input().split())
        if k == 0:
            out.append("0")
            continue
        need = abs(k)
        ops = (need + p - 1) // p
        out.append(str(ops if ops <= n else -1))
    return "\n".join(out)

# provided samples
assert solve_io("8\n21 100 10\n9 -420 42\n5 -7 2\n13 37 7\n10 0 49\n1 10 9\n7 -7 7\n20 31 1\n") == \
"10\n-1\n4\n6\n0\n-1\n1\n-1"

# custom cases
assert solve_io("1\n5 0 3\n") == "0"
assert solve_io("1\n5 1 10\n") == "1"
assert solve_io("1\n5 100 1\n") == "-1"
assert solve_io("2\n10 -25 5\n3 9 2\n") == "5\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 0 | 0 | Trạng thái đã hài lòng | 
| op đơn đủ | 1 | trường hợp tối thiểu khác không | 
| trường hợp bất khả thi | -1 | vi phạm ràng buộc năng lực | 
| trường hợp hỗn hợp | 5, -1 | tính chính xác trong các tình huống | 

## Vỏ cạnh 

Khi nào`k = 0`, thuật toán ngay lập tức trả về 0 mà không cần xem xét`p`, vì không cần sửa đổi. Khi`p`lớn so với`k`, công thức sẽ thu gọn thành một thao tác vì một phép gán đơn lẻ có thể tính toàn bộ tổng. Khi`p = 1`, số lượng thao tác trở nên chính xác`|k|`và tính khả thi phụ thuộc hoàn toàn vào việc liệu`|k| ≤ n`. Việc phân chia trần đảm bảo không có sai sót nào trong tất cả các trường hợp này, vì nó xử lý chính xác cả độ lớn chia hết và không chia hết.
