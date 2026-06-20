---
title: "CF 1030A - Tìm kiếm một vấn đề dễ dàng"
description: "Chúng ta được giao cho một nhóm nhỏ người, mỗi người đưa ra một quan điểm nhị phân về một vấn đề. Mỗi câu trả lời là 0, nghĩa là người đó coi vấn đề là dễ hoặc 1, nghĩa là người đó cho rằng vấn đề đó là khó."
date: "2026-06-16T20:57:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1030
codeforces_index: "A"
codeforces_contest_name: "Technocup 2019 - Elimination Round 1"
rating: 800
weight: 1030
solve_time_s: 186
verified: true
draft: false
---

[CF 1030A - Tìm kiếm một vấn đề dễ dàng](https://codeforces.com/problemset/problem/1030/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm nhỏ người, mỗi người đưa ra một quan điểm nhị phân về một vấn đề. Mỗi câu trả lời là 0, nghĩa là người đó coi vấn đề là dễ hoặc 1, nghĩa là người đó cho rằng vấn đề đó là khó. Quy tắc của người điều phối rất đơn giản: vấn đề chỉ được giữ nguyên nếu mọi người cho rằng nó dễ dàng. Nếu ngay cả một người gọi nó là khó, vấn đề sẽ bị từ chối. 

Nhiệm vụ là xác định xem có bất kỳ ý kiến ​​“cứng rắn” nào xuất hiện trong số các câu trả lời hay không. Nếu ít nhất một giá trị trong danh sách là 1 thì câu trả lời sẽ là “CỨNG”. Ngược lại, nếu tất cả các giá trị đều bằng 0 thì câu trả lời là “DỄ DÀNG”. 

Ràng buộc trên n nhiều nhất là 100, có nghĩa là bất kỳ lần quét tuyến tính nào cũng nhanh chóng. Ngay cả việc kiểm tra tất cả các tập hợp con hoặc tính toán lại nhiều lượt cũng có thể chấp nhận được về mặt hiệu suất, nhưng không cần thiết. Cấu trúc của bài toán đã gợi ý rằng chỉ cần thực hiện một lần là đủ. 

Không có trường hợp cạnh ẩn phức tạp nào liên quan đến thứ tự hoặc số học. Trường hợp cạnh có ý nghĩa duy nhất là khi n bằng 1, vì quyết định phụ thuộc hoàn toàn vào một giá trị duy nhất. Một trường hợp khác là khi tất cả các giá trị đều bằng 0, giá trị này sẽ trả về chính xác “DỄ DÀNG”. Một lỗi phổ biến là giả định không chính xác sự hiện diện của số 0 hàm ý sự dễ dàng mà không xác minh rằng không có số 1 tồn tại. 

Vấn đề tế nhị thứ hai là định dạng đầu ra. Mọi biến thể viết hoa đều được chấp nhận nhưng điều kiện logic vẫn phải chính xác bất kể lựa chọn định dạng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ chỉ đơn giản là quét qua danh sách và đếm rõ ràng có bao nhiêu người nói “cứng”. Nếu số lượng lớn hơn 0, chúng ta in “HARD”, nếu không thì “EASY”. Điều này đã tối ưu về cấu trúc, nhưng nó có thể được mô tả là việc kiểm tra liên tục từng phần tử và tích lũy kết quả. 

Trong trường hợp xấu nhất, chúng ta kiểm tra tất cả n giá trị, thực hiện công không đổi trên mỗi phần tử. Vì n tối đa là 100 nên các cách tiếp cận phức tạp hơn cũng không thành vấn đề, nhưng quan sát chính là chúng ta không cần xử lý mối quan hệ giữa các phần tử, chỉ phát hiện sự tồn tại của một giá trị duy nhất. 

Cái nhìn sâu sắc là đây là một vấn đề kiểm tra sự tồn tại chứ không phải là một vấn đề tổng hợp. Chúng ta không cần tổng số số 1, chỉ cần có ít nhất một số tồn tại. Điều đó cho phép chấm dứt sớm ngay khi chúng tôi tìm thấy số 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đếm tất cả) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra dừng sớm) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Đọc số nguyên n, cho biết chúng ta sẽ xử lý bao nhiêu ý kiến. Điều này xác định số lần lặp chúng tôi sẽ thực hiện. 
2. Đọc danh sách n số nguyên biểu thị ý kiến. Mỗi giá trị là 0 hoặc 1, vì vậy chúng tôi chỉ quan tâm đến việc phát hiện sự hiện diện của 1. 
3. Khởi tạo một biến cờ chẳng hạn`has_hard = False`. Biến này theo dõi xem chúng ta có thấy ít nhất một quan điểm cứng rắn cho đến nay hay không. 
4. Lặp lại từng giá trị trong danh sách. Đối với mỗi giá trị, hãy kiểm tra xem nó có bằng 1 hay không. 
5. Nếu giá trị bằng 1, hãy đặt`has_hard = True`. Lúc này chúng ta đã biết đáp án cuối cùng sẽ là “CỨNG” nên có thể tùy ý dừng sớm. 
6. Sau khi xử lý tất cả các giá trị (hoặc dừng sớm), hãy kiểm tra cờ. Nếu như`has_hard`là True, xuất ra “HARD”. Nếu không thì xuất ra “EASY”. 

