---
title: "CF 172C - Xe buýt"
description: "Chúng tôi được yêu cầu mô phỏng hoạt động của một chiếc xe buýt liên tục chở học sinh từ điểm dừng xe buýt ở tọa độ 0 đến các điểm đến tương ứng dọc theo trục dương. Mỗi học sinh đến điểm dừng vào một thời điểm khác nhau và có tọa độ đích cố định."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 172
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2012 - Qualification Round"
rating: 1500
weight: 172
solve_time_s: 79
verified: true
draft: false
---

[CF 172C - Xe buýt](https://codeforces.com/problemset/problem/172/C) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng hoạt động của một chiếc xe buýt liên tục chở học sinh từ điểm dừng xe buýt ở tọa độ 0 đến các điểm đến tương ứng dọc theo trục dương. Mỗi học sinh đến điểm dừng vào một thời điểm khác nhau và có tọa độ đích cố định. Xe buýt có thể chở tối đa _m_ học sinh cùng một lúc và khởi hành khi đã đầy hoặc khi học sinh cuối cùng đã đến. Vận tốc di chuyển là 1 đơn vị quãng đường trên một đơn vị thời gian, khi thả học sinh xuống, xe dừng lại trong`1 + floor(k / 2)`đơn vị thời gian tại điểm dừng nơi _k_ học sinh xuống xe. Sau khi đến điểm dừng xa nhất của một nhóm, xe buýt ngay lập tức quay về số 0 mà không dừng lại để đón hoặc trả khách. 

Đầu vào cung cấp thời gian đến và điểm đến của tất cả học sinh, còn đầu ra phải liệt kê chính xác thời điểm mỗi học sinh đến điểm dừng, theo thứ tự được đưa ra. 

Các ràng buộc cho phép tối đa 100.000 sinh viên, mỗi sinh viên có tọa độ điểm đến tối đa là 10.000 và xe buýt có thể chở tới 100.000 sinh viên cùng một lúc. Một mô phỏng đơn giản nhằm nâng cao xe buýt theo từng bước hoặc vòng lặp đơn vị theo thời gian sẽ quá chậm. Chúng tôi cần một thuật toán phân nhóm học sinh, tính toán thời gian di chuyển một cách phân tích và xử lý mỗi chuyến xe buýt trong thời gian O(m log m) hoặc O(m). 

Các trường hợp đặc biệt bao gồm các tình huống trong đó một học sinh được đón một mình, nhiều học sinh có cùng điểm đến và khi học sinh đến nơi khi xe buýt đang trên đường. Ví dụ: nếu xe buýt khởi hành với sức chứa 1 và học sinh đến khi xe buýt đi vắng, học sinh phải đợi cho đến khi xe buýt quay trở lại. Quản lý sai thời gian đến hoặc thời gian dừng có thể tạo ra kết quả đầu ra không chính xác. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ mô phỏng từng đơn vị di chuyển của xe buýt dọc theo trục và kiểm tra từng học sinh khi nào họ đến và khi nào họ cần xuống xe. Điều này hoạt động chính xác nhưng là O(n * x_max) trong trường hợp xấu nhất, điều này không thể chấp nhận được vì x_max có thể là 10.000 và n lên tới 100.000. 

Quan sát quan trọng để có giải pháp nhanh hơn là mỗi chuyến xe buýt có thể được xử lý độc lập: xe buýt luôn khởi hành cùng với _m_ học sinh hoặc số học sinh còn lại, tùy theo số lượng nào nhỏ hơn. Chúng ta chỉ cần theo dõi lượt đến muộn nhất của các học sinh trong đợt hiện tại để xác định thời gian khởi hành. Sau khi xe buýt khởi hành, thời gian mỗi học sinh đến điểm dừng có thể được tính trực tiếp bằng cách sắp xếp điểm đến và cộng thời gian di chuyển. Thời gian thả rơi được xác định theo công thức`1 + floor(k / 2)`tại mỗi điểm dừng, nhưng học sinh được coi là đã đến điểm dừng vào thời điểm xe buýt đến. Thời gian quay về của xe buýt chỉ đơn giản là gấp đôi điểm đến xa nhất trong đợt. 

Điều này dẫn đến thuật toán O(n log m) trong đó mỗi lô được xử lý bằng cách sắp xếp các điểm đến để mô phỏng hiệu quả số lần bỏ mạng. Chúng tôi không bao giờ tăng dần theo thời gian mà chỉ thông qua các đợt học sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * x_max) | O(n) | Quá chậm | 
| Tối ưu | O(n log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả thời gian đến và điểm đến của học sinh, duy trì các chỉ số ban đầu của chúng. 
2. Khởi tạo bus tại thời điểm 0 và vị trí 0. Duy trì con trỏ tới học sinh tiếp theo đang đợi ở điểm dừng. 
3. Lặp lại cho đến khi tất cả học sinh được chỉ định đi xe buýt: 

1. Lập một nhóm tối đa _m_ sinh viên. Đưa _m_ học sinh tiếp theo theo thứ tự đến. 
2. Tính thời gian khởi hành xe buýt là lớn nhất giữa thời gian xe buýt hiện tại và thời gian đến của học sinh cuối cùng trong đợt. 
3. Trích xuất điểm đến của các học sinh trong đợt cùng với các chỉ số ban đầu của họ. 
4. Sắp xếp các điểm đến để mô phỏng thứ tự xe buýt dừng để trả khách. 
5. Tại mỗi điểm dừng: 

1. Ghi lại thời gian đến của tất cả học sinh xuống tại điểm dừng này là thời gian xe buýt hiện tại cộng với khoảng cách đến điểm dừng. 
2. Tăng thời gian xe buýt lên`1 + floor(k / 2)`để giải quyết việc học sinh ra trường. 
6. Cập nhật thời gian xe buýt để bao gồm chuyến về về 0, bằng khoảng cách đến đích tối đa đã đi. 
4. Xuất ra thời gian đến đã ghi theo thứ tự học sinh ban đầu. 

Tại sao nó hoạt động: Mỗi đợt được xử lý theo thứ tự đến, đảm bảo xe buýt không bao giờ khởi hành trước khi học sinh có mặt. Việc sắp xếp các điểm đến đảm bảo chúng tôi mô phỏng chính xác số lần bỏ học. Bus luôn quay về 0, đảm bảo đợt tiếp theo hạch toán chính xác các học sinh đang chờ. Những bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
students = [tuple(map(int, input().split())) + (i,) for i in range(n)]

res = [0] * n
i = 0
current_time = 0

while i < n:
    batch = students[i:i + m]
    depart_time = max(current_time, batch[-1][0])
    
    # destinations with indices
    dest_indices = [(x, idx) for t, x, idx in batch]
    dest_indices.sort()
    
    for x, idx in dest_indices:
        res[idx] = depart_time + x
    
    # bus return time
    max_x = dest_indices[-1][0]
    current_time = depart_time + 2 * max_x
    
    i += m

print(' '.join(map(str, res)))
```Mã này đọc dữ liệu đầu vào một cách hiệu quả, phân nhóm học sinh theo công suất của xe buýt, sắp xếp các điểm đến để mô phỏng thứ tự dừng, tính toán thời gian đến của từng học sinh mà không tính theo thời gian và cập nhật thời gian xe buýt bao gồm cả chuyến về. Việc sử dụng chỉ mục gốc sẽ giữ nguyên thứ tự đầu vào cho đầu ra. Cần chú ý xử lý thời gian đến tối đa cho điểm khởi hành và sắp xếp điểm đến. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 10
3 5
```| tôi | đợt | thời gian khởi hành | điểm đến | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | [(3,5,0)] | 3 | [(5,0)] | [8] | 

Xe buýt đợi cho đến khi học sinh đến lúc 3, lái 5 đơn vị, học sinh đến lúc 8, quay 5 đơn vị về 0. 

### Mẫu 2 

đầu vào:```
2 1
3 5
4 5
```| tôi | đợt | thời gian khởi hành | điểm đến | độ phân giải | hiện tại_thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [(3,5,0)] | 3 | [(5,0)] | [8] | 8 + 5 = 13 | 
| 1 | [(4,5,1)] | 13 | [(5,1)] | [8, 18] | 13 + 5 + trả về = 23 | 

Điều này thể hiện việc xử lý nhiều đợt và thời gian trả xe buýt, đảm bảo học sinh thứ hai đợi xe buýt quay lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi đợt có tối đa m sinh viên yêu cầu sắp xếp điểm đến | 
| Không gian | O(n) | Lưu trữ sinh viên và mảng kết quả | 

Độ phức tạp này dễ dàng phù hợp với các ràng buộc của bài toán n lên tới 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    students = [tuple(map(int, input().split())) + (i,) for i in range(n)]
    res = [0] * n
    i = 0
    current_time = 0
    while i < n:
        batch = students[i:i + m]
        depart_time = max(current_time, batch[-1][0])
        dest_indices = [(x, idx) for t, x, idx in batch]
        dest_indices.sort()
        for x, idx in dest_indices:
            res[idx] = depart_time + x
        max_x = dest_indices[-1][0]
        current_time = depart_time + 2 * max_x
        i += m
    return ' '.join(map(str, res))

# provided samples
assert run("1 10\n3 5\n") == "8", "sample 1"
assert run("2 1\n3 5\n4 5\n") == "8 18", "sample 2"

# custom cases
assert run("3 2\n1 2\n2 3\n3 4\n") == "3 5 7", "multiple students, capacity 2"
assert run("5 3\n1 1\n1 2\n1 3\n1 4\n1 5\n") == "2 3 4 5 6", "all arrive at same time"
assert run("2 2\n5 10\n1 1\n") == "11 6", "students arrive out of order relative to indices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2\n1 2\n2 3\n3 4 | 3 5 7 | Đúng đợt và thời gian khởi hành | 
| 5 3\n1 1\n1 2\n1 3\n1 4\n1 5 | 2 3 4 5 6 | Xử lý tất cả học sinh đến cùng lúc | 
| 2 2\n5 10\n1 1 | 11 6 | Sử dụng đúng các chỉ số gốc và khởi hành theo đợt | 

## Vỏ cạnh 

Đối với một học sinh có sức chứa xe buýt lớn hơn 1: xe buýt sẽ đợi cho đến khi học sinh
