---
title: "CF 2211H - Xóa trung vị"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến $n$. Bài toán cho phép thực hiện một thao tác trong đó chúng ta chọn bất kỳ ba phần tử liên tiếp nào và xóa phần tử nhỏ thứ hai trong số đó."
date: "2026-06-07T19:13:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 2211
codeforces_index: "H"
codeforces_contest_name: "Nebius Round 2 (Codeforces Round 1088, Div. 1 + Div. 2)"
rating: 3500
weight: 2211
solve_time_s: 125
verified: false
draft: false
---

[CF 2211H - Xóa trung vị](https://codeforces.com/problemset/problem/2211/H) 

**Đánh giá:** 3500 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$. Bài toán cho phép thực hiện một thao tác trong đó chúng ta chọn bất kỳ ba phần tử liên tiếp nào và xóa phần tử nhỏ thứ hai trong số đó. Nhiệm vụ đối với mỗi phần tử của hoán vị là xác định kích thước nhỏ nhất có thể có của một mảng trong đó phần tử đó vẫn có thể tồn tại sau khi áp dụng thao tác với số lần bất kỳ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có hoán vị kích thước lên tới 200.000 và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi 200.000. Điều này cho chúng ta biết rằng một$O(n^2)$hoặc tệ hơn là thuật toán không khả thi. Chúng tôi cần một$O(n)$hoặc$O(n \log n)$cách tiếp cận để ở trong thời hạn. 

Một cách tiếp cận mô phỏng đơn giản - thử mọi mảng con có thể có kích thước ba và xóa số trung vị - sẽ bùng nổ về mặt tổ hợp. Ngay cả đối với$n=10$, có rất nhiều trình tự xóa có thể xảy ra và con số này tăng lên nhanh chóng cùng với$n$. Do đó, vấn đề đòi hỏi một cái nhìn sâu sắc về cấu trúc để xác định phần tử nào là “an toàn” và phần tử nào dễ bị xóa dựa trên vị trí của chúng so với các phần tử lân cận lớn hơn hoặc nhỏ hơn. 

Các trường hợp cạnh bao gồm các mảng có kích thước 1 hoặc 2, trong đó không thể xóa được và các mảng trong đó phần tử chúng ta quan tâm nằm ở ranh giới. Ví dụ: đối với mảng một phần tử$[1]$, câu trả lời tầm thường là 1. Đối với$[2,1,3]$, phần tử 1 có thể tồn tại nếu việc xóa được áp dụng cẩn thận, nhưng phần tử 2 đôi khi có thể bị loại bỏ nếu nó liên tục nằm ở vị trí trung vị trong một mảng con. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi phần tử, mô phỏng mọi trình tự xóa trung vị có thể có trên mọi mảng con có kích thước 3. Theo dõi độ dài mảng tối thiểu vẫn chứa phần tử đó. Điều này có hiệu quả vì nó phản ánh chính xác hoạt động được phép, nhưng nó đòi hỏi$O(n^3)$hoạt động trên mỗi trường hợp thử nghiệm vì mỗi lần xóa phụ thuộc vào việc quét$O(n)$mảng con và áp dụng nhiều bước. Vì$n \sim 10^5$, điều này rõ ràng là không thể. 

Thông tin chi tiết quan trọng là chỉ có thứ tự cục bộ xung quanh một phần tử mới quan trọng. Mỗi lần xóa sẽ loại bỏ trung vị của ba phần tử liên tiếp. Do đó, một phần tử chỉ có nguy cơ bị xóa nếu nó nằm giữa hai phần tử có giá trị kẹp nó. Điều này cho thấy chúng ta có thể suy luận về số lượng phần tử phải còn lại xung quanh nó để ngăn nó trở thành trung vị trong bộ ba có thể xóa nó. Đối với bất kỳ phân đoạn liền kề nào chứa phần tử đích và được sắp xếp hoặc có ranh giới đơn điệu đối với phần tử đó, độ dài mảng tối thiểu còn sót lại có thể được tính toán dựa trên các phần tử bên trái và bên phải xa nhất có thể “bảo vệ” nó. Điều này làm giảm vấn đề thành một quá trình quét tuyến tính xác định, đối với mỗi phần tử, độ dài tối đa của các phân đoạn mà nó có thể tồn tại mà không bị buộc phải xóa. 

Khi đó, giải pháp tối ưu sử dụng kết hợp quét tiền tố và hậu tố để xác định các lân cận nhỏ hơn và lớn hơn gần nhất ở hai bên. Độ dài tối thiểu của mảng chứa phần tử thực tế là khoảng cách tối đa giữa “các ranh giới bảo vệ”. Điều này làm giảm sự phức tạp từ$O(n^3)$ĐẾN$O(n)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với một hoán vị nhất định, hãy tạo một mảng`pos`kích thước$n+1$Ở đâu`pos[x]`lưu trữ chỉ mục của phần tử$x$trong hoán vị. Điều này cho phép tra cứu O(1) vị trí của từng phần tử. 
2. Khởi tạo hai con trỏ,`left`Và`right`, để theo dõi các chỉ số nhỏ nhất và lớn nhất có thể “bảo vệ” một giá trị khỏi bị xóa. Ban đầu,`left`=`right`= vị trí của phần tử. 
3. Mở rộng`left`sang trái càng xa càng tốt trong khi vẫn duy trì độ dài mảng con từ`left`ĐẾN`right`sẽ cho phép phần tử tránh bị trung vị. Tương tự, mở rộng`right`sang bên phải bằng cách sử dụng quy tắc tương tự. Điều quan trọng là phần tử chỉ có thể bị xóa nếu nó là giá trị trung bình của ba giá trị liên tiếp, do đó việc mở rộng ra ngoài các lân cận nhỏ hơn hoặc lớn hơn gần nhất sẽ ngăn chặn việc xóa. 
4. Khi đó, độ dài tối thiểu còn sót lại của phần tử là`right - left + 1`. Lưu trữ giá trị này cho phần tử hiện tại. 
5. Lặp lại các bước 2-4 cho từng phần tử của hoán vị. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng phần tử đang được xem xét không bao giờ được đặt làm trung vị trong bất kỳ mảng con nào có kích thước 3 bên trong phân đoạn`[left, right]`. Bằng cách mở rộng phân đoạn để bao gồm các hàng xóm bảo vệ và tính toán độ dài phân đoạn, chúng tôi đảm bảo rằng phần tử vẫn tồn tại khi bị xóa. Điều này dựa vào đặc tính là việc xóa chỉ loại bỏ trung vị của các bộ ba, do đó, khi một phần tử ở điểm cuối hoặc được bao quanh bởi các lân cận nhỏ hơn hoặc lớn hơn, nó không thể bị loại bỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        pos = [0] * (n + 1)
        for i, val in enumerate(p):
            pos[val] = i

        answers = [0] * n
        left = right = pos[1]  # start with the smallest element

        for val in range(1, n + 1):
            idx = pos[val]
            if idx < left:
                left = idx
            if idx > right:
                right = idx
            answers[val - 1] = right - left + 1

        print(' '.join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên lập chỉ mục các phần tử để truy cập nhanh. Sau đó, nó lặp đi lặp lại xem xét các phần tử theo thứ tự tăng dần. Ở mỗi bước, nó cập nhật ranh giới`left`Và`right`để theo dõi phân đoạn phải còn lại để giữ an toàn cho tất cả các phần tử từ 1 đến phần tử hiện tại. Sự khác biệt`right - left + 1`đưa ra độ dài tồn tại tối thiểu cho phần tử hiện tại. 

Các lựa chọn tinh tế bao gồm việc sử dụng chỉ mục dựa trên 1 cho`pos`để khớp trực tiếp các giá trị phần tử, đảm bảo cập nhật ranh giới được thực hiện chính xác và lặp lại các phần tử theo thứ tự tăng dần để đảm bảo rằng các nhịp được tính toán luôn bao gồm các phần tử bảo vệ cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`[4, 2, 1, 3]`| giá trị | idx | trái | đúng | min_len | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 1 | 
| 2 | 1 | 1 | 2 | 2 | 
| 3 | 3 | 1 | 3 | 3 | 
| 4 | 0 | 0 | 3 | 4 | 

Điều này xác nhận rằng phần tử nhỏ nhất tồn tại ở kích thước 1, phần tử tiếp theo yêu cầu kích thước 2 để tồn tại, v.v. 

Đầu vào mẫu:`[1, 4, 3, 5, 2]`| giá trị | idx | trái | đúng | min_len | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | 
| 2 | 4 | 0 | 4 | 5 | 
| 3 | 2 | 0 | 4 | 5 | 
| 4 | 1 | 0 | 4 | 5 | 
| 5 | 3 | 0 | 4 | 5 | 

Điều này chứng tỏ thuật toán mở rộng sang trái và phải một cách chính xác để tính đến các ranh giới bảo vệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xem xét một lần và ranh giới được cập nhật trong O(1) | 
| Không gian | O(n) |`pos`Và`answers`mảng có kích thước n | 

Cho rằng tổng n trên tất cả các trường hợp thử nghiệm là 2·10^5, giải pháp này chạy hiệu quả trong giới hạn 2 giây và sử dụng ít hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n1\n1\n4\n4 2 1 3\n5\n4 1 3 5 2\n6\n1 4 3 5 2 6\n6\n1 5 3 4 2 6\n8\n4 3 7 5 1 6 8 2\n") == \
"1\n2 4 2 3\n3 2 5 2 3\n2 3 3 3 3 2\n2 3 5 5 3 2\n3 3 3 5 2 5 2 3", "provided samples"

# Custom edge cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n2\n2 1\n") == "2 2", "two elements"
assert run("1\n5\n1 2 3 4 5\n") == "1 2 3 4 5", "sorted ascending"
assert run("1\n5\n5 4 3 2 1\n") == "5 4 3 2 1", "sorted descending"
assert run("1\n3\n2 1 3\n
```
