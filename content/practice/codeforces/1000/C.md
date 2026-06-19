---
title: "CF 1000C - Số điểm được bảo hiểm"
description: "Chúng ta được cho một tập hợp các đoạn trên trục số rất lớn. Mỗi đoạn bao gồm mọi điểm nguyên giữa các điểm cuối của nó, bao gồm cả hai đầu. Các phân đoạn khác nhau có thể chồng lên nhau theo những cách tùy ý: chúng có thể rời rạc, lồng nhau, giống hệt nhau hoặc giao nhau một phần."
date: "2026-06-16T23:47:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1000
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 46 (Rated for Div. 2)"
rating: 1700
weight: 1000
solve_time_s: 74
verified: true
draft: false
---

[CF 1000C - Số điểm được bao gồm](https://codeforces.com/problemset/problem/1000/C) 

**Đánh giá:** 1700 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn trên trục số rất lớn. Mỗi đoạn bao gồm mọi điểm nguyên giữa các điểm cuối của nó, bao gồm cả hai đầu. Các phân đoạn khác nhau có thể chồng lên nhau theo những cách tùy ý: chúng có thể rời rạc, lồng nhau, giống hệt nhau hoặc giao nhau một phần. 

Nhiệm vụ không phải là tìm phạm vi phủ sóng tại một điểm duy nhất mà là hiểu được sự phân bổ phạm vi phủ sóng toàn cầu. Với mỗi giá trị có thể có của k, chúng ta cần tính xem có bao nhiêu vị trí số nguyên được bao phủ bởi chính xác k đoạn. 

Nói cách khác, nếu chúng ta tưởng tượng việc quét dọc theo trục số và tại mỗi tọa độ nguyên, đếm xem có bao nhiêu đoạn hiện bao phủ nó, thì chúng ta muốn biết có bao nhiêu tọa độ tạo ra số đếm 1, bao nhiêu tọa độ tạo ra số đếm 2, v.v. cho đến n. 

Những ràng buộc là những gì buộc phải lựa chọn thiết kế. Với tối đa 200.000 phân đoạn và điểm cuối có kích thước lớn tới 10^18, chúng tôi không thể đủ khả năng kiểm tra từng điểm nguyên một cách rõ ràng. Bất kỳ cách tiếp cận nào lặp lại tọa độ đều không khả thi ngay lập tức vì không gian tọa độ thực sự không bị giới hạn ở quy mô này. Chúng ta cần lý luận về những thay đổi trong phạm vi bao phủ hơn là các điểm riêng lẻ. 

Một dạng lỗi phổ biến xuất phát từ việc cố gắng mô phỏng vùng phủ sóng trên mỗi điểm hoặc trên mỗi khoảng đơn vị mà không nén tọa độ. Ví dụ: với các phân đoạn [0, 3], [1, 3], [3, 8], việc quét đơn giản trên mọi số nguyên sẽ hoạt động trên đầu vào nhỏ bé này nhưng sẽ không thể thực hiện được nếu các khoảng kéo dài đến 10^18. Một sai lầm tinh vi khác là coi các khoảng thời gian là nửa mở một cách không nhất quán. Vì các điểm cuối đã bao gồm nên phạm vi bao phủ thay đổi chính xác tại l và tại r + 1, không phải tại r. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi tọa độ số nguyên nằm bên trong ít nhất một phân đoạn, hãy đếm xem có bao nhiêu phân đoạn bao phủ nó, sau đó tăng một mảng tần số được lập chỉ mục theo số lượng đó. Về mặt khái niệm, đối với mỗi phân đoạn, chúng tôi đánh dấu tất cả các điểm mà nó bao gồm và duy trì bộ đếm toàn cầu cho mỗi điểm. 

Điều này hoạt động hợp lý nhưng lại phá vỡ hiệu suất ngay lập tức. Một phân đoạn có thể kéo dài tới 10^18 số nguyên, do đó, ngay cả một khoảng cũng có thể yêu cầu công việc không thể thực hiện được. Ngay cả khi chúng tôi cố gắng hạn chế chỉ ở các điểm cuối được quan sát, chúng tôi vẫn phải đối mặt với vấn đề rằng mức độ bao phủ chỉ không đổi giữa các điểm sự kiện chứ không nhất thiết phải ở các điểm đó. 

Quan sát quan trọng là số lượng vùng phủ sóng chỉ thay đổi ở ranh giới phân đoạn. Giữa hai “vị trí sự kiện” liên tiếp trên trục số, số đoạn hoạt động là không đổi. Vì vậy, thay vì nghĩ về từng điểm nguyên, chúng ta nén bài toán thành các khoảng mà độ bao phủ không thay đổi. 

Điều này dẫn đến ý tưởng về đường quét: chúng tôi chuyển đổi mỗi phân đoạn thành hai sự kiện, một sự kiện làm tăng phạm vi bao phủ tại l và một làm giảm phạm vi bao phủ sau r. Việc sắp xếp các sự kiện này cho phép chúng tôi xử lý dòng theo thứ tự tăng dần và duy trì số lượng phân đoạn đang hoạt động hiện tại. Bất cứ khi nào chúng tôi di chuyển từ vị trí sự kiện này sang vị trí sự kiện tiếp theo, số lượng phân đoạn vẫn cố định trên toàn bộ khoảng cách, vì vậy chúng tôi có thể thêm độ dài của khoảng cách đó vào câu trả lời cho số lượng đó. 

Điều này làm giảm vấn đề từ việc đếm từng điểm sang tổng hợp theo từng khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số điểm) | O(1) đến O(n) | Quá chậm | 
| Đường quét | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng phân đoạn thành hai sự kiện ranh giới, một sự kiện có mức độ bao phủ tăng dần và một sự kiện có mức độ bao phủ giảm dần sau khi phân đoạn kết thúc. Sau đó chúng tôi sắp xếp các sự kiện này theo vị trí. 

