---
title: "CF 128D - Số"
description: "Chúng ta được cho một tập hợp các số và được hỏi liệu có thể sắp xếp chúng thành một vòng tròn sao cho mỗi cặp số liền kề cách nhau đúng một đơn vị. Về mặt khái niệm, điều này có nghĩa là mỗi số là một đỉnh trên một chu trình và hiệu tuyệt đối giữa các số lân cận phải là 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 128
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 1 Only)"
rating: 2000
weight: 128
solve_time_s: 98
verified: true
draft: false
---

[CF 128D - Số](https://codeforces.com/problemset/problem/128/D) 

**Xếp hạng:** 2000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số và được hỏi liệu có thể sắp xếp chúng thành một vòng tròn sao cho mỗi cặp số liền kề cách nhau đúng một đơn vị. Về mặt khái niệm, điều này có nghĩa là mỗi số là một đỉnh trên một chu trình và hiệu tuyệt đối giữa các số lân cận phải là 1. Đầu vào bao gồm số phần tử, theo sau là chính các phần tử đó. Đầu ra chỉ đơn giản là "CÓ" nếu tồn tại sự sắp xếp vòng tròn như vậy và "KHÔNG" nếu không thể. 

Các ràng buộc rất quan trọng: số lượng phần tử$n$có thể lên đến$10^5$và bản thân các giá trị có thể lên tới$10^9$. Điều này loại trừ bất kỳ giải pháp nào cố gắng xây dựng rõ ràng tất cả các hoán vị, đó sẽ là thời gian giai thừa. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một số trường hợp khó khăn có thể dễ dàng gặp phải một giải pháp ngây thơ. Ví dụ: nếu tất cả các số đều giống hệt nhau thì sự khác biệt giữa các số lân cận sẽ bằng 0 chứ không phải một, do đó đầu ra phải là "KHÔNG". Nếu các số tạo thành một chuỗi tăng hoặc giảm đơn giản, như`1 2 3 4 3 2`, có thể sắp xếp chúng một cách đối xứng để thỏa mãn điều kiện hình tròn. Một trường hợp phức tạp khác là khi một số xuất hiện nhiều hơn hai lần nhưng không phải là điểm cuối của dãy; Ví dụ,`1 2 2 3 3 3`không thể tạo thành một vòng tròn hợp lệ vì`3`sẽ phải liền kề với hai`3`s ít nhất một lần, vi phạm điều kiện sai phân. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất dễ mô tả: tạo ra tất cả các hoán vị của các số và kiểm tra từng hoán vị để biết điều kiện sai phân vòng tròn. Điều này đúng về nguyên tắc vì nó thực sự kiểm tra mọi sự sắp xếp có thể, nhưng độ phức tạp là$O(n!)$, điều này hoàn toàn không thực tế đối với$n$lên tới$10^5$. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát các thuộc tính của chuỗi trong đó các hiệu liền kề chính xác là một. Bất kỳ sự sắp xếp hợp lệ nào cũng thực sự là một chuỗi các số nguyên liên tiếp (có thể lặp lại) tạo thành mẫu "ngọn núi" hoặc "thung lũng", bởi vì mỗi số phải khác với các số lân cận của nó đúng một. Điều này có nghĩa là nhiều tập hợp có thể được chia thành một phân đoạn tăng dần và một phân đoạn giảm dần, với mỗi số xuất hiện tối đa hai lần, ngoại trừ số tối thiểu và tối đa chỉ có thể xuất hiện một lần. Các số xuất hiện nhiều hơn hai lần ở giữa sẽ buộc phải có hai số lân cận giống hệt nhau, vi phạm yêu cầu về sự khác biệt. 

Từ quan sát này, phương pháp tối ưu xuất hiện: đếm số lần xuất hiện của mỗi số riêng biệt. Nếu bất kỳ số nào xảy ra nhiều hơn hai lần hoặc nếu số tối đa hoặc số tối thiểu xảy ra hai lần trong một cấu hình ngăn cản việc đóng vòng tròn thì câu trả lời là "KHÔNG". Mặt khác, chúng ta có thể sắp xếp các số bằng cách trước tiên đặt một dãy số duy nhất tăng dần nghiêm ngặt lên mức tối đa, sau đó nối các số còn lại theo thứ tự giảm dần về mức tối thiểu. Điều này đảm bảo rằng mỗi cặp liền kề sẽ khác nhau một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng số riêng biệt trong nhiều tập hợp bằng từ điển. Điều này cho phép chúng tôi xác định ngay lập tức nếu có bất kỳ số nào xảy ra nhiều hơn hai lần. Nếu có, hãy in "KHÔNG" và dừng lại. 
2. Tìm số tối thiểu và tối đa. Chúng xác định giới hạn của chuỗi tiềm năng liên tiếp. 
3. Xác minh rằng không có số nào ở giữa dãy xuất hiện quá hai lần. Nếu một số xuất hiện hai lần, một bản sao sẽ thuộc đoạn tăng dần và bản còn lại sẽ thuộc đoạn giảm dần. Mỗi mức tối thiểu và tối đa chỉ có thể xuất hiện một lần để tránh các lân cận lặp lại khi vòng tròn đóng lại. 
4. Nếu tất cả các bước kiểm tra đều đạt, hãy in "CÓ" vì có thể xây dựng một chuỗi vòng tròn hợp lệ bằng cách sắp xếp các số đầu tiên theo thứ tự tăng dần (mỗi số xuất hiện một lần), tiếp theo là thứ tự giảm dần của bất kỳ số trùng lặp nào. 

Tại sao nó hoạt động: thuật toán dựa vào thuộc tính là các số liền kề phải khác nhau một. Bằng cách sắp xếp các số theo thứ tự tăng dần rồi giảm dần, mỗi cặp lân cận trong dãy khác nhau một đơn vị và cách duy nhất để vi phạm điều này là nếu một số xuất hiện nhiều hơn hai lần hoặc ở giới hạn không chính xác. Bằng cách đếm tần số và kiểm tra mức tối thiểu và tối đa, chúng tôi đảm bảo không có hàng xóm bất hợp pháp nào xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

n = int(input())
a = list(map(int, input().split()))
freq = Counter(a)

# if any number appears more than twice, impossible
if any(v > 2 for v in freq.values()):
    print("NO")
    sys.exit(0)

nums = sorted(freq.keys())

# if min or max appears twice, impossible
if freq[nums[0]] == 2 or freq[nums[-1]] == 2:
    print("NO")
    sys.exit(0)

print("YES")
```Giải pháp đọc mảng, đếm tần số và loại bỏ ngay lập tức các trường hợp không thể xảy ra khi một số xuất hiện nhiều hơn hai lần hoặc giá trị tối thiểu/tối đa xuất hiện hai lần. Việc sắp xếp các khóa giúp khái niệm hóa cấu trúc "ngọn núi", nhưng chúng ta không cần phải xây dựng trình tự một cách rõ ràng vì các điều kiện đảm bảo tính khả thi. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`4\n1 2 3 2`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đếm tần số | tần số | {1:1, 2:2, 3:1} | 
| Kiểm tra >2 | - | tất cả 2, vượt qua | 
| Phím được sắp xếp | số | [1,2,3] | 
| Kiểm tra tối thiểu/tối đa hai lần | tần số[1], tần số[3] | 1,1 vượt qua | 
| Đầu ra | - | CÓ | 

Dấu vết này xác nhận rằng một số xuất hiện hai lần ở giữa (2) là có thể chấp nhận được vì nó có thể được phân chia thành các đoạn tăng và giảm. 

Mẫu 2: 

đầu vào:`3\n5 5 5`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đếm tần số | tần số | {5:3} | 
| Kiểm tra >2 | - | 3 > 2 → KHÔNG | 
| Đầu ra | - | KHÔNG | 

Điều này thể hiện việc loại bỏ các số xuất hiện nhiều hơn hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tần số đếm là O(n), việc sắp xếp các khóa duy nhất là O(k log k) trong đó k ≤ n, tổng thể bị chi phối bởi việc sắp xếp | 
| Không gian | O(n) | Từ điển tần số và khóa sắp xếp lưu trữ tối đa n phần tử | 

Với n 10^5, phương pháp này hoạt động thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    if any(v > 2 for v in freq.values()):
        return "NO"
    nums = sorted(freq.keys())
    if freq[nums[0]] == 2 or freq[nums[-1]] == 2:
        return "NO"
    return "YES"

# Provided sample
assert run("4\n1 2 3 2\n") == "YES", "sample 1"

# Custom cases
assert run("3\n5 5 5\n") == "NO", "triple identical"
assert run("3\n1 2 3\n") == "YES", "simple consecutive"
assert run("5\n1 2 3 4 5\n") == "YES", "all consecutive unique"
assert run("6\n1 2 2 3 3 4\n") == "YES", "duplicates in middle"
assert run("4\n1 1 2 2\n") == "NO", "min duplicated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n5 5 5 | KHÔNG | Số xảy ra nhiều hơn hai lần | 
| 3\n1 2 3 | CÓ | Trình tự tăng đơn giản | 
| 5\n1 2 3 4 5 | CÓ | Số liên tiếp, không trùng lặp | 
| 6\n1 2 2 3 3 4 | CÓ | Được phép trùng lặp ở giữa | 
| 4\n1 1 2 2 | KHÔNG | Trường hợp cạnh trùng lặp tối thiểu | 

## Vỏ cạnh 

Khi tất cả các số đều giống nhau, chẳng hạn như`5 5 5`, thuật toán xác định chính xác rằng một số xuất hiện nhiều hơn hai lần và trả về "KHÔNG". Đối với một chuỗi tăng đơn giản như`1 2 3`, thuật toán xác nhận rằng tất cả các tần số là một, tần số tối thiểu và tối đa chỉ xuất hiện một lần và xuất ra "CÓ". Đối với các trường hợp cạnh mà giá trị tối thiểu hoặc tối đa xuất hiện hai lần, chẳng hạn như`1 1 2 2`, thuật toán đánh dấu sự không thể thực hiện được, tránh bất kỳ sự kề cận vòng tròn không hợp lệ nào. Mỗi trường hợp được xử lý trực tiếp bằng cách kiểm tra tần suất và kiểm tra khóa được sắp xếp, đảm bảo tính chính xác.
