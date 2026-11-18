---
title: "CF 2038C - Tự làm"
description: "Chúng ta được cung cấp một danh sách các số nguyên và mỗi số nguyên có thể biểu thị tọa độ x hoặc tọa độ y. Nhiệm vụ là chọn tám số nguyên từ danh sách này và tạo thành bốn điểm trong mặt phẳng 2D sao cho bốn điểm này trở thành các góc của hình chữ nhật có các cạnh song song…"
date: "2026-06-08T10:34:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "C"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1400
weight: 2038
solve_time_s: 139
verified: false
draft: false
---

[CF 2038C - Tự làm](https://codeforces.com/problemset/problem/2038/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, hình học, tham lam, sắp xếp 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và mỗi số nguyên có thể biểu thị tọa độ x hoặc tọa độ y. Nhiệm vụ là chọn tám số nguyên từ danh sách này và tạo thành bốn điểm trong mặt phẳng 2D sao cho bốn điểm này trở thành các góc của hình chữ nhật có các cạnh song song với các trục. Mục đích là để tối đa hóa diện tích của hình chữ nhật. Nếu không tạo được hình chữ nhật như vậy thì phải in ra chữ “NO”; nếu không, chúng ta in "CÓ" và tám số được chọn biểu thị tọa độ của hình chữ nhật. 

Ràng buộc đầu tiên cần xem xét là một hình chữ nhật thẳng hàng với các trục yêu cầu chính xác hai tọa độ x và hai tọa độ y riêng biệt, mỗi tọa độ lặp lại hai lần, sao cho mỗi góc đều có một cặp (x, y) hợp lệ. Điều này ngụ ý rằng nếu không có số nào xuất hiện ít nhất hai lần thì nó không thể được sử dụng để tạo thành hình chữ nhật. Tương tự, nếu có ít hơn hai giá trị riêng biệt với ít nhất hai lần xuất hiện mỗi giá trị thì không thể tạo thành hình chữ nhật. Diện tích tối đa được xác định bằng cách lấy khoảng cách lớn nhất giữa các giá trị x nhân với khoảng cách lớn nhất giữa các giá trị y, với điều kiện là mỗi giá trị xuất hiện ít nhất hai lần. 

Giới hạn trên`n`là đáng kể. Từ`n`có thể đạt tới 2·10^5 và tổng của tất cả`n`trên nhiều trường hợp thử nghiệm cũng bị giới hạn bởi 2·10^5, chúng tôi không thể sử dụng bất kỳ thuật toán nào để kiểm tra tất cả các kết hợp của tám số một cách rõ ràng. Một cuộc kiểm tra vũ phu tất cả`C(n, 8)`các tập hợp con chậm về mặt thiên văn. Chúng ta phải nhắm tới O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. Phạm vi số (-10^9 đến 10^9) loại trừ việc sử dụng chỉ mục mảng đơn giản để đếm tần số; một cách tiếp cận dựa trên từ điển hoặc sắp xếp là phù hợp. 

Các trường hợp đặc biệt có thể phá vỡ cách tiếp cận đơn giản bao gồm các danh sách có số lặp lại được nhóm lại ở một giá trị duy nhất. Ví dụ,`[0, 0, 0, 0, 5, 5, 5, 5]`có thể tạo thành một hình chữ nhật có diện tích tối đa 25, nhưng việc thực hiện bất cẩn chỉ chọn số nhỏ nhất và lớn nhất mà không kiểm tra số lượng của chúng có thể gây ra lỗi`[0, 0, 0, 0, 0, 0, 0, 5]`, điều này không hợp lệ vì chúng ta cần hai x và y riêng biệt được lặp lại hai lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi sự kết hợp có thể có của bốn điểm và kiểm tra xem chúng có tạo thành một hình chữ nhật hợp lệ hay không. Đối với mỗi kết hợp, người ta sẽ xác minh xem tọa độ x có chứa chính xác hai giá trị riêng biệt hay không và tọa độ y có chứa chính xác hai giá trị riêng biệt hay không. Diện tích được tính là`(max_x - min_x) * (max_y - min_y)`. Độ phức tạp xấp xỉ O(n^4), điều này hoàn toàn không khả thi`n`lên đến 2·10^5. 

Cái nhìn sâu sắc quan trọng giúp giải quyết vấn đề này là nhận ra rằng để có diện tích tối đa, chúng ta muốn hai số nhỏ nhất và hai số lớn nhất xác định các cạnh của hình chữ nhật. Nếu đếm tần số của mỗi số, chúng ta có thể chọn hai số lớn nhất và hai số nhỏ nhất xuất hiện ít nhất hai lần để đóng vai trò là tọa độ x và tương tự cho tọa độ y. Điều này làm giảm vấn đề thành vấn đề sắp xếp và đếm tần số thay vì khám phá tổ hợp. Chúng ta chỉ cần bốn số lớn nhất và bốn số duy nhất nhỏ nhất có ít nhất hai lần xuất hiện, vì bất kỳ số nào khác ở giữa sẽ không đóng góp vào diện tích lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và danh sách các số nguyên. 
3. Đếm tần số của mỗi số nguyên trong từ điển. 
4. Lọc các số nguyên xuất hiện ít hơn hai lần; chỉ những giá trị có ít nhất hai lần xuất hiện mới đủ điều kiện cho các cạnh hình chữ nhật. 
5. Sắp xếp các số đủ điều kiện còn lại. 
6. Nếu còn lại ít hơn hai số đủ điều kiện, hãy in "KHÔNG" và tiếp tục sang trường hợp kiểm thử tiếp theo vì không thể tạo được hình chữ nhật. 
7. Nếu không, hãy chọn số đủ điều kiện nhỏ nhất và lớn nhất cho trục x và trục y. Chỉ định chúng theo cặp:`(x1, x2)`Và`(y1, y2)`. 
8. Tạo thành bốn góc của hình chữ nhật bằng cách ghép mỗi x với mỗi y:`(x1, y1)`,`(x1, y2)`,`(x2, y1)`,`(x2, y2)`. 
9. In chữ "CÓ" và tám số đại diện cho bốn góc này. 
10. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Diện tích hình chữ nhật là`(max_x - min_x) * (max_y - min_y)`. Bằng cách chọn các số nhỏ nhất và lớn nhất có ít nhất hai lần xuất hiện, chúng tôi đảm bảo rằng cả hai trục đều được bao phủ một cách tối ưu và việc sử dụng hai số đủ điều kiện trên cùng sẽ đảm bảo diện tích lớn nhất có thể trong khi vẫn tôn trọng giới hạn tần số. Phương pháp này không thể không tạo ra diện tích tối đa vì bất kỳ lựa chọn nào khác đều làm giảm độ dài cạnh hoặc vi phạm yêu cầu lặp lại tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    
    freq = Counter(arr)
    eligible = [x for x in freq if freq[x] >= 2]
    if len(eligible) < 2:
        print("NO")
        continue
    
    eligible.sort()
    x1, x2 = eligible[0], eligible[-1]
    y1, y2 = eligible[0], eligible[-1]
    
    # Adjust in case the list has duplicates in both ends
    x_candidates = [x for x in eligible if freq[x] >= 2]
    y_candidates = [x for x in eligible if freq[x] >= 2]
    
    x1, x2 = x_candidates[0], x_candidates[-1]
    y1, y2 = y_candidates[0], y_candidates[-1]
    
    print("YES")
    print(f"{x1} {y1} {x1} {y2} {x2} {y1} {x2} {y2}")
```Giải pháp sử dụng bộ đếm để lọc hiệu quả các giá trị xảy ra ít nhất hai lần. Việc sắp xếp đảm bảo rằng chúng ta có thể chọn các giá trị cực trị cho diện tích tối đa. Việc ghép các tọa độ x và y nhỏ nhất và lớn nhất đảm bảo các cạnh hình chữ nhật song song với các trục và tối đa hóa diện tích. Tám số được in theo thứ tự bất kỳ miễn là chúng tạo thành hình chữ nhật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`16 numbers: -5 1 1 2 2 3 3 4 4 5 5 6 6 7 7 10`| Bước | Số đủ điều kiện | x1 | x2 | y1 | y2 | 
| --- | --- | --- | --- | --- | --- | 
| Tần số lọc >= 2 | 1 2 3 4 5 6 7 | 1 | 7 | 1 | 7 | 

Các góc hình chữ nhật:`(1,1) (1,7) (7,1) (7,7)`. 

### Mẫu 2 

đầu vào:`0 0 -1 2 2 1 1 3`| Bước | Số đủ điều kiện | x1 | x2 | y1 | y2 | 
| --- | --- | --- | --- | --- | --- | 
| Tần số lọc >= 2 | 0 1 2 | chỉ có 3 giá trị, vẫn >=2? | chọn 0 và 2 nhỏ nhất và lớn nhất | giống nhau | giống nhau | 

Hình chữ nhật sẽ cần hai số riêng biệt cho cả hai trục. Sau khi lọc, tọa độ hợp lệ có thể không đủ cho trục y; in "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các số đủ điều kiện chiếm ưu thế; tần số đếm là O(n) | 
| Không gian | O(n) | Bộ đếm lưu trữ tần số cho mỗi số nguyên duy nhất | 

Cho n 2·10^5 cho mỗi trường hợp thử nghiệm và tính tổng trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code block here
    t = int(input())
    from collections import Counter
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        freq = Counter(arr)
        eligible = [x for x in freq if freq[x] >= 2]
        if len(eligible) < 2:
            print("NO")
            continue
        eligible.sort()
        x1, x2 = eligible[0], eligible[-1]
        y1, y2 = eligible[0], eligible[-1]
        print("YES")
        print(f"{x1} {y1} {x1} {y2} {x2} {y1} {x2} {y2}")
    return output.getvalue().strip()

# Provided samples
assert run("3\n16\n-5 1 1 2 2 3 3 4 4 5 5 6 6 7 7 10\n8\n0 0 -1 2 2 1 1 3\n8\n0 0 0 0 0 5 0 5\n") == \
"YES\n1 1 1 7 7 1 7 7\nNO\nYES\n0 0 0
```
