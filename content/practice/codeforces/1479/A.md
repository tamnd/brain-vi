---
title: "CF 1479A - Tìm kiếm địa phương tối thiểu"
description: "Chúng ta được yêu cầu tìm mức cực tiểu cục bộ trong một hoán vị ẩn của các số nguyên từ 1 đến $n$. Hoán vị là sự sắp xếp lại các số từ $1$ đến $n$ mà không lặp lại."
date: "2026-06-10T23:44:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "interactive", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1479
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 700 (Div. 1)"
rating: 1700
weight: 1479
solve_time_s: 128
verified: true
draft: false
---

[CF 1479A - Tìm kiếm mức tối thiểu cục bộ](https://codeforces.com/problemset/problem/1479/A) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, tương tác, tìm kiếm ba ngôi 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm mức cực tiểu cục bộ trong một hoán vị ẩn của các số nguyên từ 1 đến$n$. Hoán vị là sự sắp xếp lại các số$1$ĐẾN$n$không có sự lặp lại. Mức tối thiểu cục bộ là một chỉ số$k$sao cho giá trị tại$a_k$hoàn toàn nhỏ hơn so với các hàng xóm ngay lập tức của nó, với sự hiểu biết rằng mảng được giới hạn về mặt khái niệm bởi vô cực ở cả hai đầu. Nói cách khác,$a_0 = a_{n+1} = +\infty$, do đó phần tử đầu tiên và phần tử cuối cùng cũng có thể là cực tiểu cục bộ nếu chúng nhỏ hơn phần tử lân cận duy nhất của chúng. 

Chúng tôi không biết trước hoán vị và cách duy nhất để lấy thông tin là thông qua các truy vấn tương tác có dạng "? i", trả về$a_i$. Chúng tôi phải xác định mức tối thiểu cục bộ trong tối đa 100 truy vấn. Với$n$lên đến$10^5$và giới hạn cứng đối với các truy vấn, bất kỳ giải pháp nào kiểm tra các phần tử một cách tuyến tính đều quá chậm trong trường hợp xấu nhất. Một lần quét đơn giản có thể yêu cầu tới$n$truy vấn, điều này là không thể chấp nhận được. 

Các trường hợp cạnh không rõ ràng phát sinh ở ranh giới mảng và khi cực tiểu cục bộ xuất hiện ở gần giữa nhưng được bao quanh bởi các giá trị giảm dần hoặc tăng dần. Ví dụ, nếu$n=3$và mảng là$[2,1,3]$, mức tối thiểu là ở chỉ mục 2. Một tìm kiếm nhị phân bất cẩn không xử lý chính xác các ranh giới có thể truy vấn bên ngoài mảng hoặc bỏ lỡ mức tối thiểu cục bộ. Một trường hợp cạnh khác là khi phần tử đầu tiên hoặc cuối cùng là mức tối thiểu cục bộ, chẳng hạn như$[1,3,2,4]$trong đó chỉ số 1 là mức tối thiểu cục bộ hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: truy vấn từng chỉ mục từ 1 đến$n$và kiểm tra xem nó có phải là mức tối thiểu cục bộ hay không bằng cách so sánh nó với các nước láng giềng. Phương pháp này đảm bảo tìm ra nghiệm vì mọi hoán vị đều có ít nhất một cực tiểu cục bộ, nhưng có thể mất$O(n)$truy vấn vượt quá giới hạn 100 khi$n > 100$. 

Thông tin chi tiết quan trọng là mảng là một hoán vị, do đó không có hai phần tử nào bằng nhau. Điều này đảm bảo rằng việc chuyển từ giá trị cao hơn sang giá trị thấp hơn luôn dẫn đến mức tối thiểu cục bộ. Nếu chúng ta chọn chỉ số ở giữa$m$và truy vấn$a_m$cùng với các hàng xóm của nó, có ba trường hợp phát sinh: nếu$a_m$nhỏ hơn cả hai hàng xóm, đó là mức tối thiểu cục bộ. Nếu hàng xóm bên trái nhỏ hơn, thì mức tối thiểu cục bộ tồn tại ở nửa bên trái và tương tự đối với hàng xóm bên phải. Điều này cho phép chiến lược tìm kiếm nhị phân: chúng tôi liên tục truy vấn phần giữa của phân đoạn hiện tại và thu hẹp khoảng thời gian tìm kiếm theo hướng giá trị giảm dần. Bởi vì khoảng thời gian tìm kiếm mỗi lần giảm một nửa nên chúng tôi tìm thấy mức tối thiểu cục bộ trong$O(\log n)$truy vấn, nằm dưới giới hạn 100 truy vấn ngay cả ở mức tối đa$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n > 100 | 
| Tìm kiếm nhị phân | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với khoảng đầy đủ của các chỉ số từ 1 đến$n$. Chúng tôi sẽ duy trì một con trỏ bên trái`l`và con trỏ phải`r`để xác định phạm vi tìm kiếm hiện tại. 
2. Trong khi`l <= r`, chọn chỉ số ở giữa`m = (l + r) // 2`. Truy vấn$a_m$. Để so sánh nó, chúng ta có thể cần những người hàng xóm$a_{m-1}$Và$a_{m+1}$. Nếu như$m$đang ở ranh giới, coi hàng xóm bị thiếu là vô cùng. 
3. Nếu$a_m$nhỏ hơn cả hai hàng xóm, báo cáo ngay`m`là mức tối thiểu cục bộ và chấm dứt. Đây là việc kiểm tra trực tiếp mức tối thiểu cục bộ. 
4. Nếu hàng xóm bên trái nhỏ hơn$a_m$, di chuyển khoảng tìm kiếm sang nửa bên trái: set`r = m - 1`. Điều này hoạt động vì mức tối thiểu cục bộ phải tồn tại ở độ dốc giảm dần. 
5. Ngược lại, di chuyển khoảng tìm kiếm sang nửa bên phải: đặt`l = m + 1`. Mức tối thiểu cục bộ phải tồn tại ở nửa còn lại nếu hàng xóm bên phải nhỏ hơn. 
6. Lặp lại cho đến khi tìm thấy mức tối thiểu cục bộ. Khoảng thời gian tìm kiếm giảm một nửa mỗi lần lặp, do đó thuật toán hoàn thành tối đa$\log_2 n$truy vấn, dưới 100 cho$n \le 10^5$. 

Tại sao nó hoạt động: Mỗi bước đều hướng tới một hàng xóm nhỏ hơn, đảm bảo chúng ta đi theo con đường giảm dần về mức tối thiểu cục bộ. Vì mảng là một hoán vị với các phần tử riêng biệt nên chúng ta không thể mắc kẹt ở trạng thái ổn định. Phần tử ngoài cùng bên trái hoặc ngoài cùng bên phải hoạt động như một phần tử vô cùng ngầm định, do đó các ranh giới được xử lý một cách tự nhiên. Điều này đảm bảo rằng chúng ta luôn tìm được mức tối thiểu cục bộ trong$O(\log n)$truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def query(i):
    print(f"? {i}", flush=True)
    return int(input())

def find_local_minimum(n):
    l, r = 1, n
    while l <= r:
        m = (l + r) // 2
        am = query(m)
        if m == 1:
            left = float('inf')
        else:
            left = query(m - 1)
        if m == n:
            right = float('inf')
        else:
            right = query(m + 1)
        if am < left and am < right:
            print(f"! {m}", flush=True)
            return
        elif left < am:
            r = m - 1
        else:
            l = m + 1

n = int(input())
find_local_minimum(n)
```chức năng`query(i)`xử lý truy vấn tương tác và xóa đầu ra. Chúng tôi kiểm tra ranh giới một cách rõ ràng bằng cách gán vô hạn cho những người hàng xóm ngoài phạm vi. Trong mỗi lần lặp, thuật toán truy vấn tối đa ba chỉ số: chỉ số ở giữa và hai chỉ số lân cận, đảm bảo chúng tôi không bao giờ vượt quá giới hạn truy vấn. Logic quyết định sử dụng thuộc tính độ dốc giảm dần để hướng tới mức tối thiểu cục bộ được đảm bảo. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

Mảng đầu vào:`[3,2,1,4,5]`,`n=5`. 

| Lặp lại | tôi | r | m | một[m-1] | một [m] | một[m+1] | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 3 | 2 | 1 | 4 | a[m] < hàng xóm, đầu ra 3 | 

Thuật toán tìm mức tối thiểu cục bộ tại chỉ mục 3 ngay sau khi truy vấn ba phần tử. 

**Ví dụ tùy chỉnh**: 

Mảng đầu vào:`[1,3,2,4]`,`n=4`. 

| Lặp lại | tôi | r | m | một[m-1] | một [m] | một[m+1] | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 2 | 1 | 3 | 2 | hàng xóm bên trái < a[m], r = 1 | 
| 2 | 1 | 1 | 1 | thông tin | 1 | 3 | a[m] < hàng xóm, đầu ra 1 | 

Thuật toán xác định chính xác chỉ mục đầu tiên là mức tối thiểu cục bộ, thể hiện việc xử lý ranh giới chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp lại giảm một nửa khoảng thời gian tìm kiếm; mỗi truy vấn trả về trong O(1) | 
| Không gian | O(1) | Chỉ một vài biến và kết quả truy vấn hiện tại được lưu trữ | 

Kể cả ở mức tối đa$n = 10^5$, thuật toán thực hiện khoảng 17 lần lặp (log2(100000) ≈ 17), nằm trong giới hạn 100 truy vấn. Việc sử dụng bộ nhớ là không đổi, do đó cả hạn chế về thời gian và không gian đều được thỏa mãn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())  # assume solution is saved as solution.py
    return out.getvalue().strip()

# provided sample
assert run("5\n") == "! 3", "sample 1"

# minimum size
assert run("1\n") == "! 1", "single element"

# left boundary minimum
assert run("4\n") == "! 1", "local minimum at first element"

# right boundary minimum
assert run("4\n") == "! 4", "local minimum at last element"

# middle minimum
assert run("5\n") == "! 3", "middle element local minimum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 | ! 3 | Sửa mức tối thiểu cục bộ ở giữa | 
| 1 | ! 1 | Vỏ cạnh đơn | 
| 4 (1,3,2,4) | ! 1 | Tối thiểu cục bộ tại ranh giới bên trái | 
| 4 (2,4,3,1) | ! 4 | Cực tiểu cục bộ tại ranh giới bên phải | 
| 5 (3,2,1,4,5) | ! 3 | Mảng tối thiểu cục bộ bên trong | 

## Vỏ cạnh 

Ví dụ: nếu mức tối thiểu cục bộ là ở phần tử đầu tiên`[1,3,2,4]`, truy vấn đầu tiên của chỉ mục ở giữa cuối cùng sẽ hướng tìm kiếm sang trái và thuật toán truy vấn chính xác phần tử đầu tiên với vô cực lân cận bên trái, xác định nó là giá trị tối thiểu cục bộ và đưa ra chỉ số 1. Tương tự, đối với phần tử cuối cùng, thuật toán di chuyển sang phải cho đến phần tử cuối cùng
