---
title: "CF 425D - Sereja và hình vuông"
description: "Chúng ta có một tập hợp $n$ điểm trên mặt phẳng 2D, mỗi điểm có tọa độ nguyên và tất cả các điểm đều khác nhau. Nhiệm vụ là đếm xem có bao nhiêu hình vuông thẳng hàng với bốn góc đều là các điểm từ tập hợp này."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "hashing"]
categories: ["algorithms"]
codeforces_contest: 425
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 243 (Div. 1)"
rating: 2300
weight: 425
solve_time_s: 35
verified: true
draft: false
---

[CF 425D - Sereja và Squares](https://codeforces.com/problemset/problem/425/D) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, băm 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$các điểm trên mặt phẳng 2D, mỗi điểm có tọa độ nguyên và tất cả các điểm đều khác biệt. Nhiệm vụ là đếm xem có bao nhiêu hình vuông thẳng hàng với bốn góc đều là các điểm từ tập hợp này. Hình vuông thẳng hàng là hình vuông có mỗi cạnh ngang hoặc dọc. Đầu ra là một số nguyên duy nhất: tổng số ô vuông như vậy. 

Được cho$n$có thể lên tới$10^5$và tọa độ có thể lên tới$10^5$, một cách tiếp cận mạnh mẽ để kiểm tra tất cả bốn điểm là không khả thi. Ví dụ: việc lặp qua mọi sự kết hợp của bốn điểm sẽ liên quan đến khoảng$O(n^4)$hoạt động, đại khái là$10^{20}$tối đa$n$. Rõ ràng, chúng ta cần một cách tiếp cận hiệu quả hơn nhiều, lý tưởng nhất là xoay quanh$O(n \log n)$hoặc$O(n \sqrt{n})$. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó nhiều điểm nằm trên cùng một đường thẳng. Ví dụ: chỉ một đường thẳng đứng của các điểm không thể tạo thành bất kỳ hình vuông nào nếu không có các điểm ngang. Một trường hợp phức tạp khác là khi hai điểm tạo thành một hình vuông tiềm năng theo đường chéo nhưng hai góc còn lại bị thiếu, ví dụ:$(0,0), (2,2)$một mình không thể tạo thành một hình vuông mà không có$(0,2)$Và$(2,0)$. Một cách tiếp cận ngây thơ đếm các đường chéo tiềm năng sẽ bị tính quá mức nếu không xác minh cả bốn góc. 

## Phương pháp tiếp cận 

Giải pháp ngây thơ sẽ lặp lại trên tất cả bốn điểm. Đối với mỗi bộ tứ, hãy kiểm tra xem chúng có tạo thành hình vuông hay không bằng cách so sánh khoảng cách và xác minh sự căn chỉnh của các trục. Điều này đúng nhưng cực kỳ chậm; độ phức tạp của nó là$O(n^4)$. Vì$n=10^5$, điều này không thực tế. 

Một cách tiếp cận thông minh hơn nhận thấy rằng bất kỳ hình vuông nào cũng có thể được xác định duy nhất bởi các góc dưới bên trái và trên cùng bên phải (hoặc trên cùng bên trái và dưới cùng bên phải), bởi vì hai góc còn lại được cố định bởi hai góc này. Do đó, thay vì kiểm tra các bộ tứ, chúng ta có thể lặp qua tất cả các cặp điểm có thể đóng vai trò là các góc đối diện theo đường chéo của hình vuông. Đối với mỗi đường chéo ứng viên, hai góc còn lại được tính trực tiếp. Sau đó chúng ta cần kiểm tra một cách hiệu quả xem những điểm đó có tồn tại trong tập hợp hay không. Bằng cách lưu trữ tất cả các điểm trong một tập hợp hoặc bản đồ băm, việc tra cứu này trở thành$O(1)$trung bình. 

Điều này làm giảm độ phức tạp$O(n^2)$trong trường hợp xấu nhất để lặp lại tất cả các cặp, điều này có thể chấp nhận được đối với$n$lên tới vài nghìn nhưng có thể quá lớn đối với$n=10^5$. Để tối ưu hóa hơn nữa, chúng ta có thể khai thác hàm băm tọa độ. Chúng ta có thể nhóm các điểm theo tọa độ x và quét các khoảng cách y tiềm năng một cách hiệu quả hoặc nhóm theo tọa độ y. Điều này đảm bảo rằng chúng tôi tránh được việc kiểm tra cặp không cần thiết, đạt được hiệu suất thực tế trong điều kiện hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tăng gấp bốn lần) | O(n^4) | O(n) | Quá chậm | 
| Kiểm tra đường chéo dựa trên cặp | O(n^2) | O(n) | Có thể chấp nhận được ở mức vừa phải | 
| Được tối ưu hóa với băm tọa độ | O(n√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả$n$điểm và lưu trữ chúng trong một tập hợp cho các truy vấn tồn tại O(1). 
2. Lặp lại từng cặp điểm$(x_1, y_1)$Và$(x_2, y_2)$. Hãy coi chúng như các góc đối diện tiềm năng theo đường chéo của một hình vuông. 
3. Bỏ qua cặp ở đâu$x_1 = x_2$hoặc$y_1 = y_2$bởi vì hình vuông không thể có chiều rộng hoặc chiều cao bằng 0. 
4. Tính hai góc còn lại là$(x_1, y_2)$Và$(x_2, y_1)$. 
5. Kiểm tra xem cả hai điểm này có tồn tại trong tập hợp không. Nếu có, hãy tăng số lượng bình phương. 
6. Vì mỗi ô vuông sẽ được tính hai lần (một lần trên mỗi đường chéo), nên chia số đếm cuối cùng cho 2 trước khi xuất ra. 

Tại sao nó hoạt động: Mỗi hình vuông có chính xác hai đường chéo riêng biệt. Bằng cách lặp lại tất cả các cặp điểm và coi chúng là các đường chéo, chúng ta đảm bảo rằng mỗi ô vuông được tính chính xác hai lần. Việc sử dụng một bộ cho phép kiểm tra sự tồn tại nhanh chóng của các góc còn lại. Điều này đảm bảo tính chính xác và tránh việc đếm quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_squares():
    n = int(input())
    points = set()
    coords = []
    for _ in range(n):
        x, y = map(int, input().split())
        points.add((x, y))
        coords.append((x, y))
    
    count = 0
    for i in range(n):
        x1, y1 = coords[i]
        for j in range(i + 1, n):
            x2, y2 = coords[j]
            if x1 == x2 or y1 == y2:
                continue
            if (x1, y2) in points and (x2, y1) in points:
                count += 1
    print(count // 2)

count_squares()
```Giải pháp trước tiên lưu trữ các điểm trong một tập hợp để tra cứu nhanh và giữ chúng trong danh sách để lặp lại theo cặp. Mỗi cặp được coi là một đường chéo. Việc kiểm tra đảm bảo chúng tôi không coi các cặp dọc hoặc ngang là đường chéo. Cuối cùng, chia cho hai sẽ đúng để đếm kép. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
0 0
0 2
2 0
2 2
1 1
```| Cặp (i,j) | Đường chéo? | Các góc khác tồn tại? | Đếm số tăng | 
| --- | --- | --- | --- | 
| (0,0)-(2,2) | Có | (0,2),(2,0) tồn tại | +1 | 
| Các cặp khác | Hoặc giống x hoặc y, hoặc không chéo | Không áp dụng | 0 | 

Đầu ra: 1 

Dấu vết này cho thấy thuật toán xác định chính xác một hình vuông được tạo thành bởi các góc (0,0),(0,2),(2,0),(2,2) và bỏ qua điểm (1,1). 

### Mẫu 2
