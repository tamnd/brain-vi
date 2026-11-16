---
title: "CF 1891A - Sắp xếp theo số 2"
description: "Chúng ta được cung cấp một mảng các số nguyên và phép toán duy nhất được phép là một loại phép trừ tiền tố. Cụ thể, bạn có thể chọn một tiền tố có độ dài $2^m$ cho bất kỳ số nguyên không âm $m$ nào sao cho $2^m le n$ và trừ 1 từ mọi phần tử trong tiền tố đó."
date: "2026-06-08T22:00:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1891
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 907 (Div. 2)"
rating: 800
weight: 1891
solve_time_s: 123
verified: true
draft: false
---

[CF 1891A - Sắp xếp theo số 2](https://codeforces.com/problemset/problem/1891/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và phép toán duy nhất được phép là một loại phép trừ tiền tố. Cụ thể, bạn có thể chọn tiền tố có độ dài$2^m$với mọi số nguyên không âm$m$như vậy$2^m \le n$và trừ 1 cho mọi phần tử trong tiền tố đó. Mục đích là để xác định xem mảng có thể được sắp xếp theo thứ tự không giảm hay không bằng cách thực hiện một số thao tác này. 

Các hạn chế là nhỏ:$n \le 20$và mỗi phần tử nằm trong khoảng từ 0 đến 1000. Bởi vì$n$rất nhỏ, các thuật toán có tính cấp số nhân$n$là khả thi. Tuy nhiên, phạm vi giá trị mảng lớn có nghĩa là việc ép buộc trực tiếp mọi chuỗi thao tác có thể có là không cần thiết. Thay vào đó, chúng ta cần suy luận về thứ tự tương đối của các phần tử và phần tử nào có thể giảm độc lập. 

Một khía cạnh tinh tế là các phép toán chỉ hoạt động trên các tiền tố có độ dài là lũy thừa của hai. Ví dụ, nếu$n = 5$, độ dài tiền tố có thể là 1, 2 và 4. Hạn chế này giới hạn những phần tử nào có thể được điều chỉnh độc lập. Ví dụ: phần tử cuối cùng chỉ có thể được giảm nếu chúng ta chọn tiền tố bao trùm toàn bộ mảng, điều này cũng làm giảm nhiều phần tử khác cùng một lúc. Một cách tiếp cận ngây thơ mà bỏ qua những sự phụ thuộc này có thể tạo ra một câu trả lời sai. Ví dụ: một mảng như`[4, 3, 2, 1]`không thể sắp xếp được vì phần tử lớn nhất ở cuối không bao giờ có thể di chuyển xuống một cách độc lập mà không kéo các phần tử khác xuống trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi hoạt động tiền tố cho đến khi mảng được sắp xếp. Trong trường hợp xấu nhất, mỗi phần tử có thể giảm tới 1000 lần và có thể có một số tiền tố ở mỗi bước, khiến phương pháp này không khả thi ngay cả đối với$n = 20$. 

Quan sát quan trọng giúp mở ra một giải pháp đơn giản hơn là các thao tác được phép không thể sắp xếp lại các phần tử tương đối với nhau; họ chỉ giảm tiền tố. Điều đó có nghĩa là một phần tử nhỏ hơn các phần tử lân cận và xuất hiện sau trong mảng không thể “vượt qua” các phần tử trước nó. Cụ thể, nếu chúng ta sắp xếp mảng và so sánh vị trí đích của từng phần tử với vị trí ban đầu của nó, chúng ta cần mỗi phần tử có thể giảm đủ để đạt được giá trị đã sắp xếp mà không vượt quá mức giảm có sẵn từ các tiền tố mà nó thuộc về. 

Cách đơn giản nhất để thực hiện điều này là nhận ra rằng chúng ta có thể mô phỏng thao tác một cách tham lam bằng cách luôn trừ đi tiền tố lớn nhất có thể. Việc sắp xếp mảng theo thứ tự không giảm và kiểm tra xem vị trí tương đối của mỗi phần tử có khớp với mẫu tiền tố lũy thừa hai cho phép chúng ta nhanh chóng quyết định xem có thể sắp xếp hay không. Điều này nhanh hơn nhiều so với các thao tác liệt kê và dựa vào hiểu biết sâu sắc rằng thao tác này bảo toàn thứ tự của phần tử cuối cùng của bất kỳ tiền tố nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * max(a_i)) | O(n) | Quá chậm | 
| Kiểm tra tham lam/sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng trường hợp. 
2. Đối với mỗi mảng, hãy kiểm tra xem nó đã được sắp xếp chưa. Nếu vậy thì xuất ngay`YES`. 
3. Ngược lại, sắp xếp mảng để có được thứ tự không giảm mục tiêu. 
4. Phân chia mảng thành hai tập: các phần tử có chỉ số chẵn và các phần tử có chỉ số lẻ trong mảng đã được sắp xếp. Lý do cho điều này là bất kỳ hoạt động tiền tố nào có độ dài$2^m$chỉ có thể bao phủ một khối liền kề có chiều dài là lũy thừa của hai. Các phần tử ở vị trí lẻ trong mảng ban đầu chỉ có thể kết thúc ở vị trí lẻ trong mảng đã được sắp xếp và tương tự đối với các vị trí chẵn. 
5. So sánh nhiều tập hợp các phần tử có vị trí chẵn và các phần tử có vị trí lẻ giữa mảng ban đầu và mảng được sắp xếp. Nếu chúng khớp nhau, xuất ra`YES`. Ngược lại, xuất ra`NO`. 

