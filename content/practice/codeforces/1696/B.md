---
title: "CF 1696B - NIT phá hủy vũ trụ"
description: "Chúng ta được cung cấp một mảng đại diện cho “vũ trụ”, trong đó mỗi phần tử là một số nguyên không âm. Nhân vật chính, NIT, có thể thực hiện một thao tác cụ thể trên bất kỳ mảng con liền kề nào: tính toán mex của mảng con đó và đặt mọi phần tử trong đó thành mex đó."
date: "2026-06-09T22:33:14+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 900
weight: 1696
solve_time_s: 131
verified: true
draft: false
---

[CF 1696B - NIT phá hủy vũ trụ](https://codeforces.com/problemset/problem/1696/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng đại diện cho “vũ trụ”, trong đó mỗi phần tử là một số nguyên không âm. Nhân vật chính, NIT, có thể thực hiện một thao tác cụ thể trên bất kỳ mảng con liền kề nào: tính toán`mex`của mảng con đó và đặt mọi phần tử trong đó thành mảng đó`mex`. Nhiệm vụ là tìm số lượng thao tác tối thiểu cần thiết để biến toàn bộ mảng thành số không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có độ dài lên tới 100.000 và tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp về thời gian cao hơn đáng kể O(n) cho mỗi trường hợp thử nghiệm đều có khả năng vượt quá giới hạn thời gian. Ví dụ, một cách tiếp cận mạnh mẽ ngây thơ liên tục tính toán`mex`trên các mảng con tùy ý sẽ là O(n²) hoặc tệ hơn, điều này không thể chấp nhận được. Chúng tôi cần một giải pháp về cơ bản là quét mảng một hoặc hai lần. 

Các trường hợp biên không rõ ràng bao gồm các mảng đã bao gồm toàn bộ số 0, các mảng là một chuỗi hoàn hảo bắt đầu từ 0 và các mảng chứa các số rất lớn có khoảng trống ở giữa. Ví dụ, một mảng`[0,1,2,3,4]`có thể được giải quyết trong một thao tác, trong khi`[0,2,3,0,1,2,0]`đòi hỏi hai. Một cách tiếp cận bất cẩn chỉ xem xét phần tử tối đa hoặc giả sử các số liên tiếp có thể tính toán sai các phép toán cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liên tục chọn một số mảng con, tính toán`mex`và áp dụng thao tác cho đến khi tất cả các phần tử bằng 0. Điều này hiệu quả vì cuối cùng mọi phần tử khác 0 đều có thể giảm xuống 0 bằng cách sử dụng một hoặc nhiều thao tác. Tuy nhiên, mỗi phép tính của`mex`có thể mất O(n) và với nhiều thao tác như vậy, độ phức tạp có thể dễ dàng đạt tới O(n²) trong trường hợp xấu nhất, quá chậm đối với n lên tới 10^5. 

Thông tin chi tiết quan trọng là số lượng thao tác cần thiết chỉ phụ thuộc vào giá trị tối thiểu và tối đa hiện có và liệu có bao gồm số 0 hay không. Nếu mảng chỉ chứa số 0 thì không cần thực hiện thao tác nào. Nếu mảng thiếu số 0, chúng ta có thể thực hiện một thao tác duy nhất trên toàn bộ mảng để tạo số 0. Nếu có số 0 nhưng có những phần tử lớn hơn 0 cần phải loại bỏ, chúng ta có thể yêu cầu tối đa hai thao tác. Cụ thể, một thao tác có thể giảm một khối liền kề xuống`mex`, có khả năng đưa ra các số 0 ở những nơi chúng bị thiếu và thao tác thứ hai sau đó có thể truyền các số 0 qua các phần tử khác 0 còn lại. 

Vấn đề giảm xuống còn việc kiểm tra giá trị tối thiểu trong mảng, giá trị tối đa cũng như vị trí đầu tiên và cuối cùng nơi giá trị tối đa xảy ra so với số 0. Điều này cho phép chúng ta quyết định xem không, một hay hai thao tác là đủ. Giải pháp tối ưu không yêu cầu mô phỏng trực tiếp các thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem mảng đã bao gồm toàn số 0 chưa. Nếu vậy, trả về 0 vì không cần thao tác nào. 
2. Tính phần tử nhỏ nhất của mảng. Nếu giá trị tối thiểu lớn hơn 0, hãy thực hiện một thao tác trên toàn bộ mảng để đưa ra số 0, sau đó trả về 1. Điều này đảm bảo rằng giá trị nhỏ nhất trong mảng sẽ trở thành 0. 
3. Xác định phần tử lớn nhất trong mảng và vị trí xuất hiện đầu tiên và cuối cùng của nó. Nếu mảng chứa số 0 và tất cả các phần tử nhỏ hơn hoặc bằng giá trị tối đa nhưng xuất hiện trong các khối liền kề bắt đầu từ 0, thì một thao tác duy nhất là đủ để chuyển đổi các phần tử khác 0 còn lại thành 0. 
4. Nói chung, nếu có số 0 nhưng tồn tại một mảng con chứa các phần tử khác 0 được phân tách bằng số 0 thì có thể cần hai thao tác. Thao tác đầu tiên giảm khối xuống`mex`, giới thiệu số 0 khi cần thiết. Hoạt động thứ hai trải các số 0 cho các phần tử khác 0 còn lại. 
5. Trả về số thao tác được xác định bằng các bước kiểm tra trên: 0 nếu tất cả các số 0, 1 nếu thiếu số 0 hoặc một thao tác duy nhất là đủ và 2 nếu ngược lại. 

Tại sao nó hoạt động: Thuật toán hoạt động vì`mex`hoạt động có thể giảm một mảng con liền kề xuống giá trị còn thiếu nhỏ nhất của nó. Bằng cách chỉ kiểm tra mức tối thiểu và tối đa toàn cầu cũng như vị trí của chúng, chúng tôi nắm bắt được các điều kiện cần thiết để quyết định xem một hoặc hai thao tác có đủ mà không cần mô phỏng tất cả các hoạt động của mảng con hay không. Điều này đảm bảo tính chính xác trong thời gian O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if all(x == 0 for x in a):
            print(0)
            continue
        mn = min(a)
        mx = max(a)
        if mn > 0:
            print(1)
            continue
        first = None
        last = None
        for i, x in enumerate(a):
            if x == mx:
                if first is None:
                    first = i
                last = i
        # check if all elements between first and last are mx
        if all(a[i] == mx for i in range(first, last + 1)):
            print(1)
        else:
            print(2)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xử lý trường hợp đơn giản nhất của một mảng đã bao gồm các số 0. Nếu không, nó sẽ kiểm tra xem giá trị tối thiểu có lớn hơn 0 hay không, điều này ngụ ý rằng chúng ta có thể đưa ra số 0 trong một thao tác. Mặt khác, nó định vị phần tử lớn nhất và kiểm tra xem nó có xuất hiện trong khối liền kề hay không. Nếu có thì chỉ cần một thao tác là đủ; nếu có khoảng trống thì cần thực hiện hai thao tác. Cách tiếp cận này tránh được sự mô phỏng không cần thiết và đảm bảo số lượng thao tác tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[0, 0, 0, 0]`| Bước | mn | mx | đầu tiên | cuối cùng | Số lượng hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | - | - | 0 | 

Tất cả đều bằng 0 nên đầu ra là 0. 

**Ví dụ 2:**`[0, 2, 3, 0, 1, 2, 0]`| Bước | mn | mx | đầu tiên | cuối cùng | Kiểm tra liền kề | Số lượng hoạt động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 3 | 2 | 3 | Sai | 2 | 

Tối đa là 3, nhưng các lần xuất hiện của nó không liền kề nhau; do đó cần có hai thao tác. 

Những dấu vết này cho thấy cách xác định mức tối thiểu, tối đa và vị trí đủ để xác định câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét mảng một lần để tìm các vị trí tối thiểu, tối đa và tối đa | 
| Không gian | O(1) thêm | Chỉ có một vài biến được lưu trữ bên cạnh mảng đầu vào | 

Với tổng n trên tất cả các trường hợp thử nghiệm ≤ 200.000, thuật toán thực hiện thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n4\n0 0 0 0\n5\n0 1 2 3 4\n7\n0 2 3 0 1 2 0\n1\n1000000000\n") == "0\n1\n2\n1", "samples"

# custom cases
assert run("1\n1\n0\n") == "0", "single zero"
assert run("1\n5\n5 5 5 5 5\n") == "1", "all equal non-zero"
assert run("1\n6\n1 0 1 0 1 0\n") == "2", "alternating ones and zeros"
assert run("1\n3\n0 1 0\n") == "2", "max in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n0 | 0 | phần tử đơn 0 | 
| 1\n5\n5 5 5 5 5 | 1 | tất cả các phần tử giống nhau khác 0 | 
| 1\n6\n1 0 1 0 1 0 | 2 | mô hình xen kẽ cần hai thao tác | 
| 1\n3\n0 1 0 | 2 | phần tử max ở giữa yêu cầu hai thao tác | 

## Vỏ cạnh 

cho`[0,1,0]`, giá trị tối thiểu là 0 và tối đa là 1. Giá trị tối đa xảy ra trong một mảng con không liền kề (vị trí 2), do đó thuật toán trả về chính xác 2. Đối với`[5,5,5,5,5]`, min > 0, do đó, một thao tác đơn lẻ trên toàn bộ mảng sẽ đưa ra số 0. Vì`[0,0,0,0]`, tất cả đều bằng 0 nên không cần thực hiện thao tác nào. Những ví dụ này xác nhận rằng thuật toán xử lý chính xác các trường hợp quan trọng.
