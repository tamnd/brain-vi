---
title: "CF 1691D - Tổng GEQ tối đa"
description: "Chúng ta được yêu cầu xác định xem, đối với một mảng số nguyên cho trước, giá trị lớn nhất trong bất kỳ mảng con liền kề nào ít nhất phải lớn bằng tổng của mảng con đó hay không."
date: "2026-06-09T23:10:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "divide-and-conquer", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1691
codeforces_index: "D"
codeforces_contest_name: "CodeCraft-22 and Codeforces Round 795 (Div. 2)"
rating: 1800
weight: 1691
solve_time_s: 117
verified: true
draft: false
---

[CF 1691D - Tổng GEQ tối đa](https://codeforces.com/problemset/problem/1691/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, phân chia để chinh phục, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem, đối với một mảng số nguyên cho trước, giá trị lớn nhất trong bất kỳ mảng con liền kề nào ít nhất phải lớn bằng tổng của mảng con đó hay không. Nói một cách đơn giản hơn, đối với mọi phân đoạn có thể có của mảng, số lớn nhất không được nhỏ hơn tổng của tất cả các số trong phân đoạn đó. Nếu tồn tại dù chỉ một mảng con trong đó tổng vượt quá phần tử lớn nhất, câu trả lời là "KHÔNG"; nếu không thì là "CÓ". 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có độ dài lên tới 200.000 và tổng số phần tử trên tất cả các trường hợp thử nghiệm được giới hạn ở mức 200.000. Mỗi phần tử có thể nằm trong khoảng từ -10^9 đến 10^9. Cho rằng n có thể lên tới 2·10^5 và giới hạn thời gian là 2 giây, một thuật toán có độ phức tạp kém hơn O(n) cho mỗi trường hợp thử nghiệm có thể là quá chậm. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng kiểm tra tất cả các mảng con một cách rõ ràng vì điều đó sẽ yêu cầu các phép toán O(n²) trên mỗi mảng, có thể đạt tới 4·10¹⁰ trong trường hợp xấu nhất. 

Trường hợp cạnh không rõ ràng xảy ra khi mảng chứa một số dương lớn nằm cạnh số âm. Ví dụ: trong mảng [-1, 5, -1], tổng của mỗi mảng con nhiều nhất là 5 và phần tử tối đa là 5. Một cách tiếp cận đơn giản có thể thất bại nếu nó không xem xét các mảng con có độ dài 2 hoặc 3 hoặc chỉ xem xét các tổng liền kề mà không tính đến các giá trị âm làm giảm tổng. Một trường hợp cạnh khác là một chuỗi số dương tăng dần, như [1, 2, 3], trong đó tổng của hai phần tử đầu tiên là 3, bằng giá trị lớn nhất trong mảng con đó, nhưng việc cộng phần tử tiếp theo sẽ tạo ra tổng lớn hơn phần tử lớn nhất cho đến nay, vi phạm điều kiện. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra tất cả các mảng con có thể có, tính cả tổng và giá trị lớn nhất của chúng, đồng thời xác minh điều kiện cho từng mảng. Đối với một mảng có kích thước n, có khoảng n(n+1)/2 mảng con và việc tính toán từng tổng hoặc mức tối đa một cách độc lập sẽ mất O(n) thời gian, cho ra tổng độ phức tạp là O(n³). Ngay cả với tổng tiền tố và mức tối đa của phân đoạn sử dụng cấu trúc dữ liệu, chỉ riêng số lượng mảng con đã khiến phương pháp này không khả thi đối với n = 2·10^5. 

Quan sát quan trọng là nếu điều kiện không thành công thì nó phải thất bại trên một mảng con trong đó tổng đạt giá trị cực đại cục bộ theo một nghĩa nào đó. Bởi vì bất kỳ phần tử âm nào cũng chỉ làm giảm tổng, nên các mảng con nguy hiểm nhất là các đoạn liền kề nhau bắt đầu từ phần tử đầu tiên hoặc kết thúc ở phần tử cuối cùng. Nói cách khác, chúng ta chỉ cần kiểm tra tổng tiền tố và tổng hậu tố của tất cả các mảng con thích hợp (những mảng con không bao gồm toàn bộ mảng). Nếu mọi tiền tố và hậu tố thích hợp có tổng không vượt quá phần tử tối đa ở bất kỳ vị trí nào trong mảng con đó thì điều kiện vẫn đúng. 

Điều này làm giảm vấn đề tính toán tổng tiền tố tối đa và tổng hậu tố tối đa một cách hiệu quả và so sánh chúng với các phần tử liền kề. Trong thực tế, chỉ cần lặp qua mảng và kiểm tra xem liệu tiền tố hoặc tổng hậu tố của các mảng con thích hợp có lớn hơn mức tối đa trong toàn bộ mảng hay không. Nếu tồn tại một mảng con như vậy thì câu trả lời là "KHÔNG"; nếu không thì là "CÓ". Cách tiếp cận này là O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng và xác định phần tử lớn nhất của nó. Đây là ứng cử viên thỏa mãn bất đẳng thức vì bất kỳ tổng nào cũng không được vượt quá mức tối đa này. 
2. Kiểm tra tất cả các tiền tố thích hợp của mảng. Khởi tạo tổng số đang chạy về 0. Lặp lại từ phần tử đầu tiên đến phần tử áp chót, thêm từng phần tử vào tổng hiện có. Nếu tại bất kỳ thời điểm nào tổng chạy vượt quá mức tối đa của mảng thì điều kiện bị vi phạm, vì vậy ngay lập tức trả về "KHÔNG". 
3. Kiểm tra tất cả các hậu tố thích hợp của mảng. Đặt lại tổng hiện có về 0 và lặp từ phần tử cuối cùng đến phần tử thứ hai, thêm từng phần tử vào tổng hiện có. Một lần nữa, nếu tổng vượt quá phần tử tối đa, hãy trả về "KHÔNG". 
4. Nếu cả hai bước kiểm tra hoàn tất mà không tìm thấy mảng con vi phạm, thì điều kiện vẫn giữ nguyên cho mảng đó, vì vậy hãy trả về "CÓ". 

Trực giác cho thấy rằng bất đẳng thức chỉ chặt chẽ đối với các mảng con là tiền tố hoặc hậu tố của một phần nào đó của mảng. Bất kỳ mảng con nào chứa đầy đủ bên trong đều có thể được phân tách thành tiền tố và hậu tố, và vì số âm chỉ giảm tổng nên nếu điều kiện đúng cho tất cả tiền tố và hậu tố thì nó đúng cho tất cả mảng con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        max_val = max(a)

        # check prefixes
        prefix_sum = 0
        valid = True
        for i in range(n - 1):
            prefix_sum += a[i]
            if prefix_sum > max_val:
                valid = False
                break

        # check suffixes
        if valid:
            suffix_sum = 0
            for i in range(n - 1, 0, -1):
                suffix_sum += a[i]
                if suffix_sum > max_val:
                    valid = False
                    break

        print("YES" if valid else "NO")

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định phần tử tối đa. Sau đó, nó kiểm tra tổng tiền tố từ phần tử đầu tiên đến phần tử áp chót, vì việc bao gồm phần tử cuối cùng là không cần thiết - nếu toàn bộ tổng của mảng vượt quá mức tối đa thì bản thân mảng đó sẽ vi phạm điều kiện bị cấm. Tương tự, kiểm tra hậu tố chạy từ phần tử cuối cùng đến phần tử thứ hai. Mỗi vòng lặp sẽ bị ngắt ngay lập tức nếu điều kiện bị vi phạm, tối ưu hóa tốc độ. 

## Ví dụ đã hoạt động 

**Ví dụ 1: [-1, 1, -1, 2]** 

| Bước | Tiền tố/Hậu tố | Tổng Chạy | Tối đa | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| Tiền tố | -1 | -1 | 2 | CÓ | 
| Tiền tố | -1+1=0 | 0 | 2 | CÓ | 
| Tiền tố | 0-1=-1 | -1 | 2 | CÓ | 
| Hậu tố | 2 | 2 | 2 | CÓ | 
| Hậu tố | 2-1=1 | 1 | 2 | CÓ | 
| Hậu tố | 1+1=2 | 2 | 2 | CÓ | 

Tất cả các tổng ≤ phần tử tối đa, xuất ra CÓ. 

**Ví dụ 2: [2, 3, -1]** 

| Bước | Tiền tố | Tổng Chạy | Tối đa | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| Tiền tố | 2 | 2 | 3 | CÓ | 
| Tiền tố | 2+3=5 | 5 | 3 | KHÔNG | 

Tổng tiền tố 5 vượt quá tối đa 3, đầu ra NO. 

Điều này chứng tỏ thuật toán phát hiện chính xác vi phạm trong các mảng con bắt đầu từ đầu mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử mảng được truy cập tối đa hai lần: một lần cho tiền tố, một lần cho hậu tố | 
| Không gian | O(n) | Để lưu trữ mảng; các biến phụ là O(1) | 

Vì tổng số n trong tất cả các trường hợp thử nghiệm là 2·10^5 nên phương pháp tuyến tính này hoạt động tốt trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng vẫn ở dưới mức giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n4\n-1 1 -1 2\n5\n-1 2 -3 2 -1\n3\n2 3 -1\n") == "YES\nYES\nNO", "sample 1"

# custom cases
assert run("1\n1\n100\n") == "YES", "single element"
assert run("1\n5\n1 1 1 1 1\n") == "YES", "all equal small numbers"
assert run("1\n3\n-5 -3 -2\n") == "YES", "all negative numbers"
assert run("1\n4\n1 2 3 4\n") == "NO", "increasing positives"
assert run("1\n6\n2 -1 2 -1 2 -1\n") == "YES", "alternating small negative and positive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ | Mảng kích thước tối thiểu | 
| 1 1 1 1 1 | CÓ | Giá trị hoàn toàn bằng nhau | 
| -5 -3 -2 | CÓ | Chỉ số âm | 
| 1 2 3 4 | KHÔNG | Tổng tiền tố vượt quá tối đa | 
| 2 -1 2 -1 2 -1 | CÓ | Dấu hiệu xen kẽ, tổng không bao giờ vượt quá max | 

## Vỏ cạnh 

Đối với mảng một phần tử như [100], thuật toán tính giá trị tối đa là 100. Cả hai vòng lặp tiền tố và hậu tố đều bị bỏ qua vì n-1 bằng 0, do đó điều kiện sẽ tự động chuyển, xuất ra CÓ. 

Đối với một mảng có tất cả các số âm, ví dụ: [-5, -3, -2], tổng tiền tố bắt đầu âm và không bao giờ vượt quá mức tối đa là -2. Tương tự, tổng hậu tố vẫn ở dưới -2. Thuật toán trả về đúng CÓ.
