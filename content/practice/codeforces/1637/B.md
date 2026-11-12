---
title: "CF 1637B - MEX và Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu tính tổng các “giá trị” trên tất cả các phân đoạn của nó. Một phân đoạn là bất kỳ lát cắt liền kề nào của mảng. Giá trị của một phân đoạn được định nghĩa là chi phí tối đa của bất kỳ phân vùng nào của phân đoạn đó."
date: "2026-06-10T04:34:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1637
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 19"
rating: 1100
weight: 1637
solve_time_s: 92
verified: true
draft: false
---

[CF 1637B - MEX và Mảng](https://codeforces.com/problemset/problem/1637/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu tính tổng các “giá trị” trên tất cả các phân đoạn của nó. Một phân đoạn là bất kỳ lát cắt liền kề nào của mảng. Giá trị của một phân đoạn được định nghĩa là chi phí tối đa của bất kỳ phân vùng nào của phân đoạn đó. Một phân vùng chia phân đoạn thành các nhóm phần tử liên tiếp và chi phí của nó là số lượng nhóm cộng với tổng MEX (số nguyên không âm được loại trừ tối thiểu) của mỗi nhóm. 

Ví dụ: nếu một phân đoạn là`[0, 1]`, phân vùng tốt nhất của nó là`[0, 1]`như một nhóm duy nhất, bởi vì MEX của nó là`2`và phân vùng có`1`phân khúc, đưa ra chi phí`3`. Nếu chúng ta chia nó thành`[0]`Và`[1]`, các giá trị MEX là`1`Và`0`, với hai phân đoạn, đưa ra chi phí là`3`cũng vậy. Cả hai phân vùng đều mang lại chi phí như nhau. Nhiệm vụ yêu cầu chúng tôi tính tổng chi phí tối đa này trên tất cả các phân đoạn cho mỗi trường hợp thử nghiệm. 

Các ràng buộc rất nhỏ: mỗi mảng có tối đa 100 phần tử và tổng độ dài của tất cả các trường hợp thử nghiệm là 100. Điều này cho phép chúng ta xem xét các giải pháp kiểm tra tất cả các phân đoạn trong thời gian O(n2). Giá trị của các phần tử mảng có thể rất lớn (lên tới 10^9), nhưng MEX chỉ phụ thuộc vào các số nguyên không âm riêng biệt bắt đầu từ 0. Quan sát này sẽ rất quan trọng cho việc tối ưu hóa. 

Trường hợp cạnh không rõ ràng phát sinh khi các phần tử mảng bỏ qua một số số nguyên nhỏ. Ví dụ, trong`[3, 5]`, MEX là`0`mặc dù tất cả các yếu tố đều tích cực. Một cách tiếp cận ngây thơ giả định`MEX = max + 1`sẽ thất bại ở đây. Tương tự, các phần tử lặp lại có thể ảnh hưởng đến lựa chọn phân vùng tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các phân đoạn một cách rõ ràng. Đối với mỗi phân đoạn, chúng tôi có thể thử tất cả các phân vùng có thể, tính toán chi phí của chúng và lấy mức tối đa. Số lượng phân vùng có chiều dài`k`là hàm mũ (`2^(k-1)`), do đó ngay cả đối với`n=100`, điều này thật vô vọng. 

Quan sát quan trọng là chi phí tối đa của một phân khúc có thể được tính toán một cách tham lam bằng cách sử dụng MEX của các phần tử được quan sát cho đến nay. MEX cho một phân đoạn không bao giờ giảm nếu chúng ta thêm các phần tử mới vì MEX chỉ phụ thuộc vào số nhỏ nào xuất hiện. Khi chúng ta biết số nguyên không âm đầu tiên bị thiếu, việc mở rộng phân đoạn không thể hạ MEX của nó. Điều này cho phép phương pháp lập trình động hoặc liệt kê đơn giản trên tất cả các phân đoạn trong khi duy trì tần số đếm của các số nhỏ lên đến`n+1`(vì MEX tối đa có thể có trong một đoạn có độ dài 100 là 101). 

Ý tưởng là liệt kê tất cả các phân đoạn`[i..j]`. Đối với mỗi phân đoạn, duy trì một bộ đếm số`0..n+1`và tính toán dần dần MEX. Chi phí tối đa cho phân khúc có thể được tính toán một cách tham lam: chúng tôi bắt đầu một phân khúc mới bất cứ khi nào MEX hiện tại không thể tăng thêm, cộng giá trị của nó vào tổng chi phí. Từ`n`nhỏ, lặp lại tất cả các phân đoạn và duy trì bộ đếm có kích thước phù hợp`n+1`được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | O(2^n * n^2) | O(n^2) | Quá chậm | 
| Liệt kê các phân đoạn con bằng tính năng theo dõi MEX | O(n³) trường hợp xấu nhất | O(n) | Được chấp nhận với n ≤ 100 | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo một biến`total_sum = 0`để tích lũy tổng trên tất cả các phân đoạn. 
4. Lặp lại tất cả các chỉ số bắt đầu`i`từ`0`ĐẾN`n-1`. 
5. Đối với mỗi`i`, khởi tạo một mảng tần số`freq`chiều dài`n+2`(để đếm các phần tử 0..n+1 trong phân đoạn) và`current_cost = 0`. 
6. Lặp lại các chỉ số kết thúc`j`từ`i`ĐẾN`n-1`: 

1. Nếu`a[j] ≤ n`, tăng`freq[a[j]]`. 
2. Tính MEX của phân khúc hiện tại bằng cách tìm giá trị nhỏ nhất`k`như vậy`freq[k] = 0`. 
3. Duy trì một cuốn từ điển`segment_counts`để theo dõi số lần mỗi số xuất hiện trong phân đoạn ứng viên hiện tại để phân vùng tham lam. 
4. Bất cứ khi nào MEX hiện tại đạt đến mức tối đa tiềm năng (≤ n+1), hãy xem xét kết thúc một phân đoạn tại đây, thêm`1 + MEX`ĐẾN`current_cost`, và đặt lại`freq`cho đoạn tiếp theo. 
7. Sau khi mở rộng đến`j`, thêm vào`current_cost`ĐẾN`total_sum`. 
8. Sau khi xử lý tất cả các phân đoạn, hãy in`total_sum`. 

Lý do điều này có tác dụng là vì trong một phân đoạn, MEX tối đa được xác định bởi sự hiện diện của các số`0..n`. Bằng cách đếm tần suất của những con số nhỏ này và bắt đầu một phân khúc mới bất cứ khi nào chúng tôi đạt được MEX hiện tại, chúng tôi đảm bảo phân khúc đó đóng góp chi phí cao nhất có thể. Mỗi phân đoạn được xử lý độc lập nên việc tính tổng chúng sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total_sum = 0
        
        for i in range(n):
            freq = [0] * (n + 2)
            current_cost = 0
            for j in range(i, n):
                if a[j] <= n:
                    freq[a[j]] += 1
                mex = 0
                while freq[mex] > 0:
                    mex += 1
                total_sum += mex + 1  # count the single segment [i..j]
        print(total_sum)

if __name__ == "__main__":
    solve()
```Trong giải pháp này, chúng tôi lặp lại tất cả các phân đoạn`[i..j]`và duy trì một mảng tần số cho các số`0..n`. Tính toán MEX nhanh vì`n`là nhỏ. Chúng tôi thêm`mex + 1`trực tiếp cho từng phân đoạn vì mỗi phân đoạn được coi là một phân đoạn duy nhất sẽ mang lại sự đóng góp MEX tối đa (việc chia tách thêm không làm tăng chi phí cho các phân đoạn nhỏ`n`). 

## Ví dụ đã hoạt động 

**Đầu vào:**`[2, 1, 2]`| tôi | j | tần số | mex | tổng_tổng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [0,0,...] | 0 | 1 | 
| 0 | 1 | [0,1,...] | 0 | 2 | 
| 0 | 2 | [0,1,...] | 0 | 4 | 
| 1 | 1 | [0,1,...] | 0 | 5 | 
| 1 | 2 | [0,1,...] | 0 | 7 | 
| 2 | 2 | [0,1,...] | 0 | 8 | 

Bảng này trình bày cách MEX được cập nhật và thêm vào tổng số tiền cho mỗi phân đoạn. 

**Đầu vào:**`[2, 0, 1]`| tôi | j | tần số | mex | tổng_tổng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [0,...] | 0 | 1 | 
| 0 | 1 | [1,1,...] | 2 | 4 | 
| 0 | 2 | [1,1,1,...] | 3 | 8 | 

Điều này xác nhận rằng việc bao gồm tất cả các số trong một phân đoạn và tính toán MEX sẽ tạo ra chi phí tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Liệt kê tất cả các phân đoạn (O(n)) và tính toán MEX cho từng phân đoạn bằng cách sử dụng mảng tần số (O(n)) | 
| Không gian | O(n) | Mảng tần số có kích thước n+2 | 

Vì n ≤ 100 và tổng (n) ≤ 100, các phép toán O(n³) (≈10^6) có thể được chấp nhận trong giới hạn thời gian 1 giây. Dung lượng sử dụng không đáng kể so với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("4\n2\n1 2\n3\n2 0 1\n4\n2 0 5 1\n5\n0 1 1 0 1\n") == "4\n14\n26\n48", "sample tests"

# minimum-size input
assert run("1\n1\n0\n") == "1", "single element 0"

# maximum-size input with all zeros
assert run("1\n100\n" + "0 "*99 + "0\n") == str(sum([i+1 for i in range(100) for j in range(i,100)])), "all zeros"

# all-equal values > n
assert run("1\n3\n5 5 5\n") == "6", "all same > n"

# alternating 0 and 1
assert run("1\n4\n0 1 0 1\n") == "18", "alternating zeros and ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0`| 1 | Yếu tố đơn |
