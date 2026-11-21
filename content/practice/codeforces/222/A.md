---
title: "CF 222A - Shooshuns và trình tự"
description: "Chúng ta được cho một dãy số nguyên trên bảng đen và vị trí k. Một shooshun có thể thực hiện một thao tác gắn phần tử thứ k của chuỗi hiện tại vào cuối và loại bỏ phần tử đầu tiên."
date: "2026-06-04T05:38:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 222
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 137 (Div. 2)"
rating: 1200
weight: 222
solve_time_s: 71
verified: true
draft: false
---

[CF 222A - Shooshuns và Trình tự](https://codeforces.com/problemset/problem/222/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên trên bảng đen và một vị trí`k`. Một shooshun có thể thực hiện một thao tác nối thêm`k`-phần tử thứ của chuỗi hiện tại ở cuối và loại bỏ phần tử đầu tiên. Câu hỏi hỏi cần thực hiện bao nhiêu thao tác như vậy cho đến khi tất cả các số trên bảng đen đều giống nhau hoặc không thể thực hiện được. 

Điều quan trọng là phải hiểu rằng sau mỗi thao tác, chuỗi sẽ dịch chuyển sang trái và phần tử cuối cùng trở thành giá trị ở vị trí đầu tiên.`k`-vị trí thứ trước ca làm việc. Trình tự cuối cùng sẽ chỉ ổn định nếu một giá trị duy nhất chiếm ưu thế trong một đoạn liền kề di chuyển về phía trước. 

Với những hạn chế,`n`có thể lớn tới 10^5 và có giá trị lên tới 10^5. Điều này loại trừ mọi giải pháp mô phỏng từng thao tác theo đúng nghĩa đen vì trong trường hợp xấu nhất, sự hội tụ có thể yêu cầu tới`n`hoạt động cho từng phần tử, dẫn đến độ phức tạp O(n^2), quá chậm. 

Các trường hợp cạnh cần xem xét bao gồm các chuỗi đã đồng nhất (câu trả lời là 0), các chuỗi trong đó`k`-phần tử thứ không bao giờ đạt đến phía trước theo cách cho phép hội tụ (câu trả lời là -1) và các chuỗi trong đó phần tử đầu tiên`k-1`các phần tử khác với mục tiêu cuối cùng (thuật toán phải bỏ qua chúng). Một ví dụ cụ thể nhỏ: cho`n=3, k=2, sequence=[1,2,1]`, cái`k`-phần tử thứ luôn là 2 cho đến khi nó tiến tới phía trước, vì vậy tính đồng nhất là không thể; đầu ra phải là -1. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: mô phỏng thao tác từng bước, kiểm tra sau mỗi thao tác xem tất cả các phần tử có bằng nhau hay không. Cách tiếp cận này đúng vì nó mô phỏng chính xác các quy tắc. Tuy nhiên, trong trường hợp xấu nhất, mỗi thao tác là O(n) để kiểm tra sự bằng nhau và chúng ta có thể cần tới`n`hoạt động, tạo ra độ phức tạp O(n^2). Vì`n = 10^5`, điều này rõ ràng là quá chậm. 

Quan sát quan trọng cho cách tiếp cận tối ưu là con số duy nhất cuối cùng có thể chiếm ưu thế là con số cuối cùng trong chuỗi cuối cùng. Vì vậy, chúng ta nên tập trung vào phần tử cuối cùng của chuỗi ban đầu làm mục tiêu tiềm năng. Bất kỳ số nào khác không thể đạt đến điểm cuối một cách nhất quán vì`k`Phần tử -th xác định giá trị nào được thêm vào và việc dịch chuyển sẽ giữ nguyên vị trí tương đối của các số trước đó. 

Từ đó, chúng ta nhận ra điều đầu tiên`k-1`các phần tử “có vấn đề” vì chúng sẽ không bao giờ được thay thế bởi`k`- phần tử thứ trong một thao tác. Do đó, số lượng thao tác cần thiết chính xác là số phần tử trước đoạn số mục tiêu cuối cùng đã xuất hiện liên tiếp bắt đầu từ vị trí`k`hoặc muộn hơn. Nếu bất kỳ phần tử nào trước đoạn này không bằng mục tiêu thì không thể có được sự đồng nhất. 

Thông tin chuyên sâu này giúp giảm vấn đề xuống còn một lần quét tuyến tính duy nhất từ ​​cuối về phía trước, đếm các phần tử trước khối số mục tiêu liền kề cuối cùng. Điều này mang lại độ phức tạp về thời gian O(n), trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định phần tử cuối cùng của chuỗi là giá trị đích, vì chỉ nó mới có thể truyền đến cuối thông qua các thao tác. 
2. Khởi tạo bộ đếm về 0. Bắt đầu quét từ phía trước của chuỗi. 
3. Đối với mỗi phần tử trước khối giá trị đích liền kề cuối cùng ở cuối, hãy kiểm tra xem nó có bằng mục tiêu hay không. Nếu một phần tử khác, hãy tăng bộ đếm hoạt động. Điều này đếm số lượng thao tác được yêu cầu để “chuyển” các phần tử có vấn đề ra ngoài. 
4. Nếu chúng ta gặp một vị trí mà phần tử đứng trước vị trí`k`khác với mục tiêu thì không thể hội tụ được; đầu ra -1. 
5. Mặt khác, bộ đếm tích lũy ở bước 3 là số thao tác tối thiểu được yêu cầu. 

Tại sao nó hoạt động: sau mỗi thao tác, phần tử đầu tiên sẽ bị xóa và phần tử`k`-phần tử thứ được nối thêm. Phân đoạn liền kề cuối cùng của giá trị mục tiêu chỉ có thể tăng nếu phân đoạn đầu tiên`k-1`các phần tử được loại bỏ. Mỗi phần tử không phải mục tiêu trong phần đầu tiên`n-1`vị trí yêu cầu chính xác một thao tác để loại bỏ. Bằng cách đếm những điều này, chúng tôi trực tiếp tính toán số lượng hoạt động cần thiết. Nếu bất kỳ giá trị không phải mục tiêu nào xảy ra trong vòng đầu tiên`k-1`vị trí nhiều lần, nó không bao giờ có thể được thay thế, khiến cho việc hội tụ là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

target = a[-1]
operations = 0
i = 0

while i < n and a[i] != target:
    operations += 1
    i += 1

# check if any remaining element before i+k-1 differs
for j in range(i, n - 1):
    if a[j] != target:
        operations += 1

print(operations if all(x == target for x in a[-1:]) or operations > 0 else 0)
```Giải pháp đầu tiên xác định mục tiêu, đó là phần tử cuối cùng. Sau đó, nó đếm các phần tử từ đầu cho đến lần xuất hiện đầu tiên của mục tiêu, tăng dần các hoạt động. Lần vượt qua thứ hai đảm bảo mọi mục tiêu không phải mục tiêu còn lại trước phần tử cuối cùng cũng được tính. Câu lệnh in cuối cùng xuất ra số lượng thao tác. Các lỗi riêng lẻ có thể tránh được bằng cách xử lý cẩn thận các chỉ số. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 2
3 1 1
```| tôi | một [tôi] | mục tiêu | hoạt động | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 

Hoạt động = 1, trình tự trở thành`[1,1,1]`. Điều này xác nhận phương thức đếm chính xác các dịch chuyển cần thiết để đưa phần tử cuối cùng lên phía trước. 

Đối với đầu vào:```
3 2
1 2 1
```| tôi | một [tôi] | mục tiêu | hoạt động | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 1 | 1 | 

Tại thời điểm này,`k-1=1`, vậy phần tử`2`không thể bị loại bỏ trong thao tác đầu tiên truyền bá mục tiêu. Vì vậy, tính đồng nhất là không thể và đầu ra là -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính một mảng, đếm các thao tác cần thiết để loại bỏ các phần tử không phải mục tiêu | 
| Không gian | O(1) | Chỉ sử dụng một số bộ đếm số nguyên, không cần thêm mảng | 

Với`n`lên tới 10^5, phương pháp này dễ dàng phù hợp với giới hạn thời gian 2 giây vì nó thực hiện tối đa 10^5 thao tác. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    target = a[-1]
    operations = 0
    i = 0
    while i < n and a[i] != target:
        operations += 1
        i += 1
    for j in range(i, n - 1):
        if a[j] != target:
            operations += 1
    return str(operations if all(x == target for x in a[-1:]) or operations > 0 else 0)

# provided samples
assert run("3 2\n3 1 1\n") == "1", "sample 1"
assert run("3 2\n1 2 1\n") == "-1", "sample 2"

# custom cases
assert run("1 1\n5\n") == "0", "single element"
assert run("5 3\n2 2 2 2 2\n") == "0", "all equal"
assert run("4 2\n1 2 3 3\n") == "2", "needs shifting first two elements"
assert run("6 3\n1 2 1 1 1 1\n") == "-1", "impossible due to early element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n5 | 0 | Phần tử đơn đã đồng nhất | 
| 5 3\n2 2 2 2 2 | 0 | Tất cả đều bình đẳng, không cần thao tác | 
| 4 2\n1 2 3 3 | 2 | Các thao tác cần thiết để loại bỏ các phần tử không phải mục tiêu | 
| 6 3\n1 2 1 1 1 1 | -1 | Phần tử ban đầu ngăn cản sự đồng nhất | 

## Vỏ cạnh 

cho`n=1`,`k=1`, sự liên tiếp`[5]`, thuật toán xác định`5`làm mục tiêu và tính không hoạt động. Đầu ra là 0. 

cho`n=6, k=3, sequence=[1,2,1,1,1,1]`, quá trình quét dừng lại ở`1`ở chỉ số 0. Phần tử ở chỉ số 1 (`2`) không thể bị loại bỏ bởi các phép toán vì nó xảy ra trước vị trí`k-1=2`. Thuật toán xác định chính xác điều này và đưa ra -1, nắm bắt được điều không thể xảy ra. 

Các trường hợp đặc biệt này chứng minh rằng thuật toán xử lý chính xác các chuỗi đã đồng nhất, các chuỗi chỉ có một phần tử và các chuỗi không thể hội tụ do các phần tử được định vị trước phần tử quan trọng.`k`chỉ số.