### Tại sao nó hoạt động 

Quyết định chỉ phụ thuộc vào việc có tồn tại ít nhất một phần tử bằng 1 hay không. Cờ duy trì tính bất biến rằng sau khi xử lý bất kỳ tiền tố nào của mảng, nó phản ánh chính xác liệu số 1 có được nhìn thấy trong tiền tố đó hay không. Vì mảng cuối cùng chỉ là tiền tố đầy đủ nên cờ biểu thị chính xác liệu có bất kỳ phản hồi cứng nào tồn tại trong đầu vào hay không. Điều này đảm bảo tính chính xác mà không cần bất kỳ cấu trúc hoặc tính toán bổ sung nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
arr = list(map(int, input().split()))

has_hard = False

for x in arr:
    if x == 1:
        has_hard = True
        break

if has_hard:
    print("HARD")
else:
    print("EASY")
```Giải pháp đọc đầu vào theo thời gian tuyến tính và lưu trữ các phản hồi trong danh sách. Sau đó, nó quét qua danh sách một lần, dừng ngay lập tức khi gặp số 1. Việc ngắt sớm không cần thiết để đảm bảo tính chính xác nhưng phản ánh bản chất kiểm tra sự tồn tại của vấn đề và tránh những lần lặp lại không cần thiết. 

Chi tiết triển khai chính là cờ boolean. Nó tránh việc đếm và mã hóa trực tiếp điều kiện mà chúng ta quan tâm. Một điểm tinh tế khác là xử lý việc phân tách đầu vào một cách chính xác vì tất cả phản hồi đều được đưa ra trên một dòng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 1
```| Bước | Giá trị | has_hard | 
| --- | --- | --- | 
| Bắt đầu | - | Sai | 
| 1 | 0 | Sai | 
| 2 | 0 | Sai | 
| 3 | 1 | Đúng | 

Giá trị thứ ba kích hoạt điều kiện ngay lập tức. Khi tìm thấy số 1 duy nhất, kết quả sẽ trở thành “HARD” và không cần xử lý thêm. 

Đầu ra:```
HARD
```### Ví dụ 2 

đầu vào:```
4
0 0 0 0
```| Bước | Giá trị | has_hard | 
| --- | --- | --- | 
| Bắt đầu | - | Sai | 
| 1 | 0 | Sai | 
| 2 | 0 | Sai | 
| 3 | 0 | Sai | 
| 4 | 0 | Sai | 

Không có giá trị nào thay đổi cờ nên mảng không chứa ý kiến ​​cứng rắn. 

Đầu ra:```
EASY
```Điều này thể hiện trường hợp bổ sung trong đó điều kiện không bao giờ được kích hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được kiểm tra nhiều nhất một lần, với việc chấm dứt sớm tùy chọn | 
| Không gian | O(1) | Chỉ có cờ boolean được sử dụng ngoài bộ nhớ đầu vào | 

Giới hạn giới hạn n ở mức 100, do đó, ngay cả việc quét toàn bộ cũng không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    data = inp.strip().split()
    n = int(data[0])
    arr = list(map(int, data[1:]))

    has_hard = False
    for x in arr:
        if x == 1:
            has_hard = True
            break

    return "HARD" if has_hard else "EASY"

# provided sample
assert run("3\n0 0 1\n") == "HARD"

# all easy
assert run("5\n0 0 0 0 0\n") == "EASY"

# single element hard
assert run("1\n1\n") == "HARD"

# single element easy
assert run("1\n0\n") == "EASY"

# alternating values
assert run("6\n0 1 0 1 0 0\n") == "HARD"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 số không | DỄ DÀNG | trường hợp dễ dàng | 
| 1 | CỨNG | hộp cứng tối thiểu | 
| 0 | DỄ DÀNG | trường hợp dễ dàng tối thiểu | 
| hỗn hợp | CỨNG | phát hiện bất cứ nơi nào trong mảng | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi n bằng 1. Đối với đầu vào`1 0`, vòng lặp xử lý một phần tử duy nhất và không bao giờ đặt cờ, do đó kết quả đầu ra chính xác là “DỄ DÀNG”. Đối với đầu vào`1 1`, cờ được đặt ngay lập tức và đầu ra trở thành “CỨNG”. Điều này xác nhận rằng thuật toán hoạt động chính xác ngay cả khi không có cấu trúc đầu vào “số lượng lớn”. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng 0. Ví dụ:`4 0 0 0 0`giữ cho cờ không thay đổi trong suốt quá trình lặp và quyết định cuối cùng đưa ra một cách chính xác là “DỄ DÀNG”. Điều này cho thấy việc phát hiện sự vắng mặt được xử lý đúng cách. 

Cuối cùng, các trường hợp có nhiều số 1, chẳng hạn như`5 0 1 0 1 0`, kiểm tra xem thuật toán có sai hay không phụ thuộc vào việc đếm. Vì cờ được đặt ở số 1 đầu tiên và không bao giờ đặt lại nên kết quả cuối cùng vẫn là “CỨNG”, không phụ thuộc vào số lượng 1 bổ sung xuất hiện.
