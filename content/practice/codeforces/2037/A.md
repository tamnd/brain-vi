---
title: "CF 2037A - Hai Lần"
description: "Chúng ta được cho một mảng nhỏ các số nguyên. Kinich có thể ghi điểm bằng cách chọn liên tục hai chỉ số riêng biệt có giá trị bằng nhau, với hạn chế là mỗi chỉ số chỉ được sử dụng nhiều nhất một lần. Nhiệm vụ là xác định số lượng tối đa các cặp như vậy cho mỗi trường hợp thử nghiệm."
date: "2026-06-08T10:12:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2037
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 988 (Div. 3)"
rating: 800
weight: 2037
solve_time_s: 139
verified: true
draft: false
---

[CF 2037A - Hai lần](https://codeforces.com/problemset/problem/2037/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng nhỏ các số nguyên. Kinich có thể ghi điểm bằng cách chọn liên tục hai chỉ số riêng biệt có giá trị bằng nhau, với hạn chế là mỗi chỉ số chỉ được sử dụng nhiều nhất một lần. Nhiệm vụ là xác định số lượng tối đa các cặp như vậy cho mỗi trường hợp thử nghiệm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định độ dài của mảng và chính các phần tử của mảng. Đầu ra là số điểm tối đa mà Kinich có thể đạt được trong trường hợp thử nghiệm đó. Vì mỗi phép toán sử dụng chính xác hai phần tử có cùng giá trị nên điểm số được xác định hoàn toàn bằng số lượng cặp số giống hệt nhau tồn tại. 

Các ràng buộc rất chặt chẽ: độ dài mảng$n$nhiều nhất là 20 và mỗi phần tử nằm trong khoảng từ 1 đến$n$. Điều này ngụ ý rằng ngay cả các cách tiếp cận bạo lực cũng khả thi vì tổng số kết hợp mà chúng tôi có thể kiểm tra bị giới hạn bởi$n \choose 2$nhiều lần, con số này rất nhỏ đối với$n \le 20$. 

Các trường hợp cạnh không rõ ràng xảy ra khi tất cả các phần tử đều khác biệt hoặc tất cả các phần tử đều giống hệt nhau. Ví dụ: nếu mảng là [1,2,3,4] thì không thể thực hiện thao tác nào, do đó điểm sẽ là 0. Ngược lại, nếu mảng là [2,2,2,2], chúng ta có thể tạo hai cặp rời nhau, mang lại điểm là 2. Việc triển khai bất cẩn chỉ đếm các số duy nhất hoặc quên ngăn việc sử dụng lại các chỉ mục có thể tạo ra kết quả không chính xác trong các trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cặp số bằng nhau có thể có, đánh dấu chúng là đã sử dụng và lặp lại để tối đa hóa điểm số. Điều này đúng vì nó khám phá tất cả các chuỗi ghép đôi có thể có. Tuy nhiên, ngay cả với$n=20$, phương pháp đệ quy này có sự tăng trưởng giai thừa, điều này không cần thiết cho bài toán này. 

Quan sát quan trọng là chúng ta không cần xem xét vị trí của các con số mà chỉ cần xem xét tần số của chúng. Mỗi số có thể đóng góp nhiều nhất$\lfloor \text{frequency} / 2 \rfloor$tính điểm vì mỗi cặp yêu cầu hai lần xuất hiện. Tổng hợp$\lfloor \text{frequency} / 2 \rfloor$trên tất cả các số riêng biệt trực tiếp cho điểm tối đa. Điều này làm giảm vấn đề về việc đếm số lần xuất hiện, điều này không quan trọng đối với các ràng buộc đã cho. 

Brute-force hoạt động vì nó liệt kê rõ ràng tất cả các cặp hợp lệ, nhưng nó không thành công khi$n$ngày càng lớn hơn vì số lượng kết hợp bùng nổ. Việc quan sát tần số đếm biến bài toán thành số học đơn giản, tránh mọi sự bùng nổ tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm/không cần thiết | 
| Đếm tần số | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc độ dài mảng$n$và các phần tử mảng. 
3. Khởi tạo một mảng tần số có kích thước$n+1$về không. Điều này sẽ lưu trữ số lần xuất hiện của mỗi số nguyên từ 1 đến$n$. 
4. Duyệt mảng và tăng tần số của mỗi số. 
5. Khởi tạo điểm thành 0. 
6. Với mỗi số trong dãy tần số, hãy tính xem có bao nhiêu cặp có thể được tạo thành bằng cách chia số nguyên tần số cho 2. Thêm số này vào điểm. 
7. Xuất điểm cho test case. 

Điều này có hiệu quả vì số lượng cặp không chồng chéo tối đa cho bất kỳ số nào chính xác bằng một nửa số lần xuất hiện của nó. Tính tổng tất cả các số sẽ cho số điểm tối đa có thể đạt được mà không cần phải theo dõi từng chỉ số riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = [0] * (n + 1)
    for x in a:
        freq[x] += 1
    
    score = 0
    for count in freq:
        score += count // 2
    
    print(score)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, đếm số lần xuất hiện của mỗi số và tính số cặp không trùng nhau tối đa. Dải tần số có kích thước`n+1`để thuận tiện cho việc lập chỉ mục dựa trên 1 và phép chia số nguyên đảm bảo chỉ tính các cặp hoàn chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 1 2 3 1`| Số | Tần số | Cặp | Điểm | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 1 | 0 | 1 | 

Điểm là 1, tương ứng với cặp số 1. 

### Ví dụ 2 

đầu vào:`6 1 2 3 1 2 3`| Số | Tần số | Cặp | Điểm | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 2 | 1 | 3 | 

Điểm là 3, đại diện cho một cặp cho mỗi giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Đếm tần số và tính tổng các cặp cho từng test case | 
| Không gian | O(n) | Mảng tần số để lưu trữ số lượng từng số nguyên | 

Với những hạn chế$t \le 500$Và$n \le 20$, giải pháp dễ dàng thực thi tốt trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = [0] * (n + 1)
        for x in a:
            freq[x] += 1
        score = sum(count // 2 for count in freq)
        print(score)
    
    return output.getvalue().strip()

# Provided samples
assert run("5\n1\n1\n2\n2 2\n2\n1 2\n4\n1 2 3 1\n6\n1 2 3 1 2 3\n") == "0\n1\n0\n1\n3", "sample 1"

# Custom test cases
assert run("1\n4\n1 1 1 1\n") == "2", "all equal elements"
assert run("1\n4\n1 2 3 4\n") == "0", "all distinct"
assert run("1\n5\n1 2 2 3 3\n") == "2", "multiple pairs"
assert run("1\n1\n1\n") == "0", "single element"
assert run("1\n6\n1 1 2 2 2 2\n") == "3", "mixed frequencies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 2 | Đếm chính xác nhiều cặp có cùng giá trị | 
| 1 2 3 4 | 0 | Không thể có cặp số riêng biệt | 
| 1 2 2 3 3 | 2 | Đếm chính xác nhiều số khác nhau theo cặp | 
| 1 | 0 | Vỏ cạnh đơn | 
| 1 1 2 2 2 2 | 3 | Xử lý tần số hỗn hợp | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[1]`, thuật toán khởi tạo mảng tần số, đếm`1`một lần, chia cho 2 để nhận 0 cặp và xuất ra 0. Đối với các mảng có tất cả các phần tử bằng nhau, chẳng hạn như`[2,2,2,2]`, tần số là 4, số nguyên chia cho 2 được 2 cặp, chính xác là số điểm tối đa. Các trường hợp hỗn hợp như`[1,1,2,2,2,2]`đếm tần số năng suất`[0,2,4,...]`, tạo ra 1+2=3 điểm, cho thấy thuật toán xử lý chính xác các tần số khác nhau trên các số khác nhau.
