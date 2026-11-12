---
title: "CF 1676D - X-Sum"
description: "Chúng ta có một lưới hai chiều biểu diễn một bàn cờ trong đó mỗi ô chứa một số nguyên không âm. Một quân tượng có thể được đặt trên bất kỳ ô nào và nó tấn công tất cả các ô dọc theo bốn đường chéo giao nhau tại vị trí của nó. Phòng giam mà giám mục chiếm giữ được tính là bị tấn công."
date: "2026-06-10T00:59:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1676
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 790 (Div. 4)"
rating: 1000
weight: 1676
solve_time_s: 107
verified: true
draft: false
---

[CF 1676D - X-Sum](https://codeforces.com/problemset/problem/1676/D) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hai chiều biểu diễn một bàn cờ trong đó mỗi ô chứa một số nguyên không âm. Một quân tượng có thể được đặt trên bất kỳ ô nào và nó tấn công tất cả các ô dọc theo bốn đường chéo giao nhau tại vị trí của nó. Phòng giam mà giám mục chiếm giữ được tính là bị tấn công. Nhiệm vụ của chúng ta là tìm tổng giá trị tối đa từ các ô mà quân tượng có thể tấn công, xem xét tất cả các vị trí có thể có trên bảng. 

Đầu vào chỉ định nhiều trường hợp kiểm thử, mỗi trường hợp có kích thước và giá trị bảng riêng. Các ràng buộc cho chúng ta biết rằng bảng có thể lớn tới 200 x 200 và tổng của tất cả các ô trong tất cả các trường hợp thử nghiệm được giới hạn ở mức 40.000. Điều này ngụ ý rằng mặc dù một giải pháp đơn giản kiểm tra từng ô riêng lẻ có thể hoạt động cho một trường hợp thử nghiệm duy nhất, nhưng việc lặp lại lồng nhau đơn giản trên các đường chéo cho mỗi ô sẽ quá chậm đối với các bảng lớn hơn và nhiều trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các bảng chỉ có một hàng hoặc cột, trong đó quân tượng có thể tấn công rất ít ô và các bảng có tất cả các giá trị bằng 0 ngoại trừ một giá trị lớn duy nhất. Một cách tiếp cận bất cẩn làm đếm sai đường chéo hoặc đếm kép ô trung tâm có thể thất bại trong những tình huống này. Ví dụ: bảng 2x2 có các giá trị`[[1, 2], [3, 4]]`có quân tượng được đặt ở (1,1) đạt được tổng 1+4=5 nếu chỉ đếm chính xác hai ô chéo, nhưng một phép tính tổng đơn giản có thể đếm gấp đôi hoặc bỏ sót một số ô. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên quan đến việc lặp qua từng ô trên bảng, sau đó lặp dọc theo cả bốn đường chéo để tính tổng các giá trị bị tấn công. Điều này hiệu quả vì khu vực tấn công của mỗi quân tượng có thể đoán trước được, nhưng nó trở nên quá chậm đối với các bảng lớn nhất. Đối với một bảng 200x200, mỗi ô có thể lấy O(n+m) để tính tổng các đường chéo, dẫn đến O(n_m_(n+m)) = O(200_200_400) = 16.000.000 thao tác cho mỗi trường hợp thử nghiệm, gần với giới hạn trên của tính khả thi và chậm đối với nhiều trường hợp thử nghiệm. 

Quan sát quan trọng là các ô nằm trên cùng một đường chéo có chung một đặc tính: hoặc là sự khác biệt`(row - col)`hoặc tổng`(row + col)`là không đổi. Các đường chéo trên cùng bên trái đến dưới cùng bên phải được xác định bởi`row - col`và các đường chéo trên cùng bên phải đến dưới cùng bên trái được xác định bởi`row + col`. Nếu chúng ta tính toán trước tổng của mỗi đường chéo thì tổng của tất cả các ô bị quân tượng tấn công tại vị trí`(i,j)`chỉ đơn giản là tổng của nó`row-col`đường chéo cộng với tổng của nó`row+col`đường chéo, trừ đi giá trị tại`(i,j)`(để tránh tính hai lần chính ô đó). Việc tính toán trước các đường chéo này làm giảm phép tính trên mỗi ô xuống O(1), mang lại tổng độ phức tạp là O(n*m) trên mỗi bảng, đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_m_(n+m)) | O(1) | Quá chậm cho bảng lớn | 
| Tối ưu | O(n*m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai từ điển để giữ tổng các giá trị dọc theo đường chéo. Một bản đồ từ điển`row - col`tới tổng các giá trị dọc theo đường chéo đó và các bản đồ khác`row + col`tổng các giá trị dọc theo đường chéo đó. 
2. Lặp lại từng ô`(i,j)`trong bảng. Thêm giá trị của nó vào tương ứng`row - col`tổng đường chéo và`row + col`tổng đường chéo. Bước này tổng hợp các đóng góp từ tất cả các ô vào đường chéo của chúng trong một lần duy nhất. 
3. Sau khi tính tổng các đường chéo, lặp lại lần nữa trên mỗi ô`(i,j)`và tính tổng số ô bị tấn công bằng cách đặt quân tượng ở`(i,j)`bằng tổng của nó`row - col`đường chéo cộng với tổng của nó`row + col`chéo, trừ đi giá trị riêng của ô để tránh tính hai lần. 
4. Theo dõi số tiền tối đa gặp phải trong lần lặp này. Sau khi kiểm tra tất cả các ô, tổng tối đa chính là câu trả lời cho trường hợp kiểm thử đó. 

Tại sao nó hoạt động: Thuật toán đảm bảo tính chính xác vì mỗi tổng đường chéo chứa chính xác tất cả các giá trị trên đường chéo đó. Bằng cách kết hợp hai đường chéo đi qua một ô và trừ đi ô trung tâm một lần, chúng tôi tính toán chính xác từng ô mà quân tượng tấn công mà không bỏ sót hoặc tính hai lần bất kỳ giá trị nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    diag1 = {}
    diag2 = {}
    
    for i in range(n):
        for j in range(m):
            d1 = i - j
            d2 = i + j
            diag1[d1] = diag1.get(d1, 0) + a[i][j]
            diag2[d2] = diag2.get(d2, 0) + a[i][j]
    
    max_sum = 0
    for i in range(n):
        for j in range(m):
            total = diag1[i - j] + diag2[i + j] - a[i][j]
            if total > max_sum:
                max_sum = total
    
    print(max_sum)
```Vòng lặp đầu tiên tính tổng đường chéo. Việc sử dụng từ điển đảm bảo chúng tôi xử lý được các chỉ số phủ định từ`row-col`đường chéo không có lỗi. Vòng lặp thứ hai tính toán tổng tấn công Bishop một cách hiệu quả cho mọi ô bằng cách sử dụng các khoản tiền được tính toán trước này. Trừ`a[i][j]`tránh việc đếm hai lần chính ô đó, một điểm khó nhận thấy dễ bị bỏ qua. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ban:```
1 2 2 1
2 4 2 4
2 2 3 1
2 4 2 4
```Tổng đường chéo chính: 

| Đường chéo (i-j) | Tổng hợp | 
| --- | --- | 
| -3 | 1 | 
| -2 | 2+2=4 | 
| -1 | 2+2+4=8 | 
| 0 | 1+4+3+4=12 | 
| 1 | 2+2+2=6 | 
| 2 | 2+4=6 | 
| 3 | 1+4=5 | 

| Đường chéo (i+j) | Tổng hợp | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2+2=4 | 
| 2 | 2+4+2=8 | 
| 3 | 1+2+3+1=7 | 
| 4 | 4+2+2=8 | 
| 5 | 4+4=8 | 
| 6 | 4 | 

Giám mục tại`(1,1)`có`diag1[0]=12`Và`diag2[2]=8`, trừ`4`đối với ô được đếm kép, tổng số là 16. Giá trị tối đa xảy ra ở`(1,2)`hoặc`(3,1)`với tổng số 20. 

### Mẫu 2 

Ban:```
1
0
```Chỉ có một ô, tổng giám mục là 1. Thuật toán tính toán chính xác`diag1[0]=1`,`diag2[0]=1`, trừ`1`, tổng cộng 1. 

Những dấu vết này cho thấy tổng đường chéo tổng hợp chính xác và việc trừ giá trị ô sẽ tránh được việc tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Hai lần lặp lại đầy đủ trên bảng: lần đầu tiên để tính tổng đường chéo, lần thứ hai để tính tổng tấn công tối đa | 
| Không gian | O(n+m) | Lưu trữ tổng có đường chéo n+m trong hai từ điển | 

Giải pháp xử lý tổng của tất cả các ô lên tới 40.000 một cách dễ dàng. Ngay cả với 1.000 trường hợp thử nghiệm, mỗi bảng nhỏ đều hoàn thành nhanh chóng nhờ xử lý tuyến tính trên mỗi ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution starts here
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        
        diag1 = {}
        diag2 = {}
        for i in range(n):
            for j in range(m):
                diag1[i - j] = diag1.get(i - j, 0) + a[i][j]
                diag2[i + j] = diag2.get(i + j, 0) + a[i][j]
        
        max_sum = 0
        for i in range(n):
            for j in range(m):
                total = diag1[i - j] + diag2[i + j] - a[i][j]
                if total > max_sum:
                    max_sum = total
        print(max_sum)
    return output.getvalue().strip()

# Provided samples
assert run("4\n4 4\n1 2 2 1\n2 4 2 4\n2 2 3 1\n2 4 2 4\n2 1\n1\n0\n3 3\n1 1 1\n1 1 1\n1 1 1\n3 3\n0 1 1\n1 0 1\n1 1 0\n") == "20\n1\n5\n3", "sample 1"

# Custom cases
assert run("1\n1 1\n100\n") == "100", "single cell"
assert run("1\n2 2\n
```
