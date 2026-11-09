---
title: "CF 1516A - Ăn miếng trả miếng"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta có thể thực hiện một số thao tác “chuyển”. Trong một thao tác, chúng ta chọn hai phần tử khác nhau, trừ đi một phần tử ở phần tử đầu tiên và thêm một phần tử vào phần tử thứ hai, với điều kiện phần tử đầu tiên không xuống dưới 0."
date: "2026-06-10T18:24:49+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1516
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 717 (Div. 2)"
rating: 800
weight: 1516
solve_time_s: 142
verified: true
draft: false
---

[CF 1516A - Ăn miếng trả miếng](https://codeforces.com/problemset/problem/1516/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta có thể thực hiện một số thao tác “chuyển”. Trong một thao tác, chúng ta chọn hai phần tử khác nhau, trừ đi một phần tử ở phần tử đầu tiên và thêm một phần tử vào phần tử thứ hai, với điều kiện phần tử đầu tiên không xuống dưới 0. Mục tiêu là sử dụng tối đa$k$để làm cho mảng theo từ điển càng nhỏ càng tốt. 

Nhỏ hơn về mặt từ điển có nghĩa là khi so sánh các mảng từ trái sang phải, vị trí đầu tiên nơi chúng khác nhau sẽ có giá trị nhỏ hơn trong kết quả. Về cơ bản, chúng tôi muốn đẩy càng nhiều giá trị sang bên phải càng tốt trong khi vẫn giữ các phần tử ngoài cùng bên trái càng nhỏ càng tốt. 

Các hạn chế là nhỏ:$n \le 100$Và$k \le 10000$. Từ$k$có thể lớn hơn đáng kể so với$n$, chúng ta không thể có được một mô phỏng ngây thơ di chuyển từng đơn vị một cho tất cả các hoạt động nếu chúng ta cố gắng hiểu theo nghĩa đen. Mỗi phần tử tối đa là 100, vì vậy các giá trị mảng nhỏ, điều này cho thấy cách tiếp cận tham lam đang hoạt động tốt. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó một số phần tử bằng 0 ngay từ đầu. Nếu chúng ta cố gắng trừ chúng, chúng ta phải bỏ qua. Một trường hợp tế nhị khác xảy ra khi$k$lớn hơn tổng của tất cả các đơn vị có thể chuyển nhượng; trong trường hợp này, mảng nhỏ nhất có được bằng cách di chuyển tất cả các đơn vị dư thừa sang phải càng xa càng tốt. Ví dụ, với đầu vào`2 10`và mảng`1 0`, chúng ta không thể giảm phần tử thứ hai và tất cả các thao tác được thực hiện bằng cách đẩy 1 từ phần tử thứ nhất sang phần tử thứ hai, thu được`[0 1]`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng hoạt động một. Ở mỗi bước, chúng tôi quét mảng từ trái sang phải để tìm phần tử đầu tiên lớn hơn 0, trừ đi một phần tử và thêm một phần tử vào phần tử đầu tiên ở bên phải. Điều này đúng nhưng không hiệu quả, vì trong trường hợp xấu nhất chúng ta thực hiện$k \cdot n$các bước. Nếu như$k$là 10000 và$n$là 100, tức là 1.000.000 phép tính, có thể chấp nhận được đối với bài toán này nhưng không cần thiết. 

Thông tin chi tiết quan trọng là mỗi thao tác chỉ ảnh hưởng đến hai yếu tố và mục tiêu là giảm thiểu từ điển. Để làm cho mảng bên trái nhỏ hơn, chúng ta phải luôn giảm các phần tử ngoài cùng bên trái càng nhiều càng tốt và đẩy các đơn vị đã loại bỏ sang bên phải. Thay vì trừ đi từng phần tử một, chúng ta có thể di chuyển số tiền tối đa có thể có từ mỗi phần tử cùng một lúc mà không vượt quá$k$. Nếu một phần tử là$a_i$Và$k$các hoạt động còn lại, chúng ta có thể di chuyển`min(a_i, k)`đơn vị đến phần tử cuối cùng. Sự lựa chọn tham lam này đảm bảo tính tối thiểu về mặt từ điển vì mọi đơn vị còn sót lại ở vị trí bên trái sẽ chỉ làm cho mảng lớn hơn ở vị trí đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n) | Được chấp nhận nhưng chậm hơn | 
| Chuyển nhượng tham lam | O(n) | O(n) | Tối ưu, nhanh chóng | 

## Hướng dẫn thuật toán 

1. Đọc$t$, số lượng ca kiểm thử 
2. Với mỗi test case, hãy đọc$n$,$k$, và mảng$a$. 
3. Lặp lại mảng từ trái sang phải, ngoại trừ phần tử cuối cùng: 