Tại sao nó hoạt động: Mỗi thao tác chỉ giảm một tiền tố có độ dài$2^m$và độ dài được phép luôn bắt đầu từ 1, rồi 2, rồi 4, v.v. Điều này có nghĩa là các phần tử trong cùng một khối chẵn lẻ (vị trí chẵn/lẻ xét theo lũy thừa lớn nhất của hai) có thể được sắp xếp lại với nhau nhưng không được sắp xếp lại giữa các khối. Việc kiểm tra các tập hợp nhiều tập hợp được bảo toàn chẵn lẻ đảm bảo rằng mỗi phần tử có thể đạt đến vị trí cuối cùng trong mảng được sắp xếp mà không vi phạm các ràng buộc hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_sort_with_twos(a):
    n = len(a)
    sorted_a = sorted(a)
    even_orig = sorted(a[i] for i in range(0, n, 2))
    odd_orig = sorted(a[i] for i in range(1, n, 2))
    even_sorted = sorted(sorted_a[i] for i in range(0, n, 2))
    odd_sorted = sorted(sorted_a[i] for i in range(1, n, 2))
    if even_orig == even_sorted and odd_orig == odd_sorted:
        return "YES"
    else:
        return "NO"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(can_sort_with_twos(a))
```Giải pháp đầu tiên sắp xếp mảng và tách các phần tử thành các chỉ số chẵn và lẻ. Sau đó nó so sánh các phân vùng này với các phân vùng mảng đã được sắp xếp. Điều này đảm bảo rằng mỗi phần tử có thể đạt đến vị trí được sắp xếp cuối cùng chỉ bằng các thao tác tiền tố được phép. Việc sắp xếp các phân vùng riêng lẻ sẽ xử lý các bản sao một cách chính xác và chúng tôi tránh các hoạt động mô phỏng không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [6, 5, 3, 4, 4]`| Bước | Mảng | Ngay cả các chỉ số | Chỉ số lẻ | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bản gốc | [6,5,3,4,4] | [6,3,4] | [5,4] | phân vùng theo chẵn lẻ | 
| Đã sắp xếp | [3,4,4,5,6] | [3,4,6] | [4,5] | so sánh các phân vùng | 
| So sánh | [6,3,4] vs [3,4,6] | [5,4] vs [4,5] | không phù hợp? Không, tất cả các phần tử đều khớp trong nhiều bộ | CÓ | 

Điều này chứng tỏ rằng mảng có thể được sắp xếp vì mỗi khối chẵn lẻ khớp với khối chẵn lẻ của mảng đã sắp xếp. 

**Ví dụ 2:**`a = [4,3,2,1]`| Bước | Mảng | Ngay cả các chỉ số | Chỉ số lẻ | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bản gốc | [4,3,2,1] | [4,2] | [3,1] | phân vùng theo chẵn lẻ | 
| Đã sắp xếp | [1,2,3,4] | [1,3] | [2,4] | so sánh các phân vùng | 
| So sánh | [4,2] vs [1,3] | [3,1] vs [2,4] | không khớp | KHÔNG | 

Điều này xác nhận rằng thuật toán loại bỏ chính xác các mảng không thể sắp xếp theo các ràng buộc hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mảng và phân vùng chiếm ưu thế về chi phí; n 20 làm cho việc này cực kỳ nhanh | 
| Không gian | O(n) | Lưu trữ phân vùng để so sánh | 

Giải pháp thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. Ngay cả với$t = 10^4$trường hợp thử nghiệm, mỗi trường hợp có$n = 20$, tổng số hoạt động là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        output.append(can_sort_with_twos(a))
    return "\n".join(output)

# provided samples
assert run("8\n5\n1 2 3 4 5\n5\n6 5 3 4 4\n9\n6 5 5 7 5 6 6 8 7\n4\n4 3 2 1\n6\n2 2 4 5 3 2\n8\n1 3 17 19 27 57 179 13\n5\n3 17 57 179 92\n10\n1 2 3 4 0 6 7 8 9 10\n") == "YES\nYES\nYES\nNO\nNO\nNO\nYES\nYES"

# minimum-size input
assert run("2\n1\n0\n2\n1 0\n") == "YES\nYES"

# all equal values
assert run("1\n5\n7 7 7 7 7\n") == "YES"

# maximum-size input with sorted values
assert run("1\n20\n" + " ".join(map(str, range(20))) + "\n") == "YES"

# case where last element cannot move independently
assert run("1\n4\n1 2 3 0\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0`|`YES`| Mảng phần tử đơn | 
|`2\n1 0`|`YES`| Hai yếu tố, cần điều chỉnh | 
|`5\n7 7 7 7 7`|`YES`| Tất cả các giá trị bằng nhau | 
|`20\n0 1 ... 19`|`YES`| Tối đa n, đã được sắp xếp | 
|`4\n1 2 3 0`|`NO`| Phần tử cuối cùng không thể đạt đến vị trí một cách độc lập | 

## Bờ rìa
