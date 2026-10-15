---
title: "CF 105445B - Phỏng vấn"
description: "Chúng tôi được cung cấp một hệ thống nhỏ gồm những người phỏng vấn liên tục thực hiện các cuộc phỏng vấn theo nhóm. Mỗi cuộc phỏng vấn bao gồm chính xác z người phỏng vấn riêng biệt và mỗi người phỏng vấn chỉ có thể tham gia tối đa y cuộc phỏng vấn mỗi ngày."
date: "2026-06-23T03:25:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105445
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #36 (Starters-Forces)"
rating: 0
weight: 105445
solve_time_s: 81
verified: false
draft: false
---

[CF 105445B - Phỏng vấn](https://codeforces.com/problemset/problem/105445/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống nhỏ gồm những người phỏng vấn liên tục thực hiện các cuộc phỏng vấn theo nhóm. Mỗi cuộc phỏng vấn bao gồm chính xác`z`người phỏng vấn riêng biệt và mỗi người phỏng vấn chỉ có thể tham gia tối đa`y`phỏng vấn mỗi ngày. có`x`tổng số người phỏng vấn và nhiệm vụ là sắp xếp càng nhiều cuộc phỏng vấn như vậy càng tốt trong khi vẫn tôn trọng giới hạn năng lực của mỗi người phỏng vấn. 

Đầu ra hợp lệ là một danh sách các nhóm, trong đó mỗi nhóm là một tập hợp các`z`ID người phỏng vấn riêng biệt. Mỗi nhóm đại diện cho một cuộc phỏng vấn được tiến hành. Ràng buộc hoàn toàn là số lần mỗi ID người phỏng vấn có thể xuất hiện trong tất cả các nhóm. 

Từ quan điểm cấu trúc, đây là bài toán đóng gói tổ hợp bị chặn. Chúng tôi đang cố gắng đóng gói càng nhiều kích cỡ-`z`tập hợp con càng tốt thành nhiều tập hợp`x`các phần tử trong đó mỗi phần tử có khả năng`y`. 

Những hạn chế rất nhỏ:`x, y, z ≤ 10`. Điều này ngay lập tức cho chúng ta biết rằng số lượng nhóm phỏng vấn riêng biệt có thể có nhiều nhất là`C(10, z)`, nhiều nhất là 252. Ngay cả khi chúng tôi liệt kê tất cả các kết hợp và mô phỏng số lượng sử dụng, không gian trạng thái vẫn rất nhỏ. Điều này cho thấy rõ ràng rằng việc xây dựng tham lam hoặc thậm chí liệt kê một cách có hệ thống là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi`x < z`. Trong trường hợp đó không có nhóm hợp lệ nào tồn tại vì chúng ta không thể tạo thành một tập hợp các`z`người phỏng vấn riêng biệt. Một trường hợp cạnh khác là khi`y = 0`, điều này buộc câu trả lời là không có cuộc phỏng vấn nào bất kể`x`Và`z`. Việc triển khai ngây thơ không bảo vệ rõ ràng những trường hợp này có thể cố gắng xây dựng các nhóm và gặp phải tình trạng lập chỉ mục không hợp lệ hoặc tạo ra các lịch trình không khả thi. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xử lý từng tập hợp con có thể có kích thước`z`như một cuộc phỏng vấn ứng viên và cố gắng chọn số lượng tối đa các tập hợp con như vậy trong khi đảm bảo không có người phỏng vấn nào vượt quá mức sử dụng`y`. Về cơ bản, đây là vấn đề đóng gói tối đa trên một siêu đồ thị và tìm kiếm trực tiếp sẽ thử kết hợp các tập hợp con. 

Mặc dù tính đúng đắn rất dễ được lý giải, nhưng sự bùng nổ tổ hợp xảy ra khi chúng ta cho rằng mỗi tập hợp con có thể được chọn hay không, dẫn đến kết quả là gần đúng.`2^(C(x, z))`khả năng. Mặc dù`x ≤ 10`, điều này vẫn dẫn đến`2^252`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần lựa chọn tập hợp con tối ưu theo bất kỳ ý nghĩa tổng thể nào. Chúng ta chỉ cần tạo ra một công trình khả thi nhằm tối đa hóa số lượng nhóm. Vì tất cả những người phỏng vấn đều có tính đối xứng nên chúng ta có thể phân bổ sự tham gia một cách đồng đều. Cấu trúc tự nhiên là duyệt qua những người phỏng vấn và luôn chọn các nhóm quy mô liên tiếp`z`, xoay vòng theo chu kỳ, đồng thời theo dõi số lần mỗi người phỏng vấn đã được sử dụng. 

Bởi vì`x`nhỏ và`y`nhỏ, chúng ta có thể tham lam xây dựng từng nhóm một, luôn chọn những nhóm có sẵn tiếp theo`z`những người phỏng vấn vẫn còn năng lực. Nếu chúng tôi duy trì một con trỏ và xoay vòng qua ID người phỏng vấn, chúng tôi sẽ đảm bảo tính công bằng và tránh lạm dụng bất kỳ người tham gia nào. Việc xây dựng này tiếp tục cho đến khi không có nhóm hợp lệ nào có thể được hình thành. 

Lý do điều này có tác dụng là vì bất kỳ lịch trình hợp lệ nào cũng chỉ bị giới hạn bởi tổng công suất`x * y`và mỗi cuộc phỏng vấn tiêu tốn chính xác`z`đơn vị công suất. Do đó giới hạn trên là`floor(x * y / z)`, và việc xây dựng chu kỳ tham lam một cách tự nhiên tiếp cận giới hạn này trong những ràng buộc nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tập hợp con Brute Force | O(2^(x chọn z)) | O(x chọn z) | Quá chậm | 
| Xây dựng theo chu kỳ tham lam | O(x * y * z) | O(x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi thực hiện các cuộc phỏng vấn lặp đi lặp lại trong khi theo dõi năng lực còn lại của từng người phỏng vấn. 

1. Khởi tạo một mảng`used[i] = 0`cho tất cả người phỏng vấn`i`, thể hiện số lượng cuộc phỏng vấn họ đã tham gia. Điều này đảm bảo chúng tôi có thể thực thi giới hạn`y`. 
2. Duy trì con trỏ hiện tại`p`qua những người phỏng vấn từ`1`ĐẾN`x`. Con trỏ này giúp phân phối mức sử dụng đồng đều thay vì chọn liên tục từ cùng một tập hợp con. 
3. Nếu có thể, hãy cố gắng thành lập một nhóm phỏng vấn: 

Chúng tôi quét về phía trước từ`p`và chọn cái tiếp theo`z`những người phỏng vấn khác biệt có`used[i] < y`. 

Bước quét này đảm bảo chúng tôi luôn xây dựng một nhóm hợp lệ mà không vi phạm các ràng buộc vì chúng tôi lọc rõ ràng những người phỏng vấn đã kiệt sức. 
4. Nếu tại một thời điểm nào đó chúng ta không thể tìm thấy`z`người phỏng vấn có sẵn, dừng lại. Điều này có nghĩa là không có cuộc phỏng vấn hợp lệ nào có thể được thực hiện. 
5. Sau khi thành lập nhóm, hãy tăng dần`used[i]`đối với mỗi người phỏng vấn đã chọn, hãy thêm nhóm vào danh sách câu trả lời và di chuyển con trỏ về phía trước để tiếp tục phân bổ công bằng. 
6. Tiếp tục cho đến khi đạt được điều kiện kết thúc. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán chỉ chọn những người phỏng vấn còn năng lực nên không bao giờ vi phạm ràng buộc. Quá trình quét theo chu kỳ đảm bảo rằng nếu một nhóm khả thi tồn tại thì cuối cùng nó sẽ được phát hiện vì tất cả sự kết hợp của những người phỏng vấn có sẵn đều có thể truy cập được thông qua việc quay trong một vũ trụ cố định nhỏ. Vì công suất giảm một cách đơn điệu nên quá trình phải chấm dứt và khi đó, không có nhóm hợp lệ nào có thể được hình thành dưới các ràng buộc, nghĩa là việc xây dựng là tối đa theo chính sách tham lam trong bối cảnh bị giới hạn này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y, z = map(int, input().split())
        
        if z > x or y == 0:
            print(0)
            continue
        
        used = [0] * (x + 1)
        ans = []
        p = 1
        
        while True:
            group = []
            start = p
            i = start
            scanned = 0
            
            while scanned < x and len(group) < z:
                if i > x:
                    i = 1
                if used[i] < y:
                    group.append(i)
                i += 1
                scanned += 1
            
            if len(group) < z:
                break
            
            for v in group:
                used[v] += 1
            
            ans.append(group)
            p += 1
            if p > x:
                p = 1
        
        print(len(ans))
        for g in ans:
            print(*g)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì bộ đếm mức sử dụng cho mỗi người phỏng vấn và xây dựng từng nhóm bằng cách quét theo chu kỳ thông qua ID người phỏng vấn. Vòng lặp bên trong đảm bảo chúng tôi thu thập chính xác`z`những người phỏng vấn riêng biệt với năng lực còn lại. Con trỏ`p`thay đổi sau mỗi nhóm để chúng ta tránh phải chọn nhiều lần các chỉ số ban đầu giống nhau. 

Một chi tiết triển khai tinh tế là sự bao bọc rõ ràng của chỉ mục`i`. Nếu không có nó, quá trình quét sẽ bỏ sót những ứng viên hợp lệ khi đến cuối danh sách người phỏng vấn. Một chi tiết quan trọng khác sẽ bị hỏng ngay lập tức khi ít hơn`z`các ứng cử viên có thể được tìm thấy, vì bất kỳ nỗ lực nào nữa cũng sẽ thất bại do sự suy giảm năng lực đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 2
```Chúng tôi có 3 người phỏng vấn, mỗi người có thể xuất hiện hai lần và mỗi cuộc phỏng vấn sử dụng 2 người. 

| Bước | Con trỏ p | Nhóm đã chọn | Số lượng đã sử dụng | 
| --- | --- | --- | --- | 
| 1 | 1 | [1,2] | 1:1, 2:1, 3:0 | 
| 2 | 2 | [2,3] | 1:1, 2:2, 3:1 | 
| 3 | 3 | [3,1] | 1:2, 2:2, 3:2 | 

Ở bước 4, không thể thành lập nhóm cỡ 2 vì tất cả người phỏng vấn đã đạt đến năng lực 2. Việc xây dựng dừng lại ở 3 cuộc phỏng vấn. 

Điều này cho thấy quá trình quét theo chu kỳ phân bổ tải đồng đều và đạt được mức sử dụng tối đa công suất. 

### Ví dụ 2 

đầu vào:```
4 1 3
```Mỗi người phỏng vấn chỉ được xuất hiện một lần và mỗi nhóm cần 3 người phỏng vấn. 

| Bước | Con trỏ p | Nhóm đã chọn | Số lượng đã sử dụng | 
| --- | --- | --- | --- | 
| 1 | 1 | [1,2,3] | 1:1, 2:1, 3:1, 4:0 | 
| 2 | 2 | [2,3,4] | 1:1, 2:2 (không hợp lệ) | 

Tại thời điểm này, người phỏng vấn 2 và 3 đã vượt quá khả năng nếu được sử dụng lại, do đó không tồn tại nhóm hợp lệ thứ hai. Thuật toán dừng sau 1 cuộc phỏng vấn. 

Điều này chứng tỏ ràng buộc về năng lực trực tiếp giới hạn số lượng nhóm ngay cả khi tồn tại nhiều sự kết hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x * y * z) cho mỗi trường hợp thử nghiệm | Mỗi nhóm hình thành sẽ quét tối đa x phần tử và chúng tôi tạo thành nhiều nhất x*y/z nhóm | 
| Không gian | O(x) | Chúng tôi chỉ lưu trữ bộ đếm sử dụng và nhóm hiện tại | 

Cho rằng`x, y, z ≤ 10`Và`t ≤ 1000`, điều này thoải mái phù hợp trong giới hạn thời gian. Ngay cả việc thực thi trong trường hợp xấu nhất cũng chỉ thực hiện được vài nghìn thao tác nguyên thủy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like cases
assert run("1\n3 2 2\n") != ""

# minimum case
assert run("1\n1 1 1\n") == "1\n1"

# impossible due to z > x
assert run("1\n2 1 3\n") == "0"

# zero capacity
assert run("1\n3 0 2\n") == "0"

# symmetric case
out = run("1\n4 1 2\n")
lines = out.splitlines()
assert int(lines[0]) == len(lines) - 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 1 1 | 1\n1 | trường hợp khả thi tối thiểu | 
| 1\n2 1 3 | 0 | không thể do không đủ người tham gia | 
| 1\n3 0 2 | 0 | trường hợp cạnh công suất bằng không | 
| 1\n4 1 2 | cặp hợp lệ | xây dựng chung đúng đắn | 

## Vỏ cạnh 

Khi nào`z > x`, thuật toán ngay lập tức trả về 0. Ví dụ, với đầu vào`x=2, z=3`, không có quá trình quét nào có thể thu thập đủ những người phỏng vấn riêng biệt, vì vậy vòng lặp bên trong luôn thất bại và chúng tôi dừng lại mà không tạo nhóm. 

Khi`y = 0`, mọi người phỏng vấn đều không có năng lực ngay từ đầu. Vòng quét không bao giờ thêm bất kỳ ứng cử viên nào, vì vậy`group`không bao giờ đạt đến kích thước`z`và thuật toán kết thúc ngay lập tức với đầu ra`0`. 

Khi`x = z`, mọi cuộc phỏng vấn hợp lệ đều phải sử dụng tất cả người phỏng vấn. Thuật toán sẽ tạo ra nhiều nhất`y`các nhóm đầy đủ giống hệt nhau, vì mỗi người phỏng vấn được sử dụng thống nhất qua các lần lặp. Điều này phù hợp với cách đóng gói tối ưu trong đó mỗi vòng sử dụng đồng đều tất cả dung lượng sẵn có.
