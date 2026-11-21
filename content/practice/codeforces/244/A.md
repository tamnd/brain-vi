---
title: "CF 244A - Cam phân chia"
description: "Chúng ta được yêu cầu chia một quả cam gồm nk phân đoạn cho k trẻ sao cho mỗi trẻ nhận được đúng n phân đoạn, mỗi trẻ chắc chắn nhận được phân đoạn mình đã chọn và không có phân đoạn nào được trao cho nhiều hơn một trẻ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 244
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 150 (Div. 2)"
rating: 900
weight: 244
solve_time_s: 193
verified: false
draft: false
---

[CF 244A - Màu cam phân chia](https://codeforces.com/problemset/problem/244/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chia một quả cam gồm`n*k`phân khúc giữa`k`trẻ em để mỗi đứa trẻ nhận được chính xác`n`phân đoạn, mỗi đứa trẻ chắc chắn nhận được phân đoạn mà chúng đã chọn và không có phân đoạn nào được trao cho nhiều hơn một đứa trẻ. Đầu vào cung cấp tổng số phân đoạn cho mỗi đứa trẻ`n`, số con`k`, và một danh sách`k`số phân đoạn riêng biệt, mỗi số tương ứng với phân đoạn mà trẻ muốn. 

Đầu ra là một danh sách`n*k`số nguyên được phân chia thành`k`các nhóm liên tiếp`n`số, đại diện cho các phân đoạn được gán cho mỗi đứa trẻ. Thứ tự trong nhóm của mỗi đứa trẻ không quan trọng. 

Những hạn chế`1 ≤ n, k ≤ 30`có nghĩa là tổng số phân đoạn nhiều nhất`900`, do đó, bất kỳ thuật toán nào lặp lại các phân đoạn hoặc phân đoạn con một vài lần đều khả thi. Giới hạn thấp cũng ngụ ý rằng chúng ta không cần phải tối ưu hóa nhiều về thời gian, vì ngay cả một cách tiếp cận tham lam đơn giản là chọn từng phân đoạn không được sử dụng cũng sẽ hoạt động thoải mái. 

Trường hợp cạnh xảy ra khi trẻ chọn các phân đoạn ở cuối cùng của tổng số phân đoạn, chẳng hạn như`1`hoặc`n*k`. Nếu người ta triển khai giải pháp bằng cách điền tuần tự các phân đoạn của trẻ em mà không kiểm tra phân đoạn nào còn trống, thì rất dễ vô tình gán cùng một phân đoạn cho nhiều trẻ em hoặc điền sai hạn ngạch của một trẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị của các phân đoạn và sau đó kiểm tra từng phép gán để xem liệu nó có thỏa mãn các ràng buộc hay không. Điều này đúng nhưng không thực tế vì số hoán vị của`n*k`các phân khúc tăng trưởng theo giai đoạn. Ngay cả đối với`n=k=10`, có`100!`hoán vị, điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các hoán vị. Mỗi đứa trẻ đã “đặt trước” một phân đoạn. Chúng ta có thể lấp đầy phần còn lại`n-1`phân đoạn cho mỗi phân đoạn con từ tập hợp các phân đoạn chưa được chỉ định. Vì bài toán đảm bảo rằng câu trả lời tồn tại và các phân đoạn có thể được chỉ định theo bất kỳ thứ tự nào, nên một cách tiếp cận tham lam đơn giản sẽ có hiệu quả: lặp lại tất cả các phân đoạn theo thứ tự tăng dần và gán từng phân đoạn chưa được chỉ định cho phân đoạn con hiện tại cho đến khi đạt được hạn ngạch của chúng. Điều này tránh xung đột vì chúng tôi duy trì bản ghi các phân đoạn được chỉ định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n*k)!) | O(n*k) | Quá chậm | 
| Nhiệm Vụ Tham Lam | O(n*k) | O(n*k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào cho`n`,`k`và danh sách các phân đoạn đã chọn`a`. 
2. Khởi tạo một bộ`used`để theo dõi các phân đoạn đã được chỉ định. 
3. Khởi tạo danh sách các danh sách`children_segments`, mỗi phần có phân đoạn mà trẻ yêu cầu cụ thể. Thêm phân đoạn này vào`used`bộ. 
4. Tạo một trình vòng lặp trên tất cả các số phân đoạn có thể có từ`1`ĐẾN`n*k`. 
5. Với mỗi em, hãy điền phần còn lại của các em`n-1`phân đoạn bằng cách lặp qua tất cả các số phân đoạn và chọn những phân đoạn không có trong`used`. Sau khi chỉ định một phân đoạn, hãy thêm nó vào`used`. 
6. Suy cho cùng thì trẻ em cũng có`n`phân đoạn, làm phẳng`children_segments`liệt kê và in các số theo định dạng yêu cầu. 

Tại sao nó hiệu quả: Mỗi đứa trẻ đều bắt đầu với phân đoạn đã chọn. Phép gán tham lam đảm bảo không có phân đoạn nào bị lặp lại vì chúng tôi luôn kiểm tra lại`used`. Vì tổng số phân đoạn chính xác là`n*k`và mỗi đứa trẻ nhận được chính xác`n`phân đoạn, tất cả các phân đoạn được sử dụng chính xác một lần, đáp ứng tất cả các ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

used = set(a)
children_segments = [[x] for x in a]

all_segments = iter(range(1, n*k + 1))

for i in range(k):
    while len(children_segments[i]) < n:
        seg = next(all_segments)
        if seg not in used:
            children_segments[i].append(seg)
            used.add(seg)

# Output the segments child by child
for child in children_segments:
    print(" ".join(map(str, child)))
```Đầu tiên, mã sẽ đọc đầu vào và khởi tạo từng phần tử con với phân đoạn mong muốn của chúng. Một bộ đảm bảo chúng ta không chỉ định cùng một phân đoạn hai lần. Việc lặp lại tất cả các phân đoạn đảm bảo rằng cuối cùng mọi đứa trẻ đều nhận được chính xác`n`phân đoạn. Việc làm phẳng và in được thực hiện theo thứ tự con để phù hợp với định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 2
4 1
```| Bước | Con 1 đoạn | Con 2 đoạn | Các phân đoạn có sẵn tiếp theo | 
| --- | --- | --- | --- | 
| Ban đầu | [4] | [1] | 1,2,3,4 | 
| Điền Con 1 | [4,2] | [1] | đã sử dụng = {1,2,4} | 
| Điền Con 2 | [4,2] | [1,3] | đã sử dụng = {1,2,3,4} | 

Đầu ra:```
4 2
1 3
```Điều này xác nhận rằng mỗi đứa trẻ nhận được chính xác 2 phân đoạn, bao gồm cả phân đoạn chúng đã chọn mà không bị trùng lặp. 

**Mẫu 2** 

đầu vào:```
3 3
3 1 9
```| Bước | Con 1 | Con 2 | Con 3 | Có sẵn tiếp theo | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [3] | [1] | [9] | 1..9 | 
| Điền Con 1 | [3,2,4] | [1] | [9] | đã sử dụng={1,2,3,4,9} | 
| Điền Con 2 | [3,2,4] | [1,5,6] | [9] | đã sử dụng={1,2,3,4,5,6,9} | 
| Điền Con 3 | [3,2,4] | [1,5,6] | [9,7,8] | đã sử dụng={1..9} | 

Đầu ra:```
3 2 4
1 5 6
9 7 8
```Dấu vết xác nhận rằng phép gán tham lam thỏa mãn tất cả các ràng buộc cho nhiều phần tử con và phân đoạn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*k) | Mỗi phân đoạn được xem xét nhiều nhất một lần; mỗi đứa trẻ được điền chính xác vào`n`phân đoạn. | 
| Không gian | O(n*k) | Lưu trữ các phân đoạn được chỉ định và tập hợp các phân đoạn được sử dụng. | 