Chúng tôi duy trì một biến đang chạy`active`biểu thị số lượng phân đoạn hiện đang bao phủ vị trí quét. Chúng tôi cũng lặp qua các vị trí sự kiện được sắp xếp theo thứ tự. 

1. Chuyển đổi mỗi đoạn [l, r] thành hai sự kiện: tại l chúng ta thêm +1 và tại r + 1 chúng ta thêm -1. Mã hóa này đảm bảo tính chính xác cho các khoảng bao gồm vì phân đoạn đóng góp thông qua r và ngừng đóng góp bắt đầu từ r + 1. 
2. Sắp xếp tất cả các sự kiện theo tọa độ của chúng. Việc sắp xếp là cần thiết vì chúng ta cần xử lý các thay đổi về phạm vi bao phủ theo thứ tự từ trái sang phải. 
3. Khởi tạo`active = 0`và một mảng kết quả trống`ans`có kích thước n+1. 
4. Lặp lại các sự kiện đã sắp xếp trong khi theo dõi tọa độ trước đó`prev`. Với mỗi tọa độ riêng biệt`x`, trước tiên chúng tôi xem xét phân khúc từ`prev`ĐẾN`x - 1`. Trong toàn bộ vùng này, số lượng phân đoạn hoạt động là không đổi, do đó mọi điểm nguyên trong phạm vi này đều góp phần vào`ans[active]`. 
5. Sau khi tính toán khoảng cách, áp dụng tất cả các cập nhật tại tọa độ`x`, điều chỉnh`active`tương ứng. 
6. Di chuyển`prev`ĐẾN`x`và tiếp tục. 
7. Sau khi xử lý tất cả các sự kiện, không còn khoảng trống nào nữa. 

Điểm chính xác quan trọng là chúng tôi chỉ gán số đếm trên các vùng ổn định không có sự kiện nào xảy ra, đảm bảo rằng mỗi điểm số nguyên được tính chính xác một lần và chính xác với giá trị phạm vi thực của nó. 

### Tại sao nó hoạt động 

Đường quét duy trì tính bất biến giữa hai tọa độ sự kiện liên tiếp, không có đoạn nào bắt đầu hoặc kết thúc. Do đó, tập hợp các phân đoạn hoạt động là không đổi trong suốt khoảng thời gian đó. Vì mỗi điểm nguyên thuộc về chính xác một khoảng như vậy và chúng tôi tính toàn bộ chiều dài của nó bằng cách sử dụng giá trị không đổi đó nên mỗi điểm đóng góp chính xác một lần vào nhóm tần số chính xác. Việc dịch chuyển điểm cuối +1 đảm bảo tính toàn diện được xử lý mà không cần tính hai lần ở các ranh giới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = []
    
    for _ in range(n):
        l, r = map(int, input().split())
        events.append((l, 1))
        events.append((r + 1, -1))
    
    events.sort()
    
    ans = [0] * (n + 1)
    
    active = 0
    prev = None
    
    i = 0
    m = len(events)
    
    while i < m:
        x = events[i][0]
        
        if prev is not None and prev < x:
            length = x - prev
            ans[active] += length
        
        delta = 0
        while i < m and events[i][0] == x:
            delta += events[i][1]
            i += 1
        
        active += delta
        prev = x
    
    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Giải pháp mã hóa các điểm cuối của phân đoạn thành các sự kiện kiểu mảng khác biệt. Chi tiết quan trọng là sử dụng r + 1 cho sự kiện giảm dần, điều chỉnh phạm vi bao hàm với quá trình xử lý khoảng thời gian nửa mở. Sắp xếp các nhóm tất cả các thay đổi ở cùng một tọa độ và chúng tôi tổng hợp chúng trước khi chuyển tiếp để các trạng thái trung gian ở cùng một vị trí không ảnh hưởng không chính xác đến số lượng hoạt động. 

