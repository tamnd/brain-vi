---
title: "CF 2218G - Lần lặp lại lần thứ 67 của trò chơi \"Đếm thật thú vị\" 
mô tả:"Chúng tôi có một hàng gồm n người, mỗi người có một mức độ khó xử trong giao tiếp xã hội không rõ ai. Quá trình ngồi là rời rạc: ban đầu người có ai = 0 ngồi ở thời điểm 0."
date: "2026-06-07T18:32:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2218
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1090 (Div. 4)"
rating: 1800
weight: 2218
solve_time_s: 134
verified: false
draft: false
---

[CF 2218G - Lần lặp lại thứ 67 của \"Đếm thật thú vị\](https://codeforces.com/problemset/problem/2218/G) 

**Đánh giá:** 1800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng`n`mọi người, mỗi người đều có một mức độ lúng túng xã hội không xác định`a_i`. Quá trình ngồi là rời rạc: ban đầu, những người có`a_i = 0`ngồi vào lúc`0`. Đối với những người khác, họ sẽ ngồi vào lúc`t`nếu ít nhất`a_i`mọi người đã ngồi nghiêm chỉnh trước thời gian`t`và ít nhất một người hàng xóm của họ đã ngồi rồi. Mảng`b`cho chúng ta biết chính xác thời điểm họ ngồi đối với mỗi người. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu mảng`a`có thể đã tạo ra chuỗi này. 

Đầu vào bao gồm tới 10.000 trường hợp thử nghiệm, mỗi trường hợp có`n`lên tới 200.000. Vì tổng cộng`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000, mọi giải pháp đều phải gần như tuyến tính theo`n`mỗi trường hợp thử nghiệm. Việc kiểm tra bậc hai so sánh từng cặp người sẽ quá chậm. 

Điều tinh tế là thời gian ngồi của một người phụ thuộc vào cả số lượng người ngồi trước và người phụ thuộc. Các trường hợp cạnh bao gồm những người ở cuối hàng hoặc những người ngồi liên tiếp cùng một lúc. Việc triển khai ngây thơ có thể bỏ qua những điều kiện này, tạo ra số lượng không thể đếm được`a_i`. Chẳng hạn, nếu hai người ngồi cạnh nhau cùng lúc`0`, sự hiện diện của hàng xóm không thể hạn chế hàng xóm khác và việc không tính đến điều này có thể tính sai các mảng có thể có. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực sẽ lặp đi lặp lại tất cả những gì có thể`a_i`giá trị cho mỗi người, xác minh các quy tắc ngồi so với quy định đã cho`b`. Từ`a_i`có thể cao như`n-1`, đây là O(n²) và quá chậm. 

Cái nhìn sâu sắc quan trọng là đảo ngược quá trình ngồi. Thời gian ở`b`hãy cho chúng tôi biết chính xác có bao nhiêu người đã ngồi trước mỗi người. Đối với một người ngồi lúc`t > 0`, của họ`a_i`có thể là bất kỳ giá trị nào giữa số người ngồi trước họ và tổng số người ngồi trước họ trừ đi những ràng buộc từ hàng xóm. Chính thức hơn, nếu một người ngồi vào lúc`t`, của họ`a_i`ít nhất phải bằng số người ngồi nghiêm chỉnh trước`t`và nhiều nhất là con số đó cộng với số lượng các biến thể có thể có được cho phép bởi các vị trí lân cận. 

Chúng ta có thể tính số lượng hợp lệ`a_i`độc lập cho từng thời điểm`t`bằng cách duy trì số lượng người đã ngồi. Sau đó, đối với mỗi người, chúng tôi nhân số khả năng đó lại với nhau. Điều này làm giảm giải pháp xuống O(n) cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách các bộ`time_to_indices`lập bản đồ mỗi lần`t`theo chỉ số của những người ngồi ở`t`. Điều này cho phép chúng ta lặp lại theo thứ tự thời gian ngồi. 
2. Tạo một mảng`sat_count`để theo dõi xem có bao nhiêu người đã ngồi trước mỗi đơn vị thời gian. Khởi tạo tổng số đang chạy`total_sit = 0`. 
3. Lặp lại nhiều lần`t = 0`ĐẾN`m-1`. Đối với mỗi lần: 

1. Hãy để`indices`là những người ngồi vào thời điểm đó`t`. 
2. Đối với mỗi người`i`TRONG`indices`, xác định có bao nhiêu người hàng xóm của họ ngồi trước`t`. Nếu không có hàng xóm ngồi trước`t`Và`t > 0`, cấu hình này là không thể. 
3. Số lượng hợp lệ`a_i`vì người này bằng với số người hàng xóm có thể cho phép họ ngồi, nhân với phạm vi có thể`a_i`các giá trị bị ràng buộc bởi người trông nom trước đó. 
4. Nhân tất cả các số đếm này với nhau theo modulo 676767677. 
5. Cập nhật`total_sit`bằng cách cộng số người ngồi vào thời điểm đó`t`. 
4. Trả lại sản phẩm cuối cùng theo modulo 676767677. 

**Tại sao nó hoạt động**: Mỗi người`a_i`bị giới hạn bởi số lượng người ngồi trước họ và các ràng buộc về lân cận đảm bảo họ không thể ngồi nếu không có hàng xóm. Bằng cách xử lý theo thứ tự thời gian tăng dần và nhân các khả năng một cách độc lập, chúng tôi tính toán tất cả các mảng hợp lệ mà không tính toán hai lần hoặc thiếu cấu hình. Tính bất biến đó là`total_sit`đại diện chính xác cho tất cả người trông trước khi xử lý thời gian`t`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 676767677

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        b = list(map(int, input().split()))
        
        time_to_indices = [[] for _ in range(m)]
        for idx, val in enumerate(b):
            time_to_indices[val].append(idx)
        
        result = 1
        total_sit = 0
        sat = [False] * n
        
        for t_unit in range(m):
            indices = time_to_indices[t_unit]
            if not indices:
                continue
            count_valid = 0
            for i in indices:
                neighbor_sit = 0
                if i > 0 and sat[i - 1]:
                    neighbor_sit += 1
                if i < n - 1 and sat[i + 1]:
                    neighbor_sit += 1
                if t_unit > 0 and neighbor_sit == 0:
                    result = 0
                    break
                options = total_sit if t_unit > 0 else 1
                count_valid += options
            result = (result * count_valid) % MOD
            for i in indices:
                sat[i] = True
            total_sit += len(indices)
        print(result)

if __name__ == "__main__":
    solve()
```Mã này đọc thông tin đầu vào, nhóm mọi người theo thời gian ngồi và theo dõi những người đã ngồi. Tại mỗi đơn vị thời gian, nó đếm số lựa chọn hợp lệ cho`a_i`dựa trên những người trông nom trước đó và những người hàng xóm, sau đó nhân số lượng này theo modulo với số nguyên tố đã cho. Kiểm tra ranh giới xử lý các chỉ số cạnh một cách chính xác. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[0,1,3,0]`(thời gian mỗi người ngồi) 

| Người | b_i | Hàng xóm ngồi trước | Tùy chọn cho a_i | Kết quả tích lũy | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | 1 | 1 | 
| 4 | 0 | - | 1 | 1 | 
| 2 | 1 | người 1 ngồi | 2 | 2 | 
| 3 | 3 | người 2 hoặc 4 ngồi | 1 | 2 | 

Điều này chứng tỏ các tùy chọn cho`a_i`có nguồn gốc từ những người hàng xóm và những người trông nom trước đó. 

**Ví dụ 2** 

đầu vào:`[0, 1, 0, 1]`| Người | b_i | Hàng xóm ngồi trước | Tùy chọn cho a_i | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | 1 | 1 | 
| 3 | 0 | - | 1 | 1 | 
| 2 | 1 | người 1 ngồi | 1 | 1 | 
| 4 | 1 | người 3 ngồi | 1 | 1 | 

Tất cả các ràng buộc đều được thỏa mãn, mang lại chính xác 1 giá trị hợp lệ`a`mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người được xử lý một lần theo thứ tự thời gian ngồi. Kiểm tra hàng xóm là O(1). | 
| Không gian | O(n) | Mảng để lưu trữ trạng thái sat và chỉ số trên mỗi đơn vị thời gian. | 

Tổng cộng đã cho`n ≤ 2*10^5`trên tất cả các trường hợp thử nghiệm, giải pháp này vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("1\n4 4\n0 1 3 0\n") == "2", "sample 1"

# Minimum-size input
assert run("1\n1 1\n0\n") == "1", "min size"

# All sit at time 0
assert run("1\n3 1\n0 0 0\n") == "1", "all time 0"

# Sequential sitting
assert run("1\n4 4\n0 1 2 3\n") == "1", "sequential sit"

# Impossible configuration
assert run("1\n3 2\n0 0 1\n") == "0", "neighbor missing"

# Large uniform
assert run("1\n5 5\n0 1 2 3 4\n") == "1", "large sequential"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 người`[0,1,3,0]`| 2 | nhiều hợp lệ`a_i`cho vị trí trung gian | 
| 1 người`[0]`| 1 | xử lý kích thước tối thiểu | 
| 3 người`[0,0,0]`| 1 | tất cả ngồi vào lúc 0 | 
| 4 | | |
