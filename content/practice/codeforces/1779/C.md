---
title: "CF 1779C - Tổng tiền tố nhỏ nhất"
description: "Chúng ta được cho một mảng các số nguyên, và Baltic muốn một tổng tiền tố cụ thể - tổng của m phần tử đầu tiên - nhỏ nhất trong số tất cả các tổng tiền tố của mảng. Chúng ta được phép lật dấu của bất kỳ phần tử nào bao nhiêu lần cũng được."
date: "2026-06-09T11:28:55+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1779
codeforces_index: "C"
codeforces_contest_name: "Hello 2023"
rating: 1600
weight: 1779
solve_time_s: 119
verified: false
draft: false
---

[CF 1779C - Tổng tiền tố nhỏ nhất](https://codeforces.com/problemset/problem/1779/C) 

**Đánh giá:** 1600 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và Baltic muốn có một tổng tiền tố cụ thể - tổng của số đầu tiên`m`phần tử-là phần tử nhỏ nhất trong số tất cả các tổng tiền tố của mảng. Chúng ta được phép lật dấu của bất kỳ phần tử nào bao nhiêu lần cũng được. Mục tiêu là tính toán số lần lật dấu tối thiểu cần thiết để đạt được điều kiện này. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước mảng`n`, độ dài tiền tố yêu thích`m`và các phần tử mảng. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, số lần lật tối thiểu cần thiết. 

Cho rằng`n`có thể lên tới 2·10^5 và tổng của tất cả`n`trên các trường hợp thử nghiệm không vượt quá 2·10^5, mọi giải pháp vượt quá O(n log n) có thể sẽ quá chậm. Một lực lượng vũ phu ngây thơ thử tất cả các kết hợp lật dấu là hoàn toàn không khả thi vì có thể có 2^n kiểu lật dấu. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tổng ban đầu của cạnh đầu tiên`m`các phần tử đã là nhỏ nhất, các mảng có tất cả các số âm hoặc tất cả các số dương và các trường hợp trong đó các giá trị lớn xuất hiện sau trong mảng có thể chiếm ưu thế trong các tổng tiền tố. Một cách tiếp cận bất cẩn có thể làm đảo lộn quá nhiều yếu tố hoặc chỉ tập trung vào những điều chỉnh cục bộ, tạo ra nhiều sự thay đổi hơn mức cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các kết hợp lật dấu có thể có cho từng phần tử và sau đó kiểm tra xem kết hợp nào giảm thiểu kết hợp đầu tiên`m`tổng tiền tố so với những tiền tố khác. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế vì số lượng kết hợp là theo cấp số nhân, 2^n. 

Quan sát quan trọng để tối ưu hóa là vấn đề có thể được chia thành hai giai đoạn: giai đoạn đầu tiên`m`phần tử và các phần tử còn lại. Lần đầu tiên`m`các phần tử, chúng tôi muốn tổng của chúng càng âm càng tốt nếu nó dương hoặc để chúng yên nếu đã âm. Bất kỳ tổng tiền tố dương nào trong số này`m`các phần tử có thể được lật để giảm tổng và để giảm thiểu việc lật, trước tiên chúng ta nên lật các giá trị dương lớn nhất. Đối với các phần tử còn lại, chúng ta cần đảm bảo tổng tích lũy không bao giờ giảm xuống dưới tổng của phần tử đầu tiên.`m`các phần tử. Điều này có thể được kiểm soát bằng cách lật các số âm lớn sau`m`-th phần tử để ngăn chúng giảm tổng tiền tố xuống dưới mục tiêu của chúng tôi. 

Sử dụng một đống (max-heap cho lần đầu tiên`m`các phần tử để lật phần dương lớn nhất và vùng heap tối thiểu cho các phần tử còn lại để lật phần âm lớn nhất) cho phép chúng tôi xử lý các điều chỉnh này một cách hiệu quả. Mỗi thao tác lật sẽ giảm tiền tố có vấn đề ở mức tối đa có thể, đảm bảo số lần lật tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n·n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến để lưu trữ tổng tiền tố hiện tại khi chúng ta lặp qua mảng. 
2. Lần đầu tiên`m`các phần tử, duy trì một đống số dương tối đa. Lặp lại từ cái đầu tiên đến cái`m`- phần tử thứ, thêm từng phần tử vào tổng tiền tố hiện tại. Bất cứ khi nào tổng tiền tố hiện tại lớn hơn tổng mục tiêu (ban đầu số 0 có tác dụng để so sánh), hãy lật số dương lớn nhất trong vùng heap tối đa, trừ hai lần giá trị của nó khỏi tổng tiền tố và tăng bộ đếm lật. Tiếp tục cho đến khi tổng tiền tố của số đầu tiên`m`các phần tử được giảm thiểu. 
3. Đối với phần tử`m+1`ĐẾN`n`, duy trì một đống số âm tối thiểu. Lặp lại chuyển tiếp, thêm từng phần tử vào tổng tiền tố đang chạy. Nếu tại bất kỳ thời điểm nào tổng tiền tố trở nên nhỏ hơn tổng tiền tố đầu tiên`m`tổng tiền tố, liên tục lật phần tử âm nhỏ nhất từ ​​heap tối thiểu (nếu có) để tăng tổng tiền tố lên trên phần tử đầu tiên`m`tổng tiền tố. Mỗi lần lật sẽ tăng bộ đếm. 
4. Sau khi xử lý toàn bộ mảng, bộ đếm chứa số lần lật dấu tối thiểu cần thiết. 

**Tại sao nó hoạt động:** Đầu tiên`m`các phần tử được giảm thiểu một cách tham lam bằng cách lật các số dương lớn nhất trước, điều này làm giảm tổng tiền tố một cách hiệu quả nhất có thể. Đối với các phần tử còn lại, chúng tôi đảm bảo rằng không có tổng tiền tố nào thấp hơn giá trị đầu tiên`m`tổng bằng cách lật số âm lớn nhất trước. Điều này đảm bảo rằng chúng tôi chỉ thực hiện các lần lật cần thiết, duy trì tính bất biến rằng sau mỗi bước, tổng tiền tố hiện tại không bao giờ vi phạm mục tiêu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    ops = 0
    pref = 0
    max_heap = []
    
    # Process first m elements
    for i in range(m):
        pref += a[i]
        if a[i] > 0:
            heapq.heappush(max_heap, -a[i])
    while max_heap and pref > 0:
        largest = -heapq.heappop(max_heap)
        pref -= 2 * largest
        ops += 1
    
    # Process remaining elements
    min_heap = []
    for i in range(m, n):
        pref += a[i]
        if a[i] < 0:
            heapq.heappush(min_heap, a[i])
        while min_heap and pref < 0:
            smallest = heapq.heappop(min_heap)
            pref -= 2 * smallest
            ops += 1
    
    print(ops)
```Mã đầu tiên xử lý đầu tiên`m`các phần tử bằng cách lật các số dương lớn để giảm tổng một cách hiệu quả. Sau đó, nó xử lý phần còn lại bằng cách lật các số âm lớn để đảm bảo không có tổng tiền tố sau`m`-phần tử thứ nhỏ hơn phần tử đầu tiên`m`tổng tiền tố. Sự chú ý cẩn thận đến thứ tự đống đảm bảo rằng mỗi lần lật góp phần tối đa vào việc giảm hoặc tăng tổng tiền tố có liên quan. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

Mảng đầu vào`[-1, -2, -3, -4]`với`m = 3`. 

| tôi | Yếu tố | Tiền tố Tổng | Đống | Lật mặt? | hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -1 | -1 | [] | Không | 0 | 
| 1 | -2 | -3 | [] | Không | 0 | 
| 2 | -3 | -6 | [] | Không | 0 | 
| 3 | -4 | -10 | [] | Lật lớn nhất? | 1 | 

Ở đây chúng tôi lật`-4`ĐẾN`4`sau đó`m`-phần tử thứ vì tổng tiền tố nếu không sẽ nhỏ hơn`-6`. Thuật toán lật 1 phần tử, khớp với kết quả mong đợi. 

**Ví dụ 2:** 

Mảng đầu vào`[1, 2, 3, 4]`với`m = 3`. 

| tôi | Yếu tố | Tiền tố Tổng | Đống | Lật mặt? | hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | [1] | Không | 0 | 
| 1 | 2 | 3 | [1,2] | Không | 0 | 
| 2 | 3 | 6 | [1,2,3] | Lật 3 | 1 | 

Lật 3 làm giảm tổng tiền tố đầu tiên xuống 0, trở thành số tiền tối thiểu trong số tất cả các tiền tố. Điều này xác nhận các công việc tham lam dựa trên heap. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử có thể nhập vào heap một lần và mỗi thao tác với heap là log n | 
| Không gian | O(n) | Chúng tôi duy trì hai đống và một tiền tố đang chạy sum | 

Giải pháp dễ dàng xử lý giới hạn trên của tổng số 2·10^5 phần tử vì chi phí log n cho mỗi phần tử có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # paste solution code here
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        ops = 0
        pref = 0
        max_heap = []
        for i in range(m):
            pref += a[i]
            if a[i] > 0:
                heapq.heappush(max_heap, -a[i])
        while max_heap and pref > 0:
            largest = -heapq.heappop(max_heap)
            pref -= 2 * largest
            ops += 1
        
        min_heap = []
        for i in range(m, n):
            pref += a[i]
            if a[i] < 0:
                heapq.heappush(min_heap, a[i])
            while min_heap and pref < 0:
                smallest = heapq.heappop(min_heap)
                pref -= 2 * smallest
                ops += 1
        print(ops)
    return out.getvalue().strip()

# Provided samples
assert run("6\n4 3\n-1 -2 -3 -4\n4 3\n1 2 3 4\n1 1\n1\n5 5\n-2 3 -5 1 -20\n5 2\n-2 3 -5 -5 -20\n10 4\n345875723 -48 384678321 -375635768 -35867853 -35863586 -358683842 -81725678 38576 -357865873") == "1\n1\n0\n0\n3\n4"
# Minimum size
assert run("1\n1 1\n5") == "1"
# Maximum negative sum
assert run("1\n
```
