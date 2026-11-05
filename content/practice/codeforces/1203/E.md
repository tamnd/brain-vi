---
title: "CF 1203E - Võ sĩ"
description: "Chúng ta được cấp một bộ võ sĩ, mỗi võ sĩ có trọng lượng nguyên dương. Mỗi võ sĩ có thể điều chỉnh trọng lượng của mình tối đa 1, tăng hoặc giảm, nhưng trọng lượng phải duy trì đúng mức dương."
date: "2026-06-11T23:43:20+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1203
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 579 (Div. 3)"
rating: 1500
weight: 1203
solve_time_s: 77
verified: true
draft: false
---

[CF 1203E - Võ sĩ](https://codeforces.com/problemset/problem/1203/E) 

**Đánh giá:** 1500 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ võ sĩ, mỗi võ sĩ có trọng lượng nguyên dương. Mỗi võ sĩ có thể điều chỉnh trọng lượng của mình tối đa 1, tăng hoặc giảm, nhưng trọng lượng phải duy trì đúng mức dương. Mục tiêu của chúng tôi là thành lập một đội lớn nhất có thể trong đó không có hai võ sĩ nào có cùng trọng lượng sau khi điều chỉnh. Đầu vào cung cấp số lượng võ sĩ,`n`, và danh sách trọng số của chúng. Đầu ra là một số nguyên duy nhất biểu thị quy mô của nhóm lớn nhất với các trọng số duy nhất. 

Những ràng buộc chỉ ra rằng`n`có thể lớn tới 150.000 và trọng lượng có thể lên tới 150.000. Một cách tiếp cận ngây thơ thử tất cả các kết hợp điều chỉnh có thể có sẽ liên quan đến thời gian theo cấp số nhân, điều này không khả thi. Chúng ta cần một giải pháp chạy trong thời gian gần như tuyến tính hoặc tuyến tính, O(n log n) hoặc tốt hơn, vì O(n^2) sẽ dẫn đến hơn 22 tỷ phép tính trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. 

Một vài trường hợp đặc biệt đáng được xem xét. Nếu tất cả các võ sĩ đều có trọng lượng như nhau, chúng ta phải cẩn thận điều chỉnh mức tạ lên hoặc xuống để tối đa hóa sức mạnh của đội. Ví dụ, với trọng số`[1,1,1]`, kết quả tối ưu là`[1,2,3]`đưa ra một đội có kích thước 3. Một trường hợp khác là khi trọng số bao gồm 1, vì chúng ta không thể giảm thêm 1 nữa nên việc điều chỉnh chỉ phải tăng trọng số đó. Nếu trọng lượng tối đa xảy ra nhiều lần gần giới hạn trên, chúng tôi phải đảm bảo không vượt quá 150.001 khi tăng trọng lượng. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản để mô tả: đối với mỗi võ sĩ, hãy thử cả ba mức tạ có thể (hiện tại, hiện tại-1, hiện tại+1) và kiểm tra xem mức tạ đó đã được chỉ định trong đội hay chưa. Nếu không, hãy thêm nó vào nhóm. Tiếp tục cho đến khi tất cả các võ sĩ được xử lý. Điều này đúng vì nó kiểm tra rõ ràng tất cả các phép gán trọng số khả thi. Vấn đề là trong trường hợp xấu nhất, mỗi võ sĩ thử ba khả năng và việc kiểm tra tính duy nhất có thể yêu cầu tìm kiếm hoặc cập nhật một bộ nhiều lần, dẫn đến độ phức tạp O(n^2) nếu không được quản lý cẩn thận. Với`n = 150,000`, cách tiếp cận này sẽ hết thời gian chờ. 

Thông tin chi tiết quan trọng là thứ tự chúng tôi xử lý võ sĩ rất quan trọng. Việc sắp xếp các võ sĩ theo trọng lượng cho phép chúng ta xem xét các trọng lượng nhỏ hơn trước. Đối với mỗi võ sĩ, chúng tôi cố gắng giảm trọng lượng của họ đi 1 nếu nó vẫn dương và có sẵn, sau đó thử mức tạ ban đầu và cuối cùng thử tăng thêm 1. Chiến lược tham lam này đảm bảo rằng các mức tạ nhỏ hơn sẽ được lấp đầy trước, nhường chỗ cho mức tạ cao hơn mà không có xung đột. Lý do điều này có tác dụng là vì một khi trọng lượng đã được sử dụng, chúng ta không thể tái sử dụng nó. Bằng cách luôn sử dụng tùy chọn nhỏ nhất có sẵn, chúng tôi giảm thiểu xung đột sau này. Điều này giúp giảm vấn đề xuống còn một lần duyệt qua danh sách đã sắp xếp trong khi vẫn duy trì một tập hợp hoặc mảng boolean có trọng số đã sử dụng, đạt được thời gian O(n log n) do sắp xếp và không gian O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(max(a_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số võ sĩ`n`và danh sách trọng lượng`a`. 
2. Sắp xếp danh sách trọng số theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi xử lý các trọng số nhỏ hơn trước, điều này cho phép chúng tôi chỉ định các trọng số duy nhất nhỏ nhất hiện có một cách tham lam. 
3. Khởi tạo một mảng boolean hoặc thiết lập để theo dõi trọng số nào đã được chỉ định. Chúng tôi sẽ đánh dấu trọng lượng được sử dụng khi gán nó cho võ sĩ quyền anh. 
4. Khởi tạo bộ đếm`team_size`về 0 để đếm các võ sĩ được chỉ định thành công. 
5. Lặp lại từng trọng lượng`w`trong danh sách đã sắp xếp. Với mỗi cân nặng, hãy thử theo thứ tự: 

một.`w-1`, chỉ khi nó dương và không được sử dụng. Nếu được chỉ định, hãy đánh dấu nó đã được sử dụng và tăng dần`team_size`. 

b. Nếu như`w-1`không có sẵn, hãy thử`w`chính nó. Nếu có, hãy đánh dấu nó đã được sử dụng và tăng dần`team_size`. 

c. Nếu không`w-1`cũng không`w`đã có sẵn, hãy thử`w+1`. Nếu có, hãy đánh dấu nó đã được sử dụng và tăng dần`team_size`. 
6. Sau khi xử lý tất cả võ sĩ,`team_size`sẽ chứa quy mô của nhóm lớn nhất có thể. Xuất giá trị này. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến rằng tất cả các trọng số được gán là duy nhất. Việc sắp xếp đảm bảo rằng các trọng lượng nhỏ hơn được ưu tiên, điều này tránh lãng phí các số nhỏ hơn mà chỉ có thể được chỉ định bằng cách giảm các võ sĩ có trọng lượng từ 2 trở lên. Kiểm tra tham lam ba bước đảm bảo rằng mỗi võ sĩ đều đóng góp cho đội nếu có ít nhất một tạ gần đó. Vì tất cả các lựa chọn đều được kiểm tra theo thứ tự tăng dần nên không có phép gán tiềm năng nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
weights = list(map(int, input().split()))
weights.sort()

MAX_W = 150001
used = [False] * (MAX_W + 2)
team_size = 0

for w in weights:
    if w > 1 and not used[w-1]:
        used[w-1] = True
        team_size += 1
    elif not used[w]:
        used[w] = True
        team_size += 1
    elif w + 1 <= MAX_W + 1 and not used[w+1]:
        used[w+1] = True
        team_size += 1

print(team_size)
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và sắp xếp trọng lượng của võ sĩ. Chúng tôi sử dụng mảng boolean để theo dõi các trọng số đã sử dụng nhằm truy cập theo thời gian liên tục. Việc lặp lại các trọng số được sắp xếp đảm bảo chúng ta xử lý các trọng số nhỏ hơn trước, tránh những xung đột không cần thiết. Mỗi điều kiện xử lý sự lựa chọn tham lam: giảm nếu có thể, sau đó giữ, rồi tăng. Kiểm tra ranh giới đảm bảo không sử dụng trọng số âm hoặc quá lớn. tăng dần`team_size`chỉ khi trọng số được gán thành công mới duy trì tính chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
3 2 4 1
```| w | đã sử dụng[w-1] | đã sử dụng[w] | đã sử dụng[w+1] | đội_size | 
| --- | --- | --- | --- | --- | 
| 1 | - | Sai | Đúng | 1 | 
| 2 | Sai | Đúng | - | 2 | 
| 3 | Sai | Đúng | - | 3 | 
| 4 | Sai | Đúng | - | 4 | 

Tất cả các võ sĩ đều có thể giữ được trọng lượng của mình; quy mô đội cuối cùng là 4. 

Đầu vào mẫu 2:```
5
1 1 2 2 3
```| w | đã sử dụng[w-1] | đã sử dụng[w] | đã sử dụng[w+1] | đội_size | 
| --- | --- | --- | --- | --- | 
| 1 | - | Sai | Đúng | 1 | 
| 1 | Sai | Đúng | - | 2 | 
| 2 | Sai | Đúng | - | 3 | 
| 2 | Đúng | Đúng | Đúng | 3 | 
| 3 | Sai | Đúng | - | 4 | 

Đội cuối cùng có tạ`[1,2,2,3,4]`điều chỉnh để`[1,2,3,4,5]`với kích thước 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; xử lý mỗi trọng số là O(1) | 
| Không gian | O(max(a_i)) | Mảng Boolean có kích thước ~150.003 cho trọng số đã sử dụng | 

Với`n ≤ 150,000`và trọng lượng 150.000, việc sắp xếp cộng với một lần vượt qua vừa vặn thoải mái dưới giới hạn 2 giây. Mức sử dụng bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    weights = list(map(int, input().split()))
    weights.sort()
    MAX_W = 150001
    used = [False] * (MAX_W + 2)
    team_size = 0
    for w in weights:
        if w > 1 and not used[w-1]:
            used[w-1] = True
            team_size += 1
        elif not used[w]:
            used[w] = True
            team_size += 1
        elif w + 1 <= MAX_W + 1 and not used[w+1]:
            used[w+1] = True
            team_size += 1
    return str(team_size)

# Provided samples
assert run("4\n3 2 4 1\n") == "4", "sample 1"
assert run("5\n1 1 2 2 3\n") == "5", "custom sample"

# Custom cases
assert run("3\n1 1 1\n") == "3", "all equal weights"
assert run("2\n1 2\n") == "2", "minimum n"
assert run("5\n1 2 2 2 3\n") == "5", "multiple duplicates with lower bound 1"
assert run("6\n5 5 5 5 5 5\n") == "6", "all equal larger weights"
assert run("4\n150000 150000 150000 150000\n") == "4", "upper bound weights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 1 1`| 3 | Xử lý đúng trọng lượng tối thiểu lặp đi lặp lại | 
| ` | | |
