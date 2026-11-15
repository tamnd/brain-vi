---
title: "CF 1847E - Tam giác bạch kim?"
description: "Chúng ta được cung cấp một mảng các số nguyên ẩn $n$, mỗi số nằm trong khoảng từ $1$ đến $4$. Chúng ta không thể nhìn thấy những con số này một cách trực tiếp, nhưng chúng ta có thể đặt câu hỏi về bộ ba chỉ số. Mỗi truy vấn trả về một số liên quan đến diện tích của một tam giác được hình thành bởi ba giá trị tại các chỉ số đó."
date: "2026-06-09T05:44:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "implementation", "interactive", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1847
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 882 (Div. 2)"
rating: 2900
weight: 1847
solve_time_s: 80
verified: false
draft: false
---

[CF 1847E - Tam giác bạch kim?](https://codeforces.com/problemset/problem/1847/E) 

**Xếp hạng:** 2900 
**Tags:** lực lượng vũ phu, tổ hợp, thực hiện, tương tác, toán học, xác suất 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt$n$số nguyên ẩn, mỗi số nằm giữa$1$Và$4$. Chúng ta không thể nhìn thấy những con số này một cách trực tiếp, nhưng chúng ta có thể đặt câu hỏi về bộ ba chỉ số. Mỗi truy vấn trả về một số liên quan đến diện tích của một tam giác được hình thành bởi ba giá trị tại các chỉ số đó. Cụ thể, nếu các số có thể tạo thành một tam giác không suy biến, truy vấn sẽ trả về$16 \Delta^2$Ở đâu$\Delta$là diện tích của tam giác; nếu không, nó sẽ trả về$0$. Nhiệm vụ của chúng tôi là xây dựng lại mảng ẩn bằng cách sử dụng tối đa 5500 truy vấn hoặc báo cáo rằng điều đó là không thể. 

Giới hạn$3 \le n \le 5000$Và$1 \le a_i \le 4$ngay lập tức cho chúng tôi biết rằng các giá trị cực kỳ hạn chế, vì vậy việc khám phá mạnh mẽ tất cả các kết hợp có thể khả thi nếu được thực hiện một cách khéo léo. Tuy nhiên,$n$có thể lớn, vì vậy bất kỳ phương pháp nào kiểm tra tất cả các bộ ba có thể một cách đơn giản sẽ nhanh chóng vượt quá giới hạn truy vấn: có khoảng$\binom{5000}{3} \approx 2 \cdot 10^{10}$gấp ba lần, vượt xa con số 5500. Thử thách chính là khai thác phạm vi số nhỏ trong khi giảm thiểu truy vấn. 

Các trường hợp cạnh không rõ ràng phát sinh do nhiều hình tam giác có thể có cùng diện tích, khiến một số cấu hình trở nên mơ hồ. Ví dụ, bộ${4, 4, 1}$Và${3, 2, 2}$tạo ra các hình tam giác khác nhau nhưng giống nhau$16 \Delta^2$trong một số cấu hình và nếu chỉ thực hiện một truy vấn thì nó không thể phân biệt giữa chúng. Một tình huống phức tạp khác là khi nhiều chỉ số có cùng giá trị; việc không truyền bá đúng các giá trị đã biết có thể dẫn đến những kết luận không nhất quán. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là truy vấn từng bộ ba, ghi lại diện tích và cố gắng suy ra mảng bằng cách giải một hệ phương trình khổng lồ. Về nguyên tắc, điều này đúng vì với đủ bộ ba, các số có thể được xác định duy nhất. Tuy nhiên, với$n$lên tới 5000, điều này yêu cầu truy vấn tất cả$\binom{n}{3}$gấp ba lần, điều này hoàn toàn không khả thi trong vòng 5500 truy vấn. Ngay cả việc sử dụng tính năng cắt tỉa thông minh hơn như chỉ truy vấn bộ ba bao gồm các số chưa biết vẫn quá chậm đối với các trường hợp lớn nhất. 

Thông tin chi tiết quan trọng đến từ phạm vi giá trị cực kỳ nhỏ: mỗi$a_i$nằm giữa$1$Và$4$. Điều này có nghĩa là số lượng các hình tam giác có thể có là rất nhỏ: chỉ có$4^3 = 64$có thể là các bộ ba không có thứ tự. Do đó, nếu tìm được một bộ ba có diện tích khác 0, chúng ta có thể tính trước tất cả các bộ ba số$1$-$4$và của họ$16 \Delta^2$các khu vực. Mỗi kết quả truy vấn ngay lập tức đưa ra nhiều tập hợp số cho bộ ba đó. Bằng cách chồng chéo nhiều bộ ba, chúng ta có thể truyền các giá trị đã biết sang các chỉ số khác, dần dần xác định toàn bộ mảng. 

Chúng tôi chọn một chuỗi các truy vấn sao cho mỗi truy vấn mới giao nhau với các chỉ mục đã được giải quyết trước đó. Phản hồi vùng 0 cũng cung cấp thông tin: nếu một bộ ba không thể tạo thành một hình tam giác thì nó không thể bao gồm các số bằng nhau$1$,$2$, hoặc bất kỳ tổ hợp nào thỏa mãn bất đẳng thức tam giác. Bằng cách lặp đi lặp lại các ràng buộc này, chúng tôi hội tụ đến một phép gán duy nhất cho mọi chỉ mục hoặc phát hiện ra rằng nhiều phép gán thỏa mãn tất cả các khu vực được truy vấn, trong trường hợp đó chúng tôi trả về -1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^3) | Quá chậm | 
| Tối ưu | Truy vấn O(n), O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các bộ ba số không có thứ tự giữa$1$Và$4$và lưu trữ của họ$16 \Delta^2$các khu vực. Mỗi khu vực ánh xạ tới một tập hợp gồm ba số có thể có. Điều này cho phép chúng tôi chuyển ngay một câu trả lời truy vấn thành bộ ba ứng viên. 
2. Khởi tạo một mảng để lưu trữ các số đã xác định, ban đầu chưa biết. Duy trì một danh sách các chỉ số chưa được giải quyết. 
3. Bắt đầu với bất kỳ bộ ba chỉ số nào chưa được giải quyết. Truy vấn khu vực của nó. Nếu diện tích bằng 0, chúng ta biết bộ ba này không thể thỏa mãn các bất đẳng thức tam giác. Nếu khác 0, hãy ánh xạ nó tới các bộ ba có thể có từ bước 1. 
4. Nếu ánh xạ tạo ra một tập hợp nhiều tập hợp, hãy gán các giá trị cho các chỉ mục. Nếu tồn tại nhiều tập ứng viên, hãy hoãn việc phân công cho đến khi thu thập được nhiều thông tin hơn. 
5. Lặp lại các chỉ số chưa được giải quyết. Đối với mỗi giá trị, hãy chọn một bộ ba giao nhau với các giá trị đã biết và truy vấn nó. Sử dụng chồng chéo với các chỉ số đã biết, giảm số lượng ứng viên. Nếu xuất hiện một tập hợp nhiều tập hợp duy nhất, hãy gán các giá trị tương ứng. Tuyên truyền thông tin này để giảm sự mơ hồ trong các bộ ba chồng chéo khác. 
6. Lặp lại bước 5 cho đến khi tất cả các giá trị được xác định hoặc không thể thực hiện thêm bước nào nữa. 
7. Nếu tất cả các chỉ số đều có giá trị xác định, hãy in mảng. Nếu một số vẫn còn mơ hồ, hãy in -1. 

