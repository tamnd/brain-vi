---
title: "CF 102163C - Hasan và những học trò lười biếng"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi có một mảng số nguyên (N). Dãy con giữ nguyên thứ tự từ trái sang phải ban đầu nhưng có thể bỏ qua bất kỳ số phần tử nào. Nó chỉ tăng khi mọi giá trị được chọn đều lớn hơn giá trị được chọn trước đó. Nhiệm vụ có hai phần."
date: "2026-08-19T23:52:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "C"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 664
verified: false
draft: false
---

[CF 102163C - Hasan và những học trò lười biếng](https://codeforces.com/problemset/problem/102163/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi có một mảng số nguyên (N). Dãy con giữ nguyên thứ tự từ trái sang phải ban đầu nhưng có thể bỏ qua bất kỳ số phần tử nào. Nó chỉ tăng khi mọi giá trị được chọn đều lớn hơn giá trị được chọn trước đó. 

Nhiệm vụ có hai phần. Chúng ta cần độ dài tối đa có thể có của một dãy con tăng dần và chúng ta cũng cần đếm xem có bao nhiêu dãy con khác nhau đạt được độ dài tối đa đó. Hai dãy con khác nhau khi chúng chọn các vị trí khác nhau trong mảng, ngay cả khi các giá trị được chọn bằng nhau. Số đếm được in theo modulo (10^9+7). 

Ví dụ, trong```
1 2 1 4 3
```chiều dài tối đa là (3). Các dãy con sử dụng vị trí (1,2,4) và (1,2,5) cho ra (1,2,4) và (1,2,3) nên có hai LIS. 

Độ dài mảng tối đa là (1000), điều này làm cho giải pháp lập trình động (O(N^2)) trở nên thiết thực. Một giải pháp (O(N^3)) sẽ thực hiện khoảng (10^9) thao tác khi (N=1000), quá nhiều so với giới hạn 4 giây. Việc liệt kê các chuỗi con tệ hơn theo cấp số nhân, vì một mảng có độ dài (N) có (2^N) các tập hợp con vị trí có thể có. 

Các giá trị (A_i) có thể lớn bằng (10^6), nhưng điều này không yêu cầu nén tọa độ hoặc cấu trúc dữ liệu đặc biệt vì quá trình chuyển đổi lập trình động chỉ so sánh hai giá trị. Sự khác biệt quan trọng là sự bất đẳng thức nghiêm ngặt, do đó các giá trị bằng nhau không bao giờ được mở rộng một dãy con tăng dần. 

Một số trường hợp nhỏ bộc lộ những lỗi phổ biến. Với```
1
1
7
```câu trả lời là```
1 1
```bởi vì bản thân phần tử đơn lẻ là LIS duy nhất. Việc triển khai khởi tạo số cách về 0 sẽ làm mất trường hợp cơ bản này. 

Với```
1
3
5 5 5
```câu trả lời là```
1 3
```bởi vì mỗi vị trí độc lập tạo thành một LIS có độ dài một. Việc coi phép so sánh là (A_j \le A_i) sẽ tạo ra một chuỗi con dài hơn một cách không chính xác. 

Với```
1
3
1 2 3
```câu trả lời là```
3 1
```vì có đúng một cách để chọn cả ba vị trí. Việc triển khai bất cẩn tính tổng số lượng từ mọi phiên bản trước mà không kiểm tra xem độ dài kết quả có phải là độ dài tốt nhất cho vị trí hiện tại hay không có thể bị tính quá mức. 

Cuối cùng, số lượng có thể trở nên rất lớn. Ví dụ: một mảng chứa nhiều cấu trúc lặp lại có thể có số lượng LIS lớn, do đó số lượng phải giảm theo modulo (10^9+7) trong quá trình chuyển đổi lập trình động. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê mọi dãy con của mảng, kiểm tra xem nó có tăng đúng hay không và giữ lại dãy dài nhất. Có chính xác (2^N) tập hợp con vị trí. Nếu chúng tôi kiểm tra tối đa (N) vị trí cho mỗi tập hợp con thì kết quả trong trường hợp xấu nhất là (O(N2^N)). Với (N=1000), điều này hoàn toàn không khả thi. 

Chúng ta có thể làm tốt hơn bằng cách đặt một câu hỏi mang tính địa phương hơn. Thay vì xây dựng mọi dãy con có thể, hãy xem xét một LIS có vị trí được chọn cuối cùng là (i). Mọi thứ trước (i) bản thân nó phải là một dãy con tăng dần kết thúc ở vị trí nào đó (j<i), với (A_j<A_i). Điều này có nghĩa là mọi dãy con hợp lệ kết thúc tại (i) có thể được xây dựng bằng cách mở rộng một dãy con hợp lệ kết thúc ở vị trí trước đó. 

Đối với mọi vị trí (i), duy trì hai giá trị. Đầu tiên là độ dài của dãy con tăng dài nhất kết thúc chính xác tại (i). Thứ hai là số dãy con dài nhất như vậy. 

Giả sử (j<i) và (A_j<A_i). Nếu dãy con tốt nhất kết thúc tại (j) có độ dài (L), việc thêm (A_i) sẽ tạo ra một dãy con có độ dài (L+1). Nếu kết quả này tốt hơn bất kỳ kết quả nào được tìm thấy trước đó cho (i), chúng ta sẽ thay thế độ dài và sao chép số cách từ (j). Nếu nó liên quan đến độ dài tốt nhất hiện tại, chúng ta sẽ cộng số cách từ (j). 

Giải pháp brute-force hoạt động vì mọi chuỗi con đều được xem xét rõ ràng, nhưng nó thất bại vì có nhiều chuỗi con theo cấp số nhân. Quan sát rằng mọi LIS kết thúc tại (i) có một vị trí được chọn trước đó duy nhất cho phép chúng ta tóm tắt tất cả các khả năng trước đó với hai giá trị cho mỗi vị trí. Vì (N) chỉ là (1000), nên việc kiểm tra từng cặp (j<i) sẽ đưa ra chương trình động (O(N^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N2^N)) | (O(N)) | Quá chậm | 
| DP tối ưu | (O(N^2)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý mảng từ trái sang phải. Đối với mọi vị trí (i), khởi tạo`length[i] = 1`Và`count[i] = 1`. Một phần tử duy nhất luôn tạo thành một dãy con tăng dần có độ dài bằng một, vì vậy đây là các giá trị cơ sở chính xác. 
2. Đối với mỗi vị trí (i), hãy kiểm tra mọi vị trí trước đó (j<i). Chỉ các vị trí thỏa mãn (A_j<A_i) mới có thể đứng trước (i), vì dãy con phải tăng nghiêm ngặt. 
3. Với mọi phần trước hợp lệ (j), hãy tính độ dài thu được bằng cách mở rộng chuỗi con tốt nhất của nó: 

[ 
ứng viên = chiều dài[j] + 1. 
] 

Nếu`candidate`lớn hơn`length[i]`, thay thế`length[i]`với nó và thiết lập`count[i]`ĐẾN`count[j]`. Tất cả các dãy con ngắn hơn đã biết trước đây kết thúc tại (i) đều có thể bị loại bỏ vì chúng không bao giờ có thể trở nên dài nhất trên toàn cầu thông qua vị trí này. 

1. Nếu`candidate`bằng`length[i]`, thêm vào`count[j]`ĐẾN`count[i]`. Chúng đại diện cho một nhóm mới gồm các chuỗi con riêng biệt có cùng độ dài tối ưu kết thúc tại (i). Lấy tổng modulo (10^9+7). 
2. Sau khi xử lý từng vị trí, tìm giá trị lớn nhất trong`length`. Đây là độ dài LIS toàn cầu. 
3. Tổng`count[i]`trên mọi vị trí mà`length[i]`bằng mức tối đa toàn cầu. Mỗi LIS có chính xác một vị trí cuối cùng, vì vậy giá trị này tính tổng mỗi LIS chính xác một lần. Một lần nữa, lấy kết quả theo modulo (10^9+7). 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý vị trí (i),`length[i]`chính xác là độ dài tối đa của dãy con tăng dần kết thúc tại (i), trong khi`count[i]`chính xác là số dãy con đạt được độ dài đó. 

Bất kỳ dãy con tăng nào kết thúc tại (i) hoặc chỉ bao gồm (A_i), hoặc có một số chuỗi đứng trước cuối cùng (j<i) với (A_j<A_i). Trong trường hợp thứ hai, tiền tố của nó phải là dãy con tăng dần tận cùng tại (j). Quá trình chuyển đổi xem xét mọi khả năng có thể xảy ra trước đó, do đó, nó xem xét mọi cách có thể mà một chuỗi con tối ưu có thể đạt tới (i). Khi tìm thấy độ dài dài hơn, chỉ họ dài hơn đó mới có thể duy trì mức tối ưu. Khi tìm thấy độ dài bằng nhau, tất cả các chuỗi con riêng biệt của nó phải được thêm vào. 

Cuối cùng, mỗi LIS có một và chỉ một vị trí cuối cùng. Tổng hợp số lượng cho các vị trí có mức tối đa`length`do đó đếm mọi LIS chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        length = [1] * n
        count = [1] * n

        for i in range(n):
            for j in range(i):
                if a[j] >= a[i]:
                    continue

                candidate = length[j] + 1

                if candidate > length[i]:
                    length[i] = candidate
                    count[i] = count[j]
                elif candidate == length[i]:
                    count[i] += count[j]
                    count[i] %= MOD

        best_length = max(length)

        total_count = 0
        for i in range(n):
            if length[i] == best_length:
                total_count += count[i]
                total_count %= MOD

        print(best_length, total_count)

if __name__ == "__main__":
    main()
```Hai mảng là cốt lõi của trạng thái lập trình động.`length[i]`mô tả dãy con tốt nhất kết thúc ở vị trí`i`, Và`count[i]`mô tả có bao nhiêu cách đạt được độ dài chính xác đó. 

Vòng lặp bên trong kiểm tra mọi vị trí trước đó. điều kiện`a[j] >= a[i]`bác bỏ các giá trị bằng nhau cũng như giảm các chuyển đổi, để lại chính xác bất đẳng thức nghiêm ngặt mà bài toán yêu cầu. 

Khi một người tiền nhiệm thực sự tốt hơn được tìm thấy,`count[i]`phải được thay thế chứ không phải thêm vào. Dãy con ngắn hơn kết thúc tại (i) không thể đóng góp vào LIS kết thúc tại (i). Khi ứng cử viên có độ dài tốt nhất hiện tại, số lượng sẽ được cộng thêm vì cả hai lựa chọn trước đó đều tạo ra các chuỗi vị trí riêng biệt. 

Tất cả số lượng đều giảm theo modulo (10^9+7). Số nguyên Python không bị tràn nhưng việc lấy mô-đun sẽ giữ cho các giá trị ở mức nhỏ và trực tiếp thực hiện quy ước đầu ra được yêu cầu. 

Vòng lặp cuối cùng là cần thiết vì LIS có thể kết thúc ở bất kỳ vị trí nào. Chỉ lấy`count[n - 1]`chẳng hạn, sẽ chỉ đúng khi phần tử mảng cuối cùng thuộc về mọi LIS. 

Các chỉ số trong quá trình chuyển đổi thỏa mãn`j < i`, do đó thứ tự ban đầu của mảng sẽ tự động được giữ nguyên. Không có sự sắp xếp nào được thực hiện vì việc sắp xếp sẽ phá hủy ràng buộc về thứ tự dãy con. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm đầu tiên từ mẫu:```
1 3 2 3 1
```Trạng thái sau mỗi vị trí là: 

| Chức vụ (i) | (A_i) |`length[i]`|`count[i]`| Lý do | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | Yếu tố đơn | 
| 1 | 3 | 2 | 1 | Mở rộng`1`| 
| 2 | 2 | 2 | 1 | Mở rộng`1`| 
| 3 | 3 | 3 | 1 | Mở rộng dãy con tốt nhất kết thúc tại`2`| 
| 4 | 1 | 1 | 1 | Không thể mở rộng bất kỳ giá trị nào trước đó | 

Độ dài tối đa là (3), chỉ đạt ở vị trí (3). Vì thế câu trả lời là`3 1`. 

Dấu vết này cho thấy tại sao các giá trị bằng nhau không thể được sử dụng làm giá trị trước đó. thứ hai`3`không thể gia hạn lần đầu tiên`3`, nhưng nó có thể mở rộng chuỗi con kết thúc bằng`2`. 

Bây giờ hãy xem xét trường hợp thử nghiệm mẫu thứ ba:```
1 5 6 2 1 4 1
```Các trạng thái kết quả là: 

| Chức vụ (i) | (A_i) |`length[i]`|`count[i]`| 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 5 | 2 | 1 | 
| 2 | 6 | 3 | 1 | 
| 3 | 2 | 2 | 1 | 
| 4 | 1 | 1 | 1 | 
| 5 | 4 | 3 | 2 | 
| 6 | 1 | 1 | 1 | 

Độ dài tối đa là (3). Nó xảy ra hai lần, một lần cho đến hết`1, 5, 6`và một lần xuyên qua`1, 2, 4`. Số đếm ở vị trí (2) và (5) đều bằng một nên số đếm cuối cùng là (2). 

Trạng thái tại vị trí (5) thể hiện quá trình chuyển đổi đếm. Cả hai`1, 5, 4`không hợp lệ vì (5>4), nhưng`1, 2, 4`và cấu trúc tiền thân hợp lệ khác được xem xét độc lập. DP chỉ giữ lại những phần trước thỏa mãn sự so sánh nghiêm ngặt và sau đó tổng hợp các độ dài tối ưu bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Mỗi vị trí kiểm tra mọi vị trí trước đó một lần | 
| Không gian | (O(N)) | Hai mảng DP lưu trữ một trạng thái cho mỗi vị trí mảng | 

Với (N\le1000), mỗi trường hợp kiểm thử thực hiện tối đa khoảng (500{,}000) so sánh trước đó. Điều này nằm trong mức độ phức tạp dự định cho giới hạn 4 giây, trong khi mức sử dụng bộ nhớ (O(N)) thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def main():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        length = [1] * n
        count = [1] * n

        for i in range(n):
            for j in range(i):
                if a[j] >= a[i]:
                    continue

                candidate = length[j] + 1

                if candidate > length[i]:
                    length[i] = candidate
                    count[i] = count[j]
                elif candidate == length[i]:
                    count[i] = (count[i] + count[j]) % MOD

        best_length = max(length)
        total_count = sum(
            count[i] for i in range(n)
            if length[i] == best_length
        ) % MOD

        print(best_length, total_count)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        main()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run(
    """3
5
1 3 2 3 1
3
1 2 3
7
1 5 6 2 1 4 1
"""
) == """3 1
3 1
3 2
""", "provided sample"

assert run(
    """1
1
7
"""
) == """1 1
""", "minimum-size input"

assert run(
    """1
5
4 4 4 4 4
"""
) == """1 5
""", "all equal values"

assert run(
    """1
6
1 2 3 4 5 6
"""
) == """6 1
""", "strictly increasing array"

assert run(
    """1
7
1 2 1 2 1 2 3
"""
) == """3 6
""", "multiple LIS choices"

assert run(
    """1
4
1000000 999999 1000000 999998
"""
) == """2 1
""", "value boundary and strict comparison"

n = 1000
increasing = " ".join(str(i) for i in range(1, n + 1))
assert run(f"""1
{n}
{increasing}
""") == """1000 1
""", "maximum-size increasing input"

equal_values = " ".join(["42"] * n)
assert run(f"""1
{n}
{equal_values}
""") == """1 1000
""", "maximum-size all-equal input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1 1`| Vỏ đế một phần tử | 
|`4 4 4 4 4`|`1 5`| Bất bình đẳng nghiêm ngặt và tính các vị trí khác nhau | 
|`1 2 3 4 5 6`|`6 1`| LIS duy nhất sử dụng toàn bộ mảng | 
|`1 2 1 2 1 2 3`|`3 6`| Nhiều dãy con tối ưu | 
|`1000000 999999 1000000 999998`|`2 1`| Giá trị lớn và so sánh chặt chẽ | 
| 1000 giá trị tăng dần |`1000 1`| Tối đa (N) và DP bậc hai | 
| 1000 giá trị bằng nhau |`1 1000`| Tối đa (N), giá trị trùng lặp và đếm | 

## Vỏ cạnh 

Đối với một yếu tố duy nhất, hãy xem xét```
1
1
7
```DP bắt đầu với`length[0] = 1`Và`count[0] = 1`. Không có vị trí nào sớm hơn để kiểm tra nên các giá trị này không thay đổi. Độ dài tối đa toàn cầu là (1) và số lượng của nó là (1), cho`1 1`. Đây là lý do tại sao mọi trạng thái DP phải bắt đầu bằng 1 thay vì 0. 

Để có giá trị bằng nhau, hãy xem xét```
1
3
5 5 5
```Mỗi vị trí bắt đầu bằng một chuỗi con có độ dài một. Với mỗi cặp (j<i), điều kiện`a[j] >= a[i]`là đúng vì các giá trị bằng nhau nên không có quá trình chuyển đổi nào được thực hiện. Cả ba vị trí vẫn là trạng thái`(1, 1)`. Độ dài LIS toàn cầu là (1) và phép tổng hợp cuối cùng cộng cả ba số đếm, tạo ra`1 3`. Điều này mắc phải lỗi phổ biến khi sử dụng`<=`thay vì`<`. 

Đối với một mảng tăng nghiêm ngặt,```
1
3
1 2 3
```vị trí (0) có`(1,1)`. Vị trí (1) mở rộng nó tới`(2,1)`và vị trí (2) mở rộng chuỗi con kết thúc ở vị trí (1) thành`(3,1)`. LIS duy nhất chọn cả ba vị trí, vì vậy câu trả lời là`3 1`. Điều này cũng chứng minh tại sao câu trả lời cuối cùng không thể đơn giản sử dụng trạng thái của phần tử cuối cùng trong triển khai chung, mặc dù nó hoạt động ở đây. 

Đối với nhiều LIS, hãy xem xét```
1
7
1 2 1 2 1 2 3
```trận chung kết`3`có thể được bắt đầu bởi bất kỳ dãy con có độ dài hai nào bao gồm một`1`theo sau là sau`2`. Có sáu lựa chọn vị trí như vậy, do đó trạng thái cuối cùng cho`3`có chiều dài (3) và số đếm (6). Thuật toán tìm thấy những điều này thông qua các chuyển tiếp và đầu ra có độ dài bằng nhau lặp đi lặp lại`3 6`. 

Đối với các giá trị ở ranh giới trên, hãy xem xét```
1
4
1000000 999999 1000000 999998
```đầu tiên`1000000`không thể được theo sau bởi bất cứ điều gì lớn hơn, trong khi`999999`có thể được theo sau bởi cái sau`1000000`. Do đó, dãy con tốt nhất là`999999, 1000000`, với độ dài (2) và một lựa chọn. Kết quả là`2 1`. Độ lớn thực tế của các giá trị không ảnh hưởng đến DP, vì chỉ cần so sánh theo cặp. 

Để có kích thước mảng tối đa, mảng (1000) phần tử tăng dần sẽ tạo ra chính xác (1000) trạng thái DP và kiểm tra cặp khoảng (1000\cdot999/2). Độ dài LIS trở thành (1000), với số đếm (1). Một mảng hoàn toàn bằng nhau gồm (1000) phần tử thực hiện cùng số lượng so sánh nhưng không thực hiện chuyển đổi nào, để lại mọi trạng thái ở`(1,1)`. Sự tổng hợp cuối cùng tạo ra`1 1000`. Những trường hợp này chứng minh rằng việc triển khai (O(N^2)) xử lý toàn bộ giới hạn đầu vào mà không dựa vào cấu trúc mảng thuận lợi.