Được cho`n*k ≤ 900`, thuật toán thực hiện tối đa 900 lần lặp, trong vòng 2 giây và dung lượng bộ nhớ nhỏ, dưới 1 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    used = set(a)
    children_segments = [[x] for x in a]
    all_segments = iter(range(1, n*k + 1))
    for i in range(k):
        while len(children_segments[i]) < n:
            seg = next(all_segments)
            if seg not in used:
                children_segments[i].append(seg)
                used.add(seg)
    return "\n".join(" ".join(map(str, child)) for child in children_segments)

# provided sample
assert run("2 2\n4 1\n") == "4 2\n1 3" or run("2 2\n4 1\n") == "2 4\n1 3"

# custom: minimum input
assert run("1 1\n1\n") == "1"

# custom: maximum n and k
inp = "30 30\n" + " ".join(map(str, range(1,31))) + "\n"
out = run(inp)
assert all(len(line.split()) == 30 for line in out.split("\n"))

# custom: chosen segments at edges
assert run("3 2\n1 6\n") in ["1 2 3\n6 4 5","1 3 2\n6 4 5","1 2 3\n6 5 4"]

# custom: consecutive chosen segments
assert run("2 3\n2 3 4\n") in ["2 1\n3 5\n4 6"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 1 | kích thước tối thiểu | 
| 30 30 + 1..30 | Mỗi dòng có 30 số | kích thước tối đa, tất cả trẻ em đều được lấp đầy | 
| 3 2\n1 6 | 1 2 3\n6 4 5 | các đoạn được chọn ở ranh giới | 
| 2 3\n2 3 4 | 2 1\n3 5\n4 6 | các đoạn được chọn liên tiếp | 

## Vỏ cạnh 

Dành cho một đứa trẻ và một phân khúc`1 1\n1`, thuật toán khởi tạo phần tử con với phân đoạn đã chọn và vòng lặp while bị bỏ qua, tạo ra`[1]`một cách chính xác. 

Đối với đầu vào kích thước tối đa`n=k=30`với 30 phân đoạn đầu tiên do trẻ chọn, thuật toán sẽ tuần tự điền vào 29 phân đoạn còn lại cho mỗi trẻ từ các số chưa được gán từ 31 đến 900. Bất biến là không có phân đoạn nào được sử dụng lại giữ nguyên vì mỗi phân đoạn mới được gán đều được kiểm tra dựa trên`used`bộ. Điều này xác nhận rằng thuật toán mở rộng đến giới hạn trên mà không vi phạm các ràng buộc của bài toán.