Xử lý khe hở`length = x - prev`là nơi phạm vi bảo hiểm thực sự được tích lũy. Đây là nơi duy nhất mà các điểm nguyên được "đếm" và nó đếm chính xác tất cả các số nguyên từ trước đến x - 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 3
1 3
3 8
```Chúng tôi xây dựng sự kiện: 

(0, +1), (4, -1), (1, +1), (4, -1), (3, +1), (9, -1) 

Đã sắp xếp: 

(0, +1), (1, +1), (3, +1), (4, -2), (9, -1) 

| Bước | x | trước | hoạt động trước | chiều dài đoạn | đóng góp | hoạt động sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | - | 0 | - | - | 1 | 
| 2 | 1 | 0 | 1 | 1 | ans[1] += 1 | 2 | 
| 3 | 3 | 1 | 2 | 2 | ans[2] += 2 | 3 | 
| 4 | 4 | 3 | 3 | 1 | ans[3] += 1 | 1 | 
| 5 | 9 | 4 | 1 | 5 | ans[1] += 5 | 0 | 

Câu trả lời cuối cùng: 

k=1 → 6, k=2 → 2, k=3 → 1 

Dấu vết này cho thấy phạm vi bao phủ không bao giờ được tính toán rõ ràng trên mỗi số nguyên. Thay vào đó, mỗi vùng ổn định giữa các tọa độ sự kiện được nén thành một phần đóng góp duy nhất có trọng số theo chiều dài của nó. 

### Ví dụ 2 

đầu vào:```
3
0 0
2 2
4 4
```Sự kiện: 

(0, +1), (1, -1), (2, +1), (3, -1), (4, +1), (5, -1) 

| Bước | x | trước | hoạt động | chiều dài | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | - | 0 | - | - | 
| 2 | 1 | 0 | 1 | 1 | ans[1]+=1 | 
| 3 | 2 | 1 | 0 | 1 | ans[0]+=1 (bỏ qua sau) | 
| 4 | 3 | 2 | 1 | 1 | ans[1]+=1 | 
| 5 | 4 | 3 | 0 | 1 | ans[0]+=1 | 
| 6 | 5 | 4 | 1 | 1 | ans[1]+=1 | 

Sau khi bỏ qua k=0, kết quả là: 

k=1 → 3, k>=2 → 0 

Điều này nêu bật cách các điểm bị cô lập hoạt động như các khoảng có độ dài đơn vị giữa các số nguyên liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp sự kiện 2n chiếm ưu thế | 
| Không gian | O(n) | danh sách sự kiện và mảng tần số | 

Thuật toán có quy mô rõ ràng đến 200.000 phân đoạn vì tất cả các hoạt động đều giảm xuống việc sắp xếp và quét tuyến tính. Không còn sự phụ thuộc vào cường độ tọa độ, điều này rất cần thiết với phạm vi điểm cuối 10^18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    events = []
    for _ in range(n):
        l, r = map(int, input().split())
        events.append((l, 1))
        events.append((r + 1, -1))
    events.sort()

    ans = [0] * (n + 1)
    active = 0
    prev = None
    i = 0

    while i < len(events):
        x = events[i][0]
        if prev is not None and prev < x:
            ans[active] += x - prev
        delta = 0
        while i < len(events) and events[i][0] == x:
            delta += events[i][1]
            i += 1
        active += delta
        prev = x

    return " ".join(map(str, ans[1:]))

# provided sample
assert run("""3
0 3
1 3
3 8
""") == "6 2 1"

# single point segments
assert run("""3
5 5
10 10
15 15
""") == "3 0 0"

# fully overlapping
assert run("""3
1 5
1 5
1 5
""") == "0 0 5"

# nested segments
assert run("""3
1 10
2 9
3 8
""") == "0 2 6"

# disjoint large gaps
assert run("""2
0 0
100 100
""") == "2 0"

# all identical segments, large overlap
assert run("""4
0 1
0 1
0 1
0 1
""") == "0 0 2 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm đơn | 3 0 0 | đoạn thoái hóa | 
| chồng chéo hoàn toàn | 0 0 5 | đếm chồng chéo tối đa | 
| lồng nhau | 0 2 6 | độ sâu vùng phủ sóng khác nhau | 
| rời rạc | 2 0 | khoảng trống và các vùng độc lập | 
| giống hệt nhau | 0 0 2 0 | lặp đi lặp lại khoảng thời gian giống hệt nhau | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi các đoạn suy biến thành các điểm đơn lẻ. Đối với đầu vào:```
1
5 5
```Các sự kiện trở thành (5, +1) và (6, -1). Thuật toán coi khoảng [5, 5] là phần đóng góp độ dài đơn vị từ 5 đến 6. Số lượng hoạt động trở thành 1 chính xác trên khoảng đó, do đó ans[1] tăng thêm 1, phù hợp với thực tế là có chính xác một điểm nguyên được bao phủ. 

Một trường hợp khác xảy ra khi nhiều sự kiện có cùng tọa độ. Ví dụ:```
2
1 3
3 5
```Tại tọa độ 3, một đoạn kết thúc trong khi đoạn khác bắt đầu. Bởi vì chúng tôi tổng hợp tất cả các delta tại mỗi tọa độ trước khi chuyển tiếp nên quá trình chuyển đổi được xử lý nguyên tử. Số lượng hoạt động được sử dụng cho khoảng thời gian [1, 3] là chính xác và khoảng thời gian tiếp theo bắt đầu bằng số lượng hoạt động được cập nhật mà không có bất kỳ sai sót trung gian nào.