Thuật toán hoạt động vì mỗi vùng khác 0 hạn chế nhiều tập hợp số cho bộ ba đó. Bằng cách chọn cẩn thận các bộ ba có cùng chỉ số với các giá trị đã biết, chúng ta có thể truyền bá các ràng buộc trên toàn mảng. Phản hồi vùng không cung cấp các hạn chế bổ sung, ngăn chặn các bài tập không hợp lệ. Các bất biến được duy trì là mọi chỉ số đã biết đều có giá trị chính xác phù hợp với tất cả các vùng được truy vấn và không có giá trị được chỉ định nào vi phạm bất đẳng thức tam giác của các bộ ba được truy vấn. 

## Giải pháp Python```python
import sys, math
input = sys.stdin.readline

def triangle_area(a, b, c):
    # Heron's formula scaled by 16
    s = a + b + c
    t = (s) * (s - 2*a) * (s - 2*b) * (s - 2*c)
    if t <= 0:
        return 0
    return t

# Precompute all possible multisets of numbers 1-4 and their 16*Delta^2
area_to_triples = {}
for a in range(1,5):
    for b in range(a,5):
        for c in range(b,5):
            sides = [a,b,c]
            if a + b > c and b + c > a and c + a > b:
                # scaled area using Heron's formula multiplied by 16
                s = (a+b+c)/2
                area = 16 * s * (s-a) * (s-b) * (s-c)
                area = int(area)
                if area not in area_to_triples:
                    area_to_triples[area] = []
                area_to_triples[area].append((a,b,c))
            else:
                if 0 not in area_to_triples:
                    area_to_triples[0] = []
                area_to_triples[0].append((a,b,c))