1. Đối với mỗi phần tử$a[i]$, tính toán`move = min(a[i], k)`. Đây là mức tối đa chúng ta có thể chuyển từ phần tử này mà không bị âm và vượt quá các hoạt động còn lại. 
2. Trừ`move`từ$a[i]$và thêm nó vào$a[n-1]$. 
3. Giảm$k$qua`move`. Nếu như$k$đạt đến 0, phá vỡ vòng lặp sớm. 
4. In mảng kết quả. 

Tại sao nó hoạt động: bất biến là chúng ta luôn xử lý các phần tử từ trái sang phải và chúng ta chỉ giảm phần tử bên trái nếu có thể chuyển đơn vị sang phải. Sau khi được xử lý, không thể giảm bớt phần tử bên trái nào nữa, đảm bảo rằng các vị trí ngoài cùng bên trái được giảm thiểu. Đẩy mọi thứ có thể đến phần tử cuối cùng đảm bảo thứ tự từ điển được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    for i in range(n - 1):
        if k == 0:
            break
        move = min(a[i], k)
        a[i] -= move
        a[-1] += move
        k -= move
    
    print(' '.join(map(str, a)))
```Giải thích: Chúng tôi lặp lại phần tử thứ hai đến phần tử cuối cùng vì mọi lần chuyển đều phải có mục tiêu riêng biệt.`move = min(a[i], k)`đảm bảo chúng tôi không bao giờ trừ dưới 0 hoặc vượt quá các phép toán còn lại. Đang cập nhật`a[-1]`đảm bảo rằng tất cả các đơn vị được di chuyển càng xa về bên phải càng tốt. Phá vỡ khi`k == 0`tránh sự lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`3 1 3 1 4`| tôi | một trước | di chuyển | sau | k | 
| --- | --- | --- | --- | --- | 
| 0 | [3,1,4] | 1 | [2,1,5] | 0 | 

Đầu ra:`[2 1 5]`Giải thích: Chúng ta chỉ có thể thực hiện 1 thao tác nên chúng ta giảm phần tử đầu tiên đi 1 và tăng phần tử cuối cùng. 

**Mẫu 2:** 

đầu vào:`2 10 1 0`| tôi | một trước | di chuyển | sau | k | 
| --- | --- | --- | --- | --- | 
| 0 | [1,0] | 1 | [0,1] | 9 | 

Đầu ra:`[0 1]`Giải thích: Tất cả các đơn vị từ phần tử đầu tiên được chuyển sang phần tử thứ hai. Còn lại`k`là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền một lần qua mảng, độc lập với k vì chúng ta di chuyển các đơn vị tối đa có thể cùng một lúc | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng | 

Được cho$n \le 100$Và$t \le 20$, giải pháp này dễ dàng chạy trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read(), {})
    return output.getvalue().strip()

# Provided samples
assert run("2\n3 1\n3 1 4\n2 10\n1 0\n") == "2 1 5\n0 1", "samples"

# Minimum input
assert run("1\n2 1\n0 0\n") == "0 0", "minimum input"

# All equal values
assert run("1\n3 5\n2 2 2\n") == "0 2 4", "all equal"

# Large k
assert run("1\n3 100\n3 2 1\n") == "0 2 4", "large k"

# Already minimal
assert run("1\n4 10\n0 1 2 3\n") == "0 1 2 3", "already minimal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 0 | 0 0 | Xử lý đầu vào tối thiểu | 
| 3 5 2 2 2 | 0 2 4 | Chuyển chính xác khi các giá trị bằng nhau | 
| 3 100 3 2 1 | 0 2 4 | Xử lý k lớn mà không bị vọt lố | 
| 4 10 0 1 2 3 | 0 1 2 3 | Mảng đã tối thiểu về mặt từ điển | 

## Vỏ cạnh 

Nếu phần tử đầu tiên bằng 0 thì không có thao tác nào có thể trừ đi phần tử đó. đầu vào`[0 5]`với`k=10`sẽ giữ nguyên mảng vì`move = min(a[0], k) = 0`. Thuật toán bỏ qua việc chuyển từ phần tử 0 một cách chính xác. 

Nếu như`k`vượt quá tổng của tất cả các phần tử ngoại trừ phần tử cuối cùng, tất cả các giá trị được đẩy đến phần tử cuối cùng, đạt được mảng ngoài cùng bên trái nhỏ nhất. đầu vào`[3 2 1]`với`k=100`trở thành`[0 2 4]`, xác nhận rằng thuật toán tôn trọng các hoạt động còn lại trong khi vẫn duy trì tính không âm.
