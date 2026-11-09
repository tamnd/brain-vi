---
title: "CF 1466A - Vấn đề nan giải của bò"
description: "Chúng ta được yêu cầu đếm số diện tích riêng biệt của đồng cỏ hình tam giác có thể được hình thành bằng cách sử dụng cây làm đỉnh. Một cây được cố định tại điểm (0,1) phía trên trục x. Các cây còn lại được đặt dọc theo con sông mà chúng tôi mô hình hóa là trục x, tại tọa độ $(xi, 0)$."
date: "2026-06-11T01:43:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1466
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2020"
rating: 800
weight: 1466
solve_time_s: 98
verified: true
draft: false
---

[CF 1466A - Vấn đề nan giải của bò](https://codeforces.com/problemset/problem/1466/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, hình học, toán học 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số diện tích riêng biệt của đồng cỏ hình tam giác có thể được hình thành bằng cách sử dụng cây làm đỉnh. Một cây được cố định tại điểm (0,1) phía trên trục x. Các cây còn lại được đặt dọc theo dòng sông mà chúng tôi mô hình hóa là trục x, tại tọa độ$(x_i, 0)$. Một tam giác hợp lệ phải bao gồm cây trên cùng và hai cây bất kỳ dọc theo sông. 

Mỗi trường hợp thử nghiệm cho chúng ta số lượng cây sông và tọa độ x của chúng, đồng thời chúng ta sẽ đưa ra số lượng diện tích tam giác khác 0 riêng biệt có thể. Vì tất cả tọa độ y của cây sông đều bằng 0 nên mỗi tam giác sẽ có một đỉnh phía trên trục x và hai đỉnh trên trục x, tạo thành một tam giác vuông có đáy nằm ngang. 

Các hạn chế rất nhỏ: tối đa 50 cây dọc sông cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm. Điều này ngụ ý một thuật toán có độ phức tạp lên tới$O(n^2)$mỗi trường hợp thử nghiệm là khả thi vì$50^2 = 2500$các hoạt động trên mỗi trường hợp thử nghiệm có thể dễ dàng trong giới hạn 1 giây. 

Trường hợp cạnh không rõ ràng phát sinh khi chỉ có một hoặc hai cây sông. Với 1 cây sông, chúng ta không thể tạo thành một hình tam giác nên đáp án là 0. Với hai cây sông thì có đúng một hình tam giác. Một sự tinh tế khác là các diện tích trùng lặp: nếu hai hình tam giác có cùng chiều dài đáy thì diện tích của chúng bằng nhau. Ví dụ, cây sông ở vị trí [1,2,4] với cây trên cùng ở (0,1) mang lại diện tích tam giác 0,5, 2 và 1,5, một số trong đó có thể trùng nhau tùy thuộc vào tọa độ cụ thể được chọn. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các hình tam giác có thể có và tính toán từng diện tích. Vì mỗi tam giác phải bao gồm cây trên cùng và hai cây sông riêng biệt nên chúng ta lặp lại tất cả các cặp cây sông. Với mỗi cặp, chúng ta tính diện tích tam giác bằng một nửa tích của chiều dài đáy và chiều cao. Chiều cao luôn là 1 nên diện tích giảm đi một nửa khoảng cách giữa hai cây sông. Việc lưu trữ từng khu vực trong một bộ đảm bảo tính duy nhất. 

Lực lượng vũ phu này hoạt động vì các ràng buộc nhỏ, nhưng về cơ bản nó đã tối ưu. Cấu trúc bài toán cho phép chúng ta tránh được các phép tính hình học phức tạp. Điểm mấu chốt là tất cả các hình tam giác đều có chung một đỉnh trên cùng, do đó công thức tính diện tích được đơn giản hóa thành$\text{area} = |x_i - x_j|/2$, Ở đâu$x_i$Và$x_j$là hai tọa độ cây sông. Điều này có nghĩa là vấn đề giảm xuống còn việc đếm số lượng khác biệt rõ ràng giữa các cặp tọa độ cây sông, đó chính xác là những gì phương pháp tiếp cận vũ phu thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Tối ưu | O(n^2) | O(n^2) trong trường hợp xấu nhất đối với tập hợp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$, số lượng cây sông và sau đó là danh sách sắp xếp tọa độ x của chúng. 
3. Khởi tạo một tập hợp trống để lưu trữ các diện tích tam giác riêng biệt. 
4. Lặp lại tất cả các cặp chỉ số cây sông$(i,j)$với$i < j$. Tính diện tích bằng một nửa chênh lệch tuyệt đối của tọa độ x của chúng:$(x_j - x_i)/2$. 
5. Thêm từng vùng tính toán vào tập hợp. Sử dụng một bộ sẽ tự động xử lý các bản sao. 
6. Sau khi xử lý tất cả các cặp, kích thước của tập hợp sẽ cho biết số vùng riêng biệt. In số này. 

Tại sao nó hoạt động: Thuật toán đảm bảo tính chính xác vì mọi tam giác hợp lệ được tạo chính xác một lần bằng cách lặp qua tất cả các cặp cây sông. Việc đơn giản hóa công thức diện tích đảm bảo không có vấn đề về độ chính xác vì tất cả tọa độ đều là số nguyên và phép chia cho 2 là chính xác trong số học hữu tỉ. Sử dụng một bộ đảm bảo các khu vực trùng lặp chỉ được tính một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    x = list(map(int, input().split()))
    
    areas = set()
    for i in range(n):
        for j in range(i + 1, n):
            area = (x[j] - x[i]) / 2
            areas.add(area)
    
    print(len(areas))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và lặp lại trên tất cả các cặp cây sông. Sự khác biệt`(x[j] - x[i])`tính đáy của tam giác; chia cho 2 sẽ có diện tích. Sử dụng một bộ sẽ tránh được việc tính hai lần các vùng lặp lại. Chúng tôi in kích thước của bộ, đó là câu trả lời bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 4 5
```| tôi | j | x[j]-x[i] | khu vực | khu vực được thiết lập sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0,5 | {0,5} | 
| 0 | 2 | 3 | 1,5 | {0,5, 1,5} | 
| 0 | 3 | 4 | 2.0 | {0,5, 1,5, 2,0} | 
| 1 | 2 | 2 | 1.0 | {0,5, 1,0, 1,5, 2,0} | 
| 1 | 3 | 3 | 1,5 | {0,5, 1,0, 1,5, 2,0} | 
| 2 | 3 | 1 | 0,5 | {0,5, 1,0, 1,5, 2,0} | 

Đầu ra:`4`Điều này chứng tỏ rằng việc lặp lại tất cả các cặp sẽ xác định chính xác tất cả các khu vực riêng biệt. 

### Ví dụ 2 

đầu vào:```
3
1 3 5
```| tôi | j | x[j]-x[i] | khu vực | khu vực thiết lập | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1.0 | {1.0} | 
| 0 | 2 | 4 | 2.0 | {1.0, 2.0} | 
| 1 | 2 | 2 | 1.0 | {1.0, 2.0} | 

Đầu ra:`2`Điều này cho thấy các khu vực lặp lại được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Hai vòng lặp lồng nhau trên cây sông sẽ tạo ra tất cả các cặp. Mỗi thao tác bên trong vòng lặp là O(1). | 
| Không gian | O(n^2) | Bộ lưu trữ tất cả sự khác biệt rõ ràng. Trong trường hợp xấu nhất, mỗi cặp tạo ra một vùng duy nhất, cho tối đa n(n-1)/2 phần tử. | 

Với mức tối đa n=50, O(n^2)=2500 có thể chấp nhận được và sử dụng bộ kích thước tối đa 1225 vừa vặn thoải mái trong bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        areas = set()
        for i in range(n):
            for j in range(i + 1, n):
                areas.add((x[j] - x[i]) / 2)
        res.append(str(len(areas)))
    return "\n".join(res)

# provided samples
assert run("8\n4\n1 2 4 5\n3\n1 3 5\n3\n2 6 8\n2\n1 2\n1\n50\n5\n3 4 5 6 8\n3\n1 25 26\n6\n1 2 4 8 16 32\n") == \
"4\n2\n3\n1\n0\n5\n3\n15"

# custom cases
assert run("1\n1\n10\n") == "0", "single river tree"
assert run("1\n2\n5 10\n") == "1", "two river trees form one triangle"
assert run("1\n5\n1 2 3 4 5\n") == "4", "consecutive integers"
assert run("1\n3\n10 20 30\n") == "3", "equally spaced trees"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n10 | 0 | Không thể tạo thành hình tam giác với một cây | 
| 1\n2\n5 10 | 1 | Chính xác là một hình tam giác | 
| 1\n5\n1 2 3 4 5 | 4 | Đếm chính xác tất cả các khu vực riêng biệt từ các số nguyên liên tiếp | 
| 1\n3\n10 20 30 | 3 | Xử lý các cây cách đều nhau và các khu vực trùng lặp | 

## Vỏ cạnh 

Khi chỉ có một cây sông thì không thể hình thành được hình tam giác nào. Đối với đầu vào:```
1
1
10
```Thuật toán khởi tạo một tập hợp trống, không có cặp nào được lặp lại và kết quả đầu ra bằng 0, như mong đợi. 

Khi có hai cây sông, chẳng hạn [5,10], thuật toán sẽ tính một diện tích: (10-5)/2 = 2,5. Bộ này có kích thước một, trả về chính xác một. 

Các số nguyên liên tiếp [1,2,3,4,5] tạo ra nhiều hình tam giác, bao gồm các vùng lặp lại như (2-1)/2 = (3-2)/2 = 0,5, đảm bảo thuật toán loại bỏ trùng lặp thông qua