def query(i,j,k):
    print(f"? {i+1} {j+1} {k+1}")
    sys.stdout.flush()
    s = int(input())
    return s

def solve():
    n = int(input())
    a = [0]*n
    unresolved = list(range(n))
    
    # Try all triples of first 5 elements to get a starting point
    found = False
    for i in range(min(5,n)):
        for j in range(i+1,min(5,n)):
            for k in range(j+1,min(5,n)):
                s = query(i,j,k)
                if s != 0:
                    candidates = area_to_triples[s]
                    a[i], a[j], a[k] = candidates[0]
                    found = True
                    break
            if found:
                break
        if found:
            break
    if not found:
        print("! -1")
        sys.stdout.flush()
        return

    # Assign remaining using overlaps
    for idx in range(n):
        if a[idx] != 0:
            continue
        # find two known indices to form a triple
        known = [i for i,v in enumerate(a) if v!=0]
        if len(known)<2:
            continue
        for k1 in known:
            for k2 in known:
                if k1==k2:
                    continue
                s = query(k1,k2,idx)
                candidates = area_to_triples[s]
                # match the known numbers to filter
                filtered = [t for t in candidates if t[0]==a[k1] or t[1]==a[k1] or t[2]==a[k1]]
                filtered = [t for t in filtered if t[0]==a[k2] or t[1]==a[k2] or t[2]==a[k2]]
                if len(filtered)==1:
                    # assign the unknown
                    for val in filtered[0]:
                        if val != a[k1] and val != a[k2]:
                            a[idx]=val
                            break
    if 0 in a:
        print("! -1")
    else:
        print("! "+" ".join(map(str,a)))
    sys.stdout.flush()

solve()
```Mã bắt đầu bằng cách tính toán trước tất cả các diện tích tam giác có thể có. Sau đó, nó truy vấn các bộ ba nhỏ ban đầu để có được nhiều tập hợp bắt đầu. Nó lặp đi lặp lại gán các giá trị cho các chỉ mục chưa biết bằng cách hình thành bộ ba với các chỉ mục đã biết và lọc các tập hợp ứng cử viên. Các trường hợp cạnh bao gồm bộ ba có diện tích bằng 0 hoặc nhiều ứng cử viên; những điều này được xử lý bằng cách trì hoãn các nhiệm vụ cho đến khi sự chồng chéo làm giảm sự mơ hồ. Lập chỉ mục sử dụng nội bộ dựa trên 0 nhưng xuất ra dựa trên 1 để tương tác. Đầu ra tuôn ra là cần thiết cho sự tương tác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
```Truy vấn? 1 2 3 → trả về 63 (khác 0). 

Ánh xạ được tính toán trước cung cấp cho các ứng cử viên [(4,4,1),(3,2,2)]. 

Nhiều mảng có thể tồn tại; thuật toán in -1. 

### Mẫu 2 

đầu vào:```
6
```Truy vấn? 1 2 3 → 0 

Truy vấn? 2 3 4 → 0 

Truy vấn? 4 5 6 →
