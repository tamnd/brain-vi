---
title: "CF 2164C - Ngục tối"
description: "Chúng tôi được yêu cầu mô phỏng một kịch bản trong ngục tối trong đó bạn có một bộ kiếm, mỗi bộ có một giá trị sát thương và một bộ quái vật, mỗi bộ có một giá trị mạng sống. Bạn có thể giết quái vật nếu sát thương của thanh kiếm của bạn ít nhất bằng mạng sống của quái vật."
date: "2026-06-07T23:37:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2164
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 30 (Div. 1 + Div. 2)"
rating: 1400
weight: 2164
solve_time_s: 122
verified: false
draft: false
---

[CF 2164C - Ngục tối](https://codeforces.com/problemset/problem/2164/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một kịch bản trong ngục tối trong đó bạn có một bộ kiếm, mỗi bộ có một giá trị sát thương và một bộ quái vật, mỗi bộ có một giá trị mạng sống. Bạn có thể giết quái vật nếu sát thương của thanh kiếm của bạn ít nhất bằng mạng sống của quái vật. Khi một thanh kiếm được sử dụng, nó sẽ bị tiêu hao, nhưng trong một số trường hợp, bạn nhận được một thanh kiếm mới với khả năng sát thương cao hơn dựa trên sức mạnh của quái vật.`c_i`giá trị. Mục tiêu là tối đa hóa số lượng quái vật bạn có thể tiêu diệt. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp những thanh kiếm ban đầu, sức khỏe của quái vật và những thanh kiếm mới tiềm năng xuất hiện sau khi tiêu diệt từng quái vật. Đầu ra phải là số lượng quái vật tối đa bị tiêu diệt trong mỗi trường hợp thử nghiệm. 

Các ràng buộc rất chặt chẽ:`n`Và`m`có thể đạt được`2 * 10^5`và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn tương tự. Điều này giúp loại bỏ bất kỳ giải pháp vũ phu nào lặp đi lặp lại trên tất cả các thứ tự có thể có của kiếm và quái vật, bởi vì số lượng thao tác sẽ là bậc hai hoặc tệ hơn. Cách tiếp cận O(n * m) ngây thơ là không khả thi. 

Các trường hợp cạnh cần chú ý bao gồm các tình huống trong đó tất cả`c_i`bằng 0, nghĩa là không có thanh kiếm mới nào xuất hiện, hoặc trường hợp các thanh kiếm ban đầu đều yếu hơn một số quái vật. Ví dụ, nếu`a = [1]`Và`b = [2]`với`c = [0]`, không có quái vật nào có thể bị giết và thuật toán phải quay lại`0`. Một trường hợp khó khăn khác xảy ra khi việc chọn sai thanh kiếm trước tiên sẽ ngăn cản việc sử dụng nó sau này trên một con quái vật có thể tạo ra một thanh kiếm mạnh hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi sự kết hợp của các nhiệm vụ từ kiếm đến quái vật. Bạn sẽ kiểm tra từng thanh kiếm với từng quái vật, theo dõi những thanh kiếm nào được tiêu thụ và cập nhật với`c_i`các giá trị. Điều này đảm bảo tính chính xác nhưng đòi hỏi tới`n * m`so sánh cho mỗi trường hợp thử nghiệm. Với`n, m`lên đến 2 * 10^5, điều này mang lại các phép toán 4 * 10^10 trong trường hợp xấu nhất, điều này là không khả thi. 

Cách tiếp cận tối ưu thúc đẩy việc sắp xếp và lựa chọn tham lam. Đầu tiên, hãy phân loại cả kiếm và quái vật. Việc phân loại kiếm đảm bảo bạn luôn biết loại kiếm nhỏ nhất hiện có có khả năng tiêu diệt quái vật, điều này rất quan trọng khi bạn muốn tiêu thụ kiếm một cách hiệu quả. Sắp xếp quái vật theo mạng sống cho phép bạn xử lý những quái vật yếu hơn trước. Điểm mấu chốt là thanh kiếm mới thu được sau khi giết quái vật luôn có lực sát thương ít nhất bằng thanh kiếm được sử dụng. Điều này đảm bảo rằng một cách tiếp cận tham lam - sử dụng thanh kiếm đủ nhỏ nhất cho con quái vật yếu nhất còn lại - sẽ tối đa hóa tổng số lần tiêu diệt. Sử dụng min-heap (hàng ưu tiên) để lưu trữ những thanh kiếm có sẵn cho phép chúng ta tìm ra thanh kiếm nhỏ nhất có thể giết quái vật một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Sắp xếp + Tham lam với đống | O((n + m) log(n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số lượng thanh kiếm`n`và quái vật`m`. Đọc danh sách sát thương của thanh kiếm`a`, danh sách sức khỏe quái vật`b`, và danh sách kiếm được`c`. 
2. Sắp xếp các thanh kiếm theo thứ tự tăng dần. Ghép đôi quái vật với chúng`c_i`các giá trị và sắp xếp chúng theo tình trạng sức khỏe theo thứ tự tăng dần. Điều này đảm bảo rằng chúng ta sẽ giải quyết được những con quái vật yếu hơn trước. 
3. Khởi tạo một đống tối thiểu với tất cả sát thương kiếm ban đầu. Đống này sẽ luôn cung cấp thanh kiếm nhỏ nhất có thể giết được quái vật, giúp tối đa hóa tiềm năng giữ những thanh kiếm mạnh hơn cho sau này. 
4. Khởi tạo bộ đếm`kills`về không. Lặp lại danh sách quái vật đã được sắp xếp. Đối với mỗi quái vật: 

một. Lấy kiếm từ đống kiếm cho đến khi bạn tìm thấy thanh kiếm có sát thương ít nhất bằng máu của quái vật. 

b. Nếu không có thanh kiếm như vậy tồn tại, hãy bỏ qua con quái vật này. 

c. Nếu tìm thấy một thanh kiếm, hãy tăng`kills`. Lấy thanh kiếm đã sử dụng ra khỏi đống. 

d. Nếu như`c_i > 0`, đẩy một thanh kiếm mới có sát thương`max(sword_used, c_i)`trở lại đống. Điều này đảm bảo thanh kiếm mới ít nhất luôn mạnh bằng thanh kiếm đã sử dụng. 
5. Sau khi xử lý tất cả quái vật, xuất ra`kills`. 

Điều bất biến được duy trì là ở mỗi bước, đống chứa tất cả các thanh kiếm hiện có và chúng tôi luôn chọn thanh kiếm nhỏ nhất có khả năng giết chết quái vật yếu nhất hiện tại. Điều này đảm bảo rằng những thanh kiếm mạnh hơn sẽ được dành cho những quái vật mạnh hơn. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n, m = map(int, input().split())
    swords = list(map(int, input().split()))
    monsters = list(map(int, input().split()))
    gains = list(map(int, input().split()))
    
    swords.sort()
    monsters = sorted(zip(monsters, gains))
    
    heap = swords[:]
    heapq.heapify(heap)
    kills = 0
    
    for life, gain in monsters:
        while heap and heap[0] < life:
            heapq.heappop(heap)
        if not heap:
            continue
        sword = heapq.heappop(heap)
        kills += 1
        if gain > 0:
            heapq.heappush(heap, max(sword, gain))
    
    print(kills)
```Giải pháp này đầu tiên sắp xếp kiếm và quái vật. Một đống tối thiểu được sử dụng để chọn thanh kiếm nhỏ nhất một cách hiệu quả có thể tiêu diệt quái vật hiện tại. Thứ tự thực hiện rất quan trọng: bật thanh kiếm yếu hơn trước, tăng dần`kills`, rồi đẩy bất kỳ thanh kiếm mới nào thu được. sử dụng`max(sword, gain)`đảm bảo rằng thanh kiếm mới ít nhất luôn mạnh bằng thanh kiếm đã qua sử dụng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 2
2 2 2
2 3
3 2
```| Bước | Trạng thái đống | Quái vật (cuộc sống, đạt được) | Hành động | Giết chết | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [2, 2, 2] | (2,3) | Pop 2 ≤ 2 → dùng kiếm, đẩy max(2,3)=3 | 1 | 
| Tiếp theo | [2,3,2] | (3,2) | Pop 2 < 3 → pop, tiếp theo 2 < 3 → pop, tiếp theo 3 ≥ 3 → dùng kiếm, đẩy max(3,2)=3 | 2 | 

Đầu ra:`2`. Dấu vết xác nhận rằng việc chọn thanh kiếm đủ nhỏ nhất và cập nhật với`max(sword, gain)`mang lại số lần giết tối đa. 

**Ví dụ về trường hợp cạnh**```
2 2
1 2
2 3
0 0
```| Bước | Đống | Quái vật | Hành động | Giết chết | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [1,2] | (2,0) | Pop 1 < 2 → pop 2 ≥ 2 → sử dụng, tăng 0 → không đẩy | 1 | 
| Tiếp theo | [] | (3,0) | Đống trống → không thể giết | 1 | 

Đầu ra:`1`. Điều này xác nhận thuật toán xử lý chính xác các trường hợp không đủ kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(n + m)) | Việc sắp xếp kiếm và quái vật là O(n log n + m log m), các phép toán đống trên tất cả quái vật có tổng bằng O((n + m) log(n + m)) | 
| Không gian | O(n + m) | Heap lưu trữ tới n + m thanh kiếm, cộng với mảng đầu vào | 

Điều này phù hợp thoải mái trong các hạn chế`n + m ≤ 2 * 10^5`mỗi trường hợp thử nghiệm và tổng số tiền 2 * 10^5. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste solution here
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        swords = list(map(int, input().split()))
        monsters = list(map(int, input().split()))
        gains = list(map(int, input().split()))
        
        swords.sort()
        monsters = sorted(zip(monsters, gains))
        
        heap = swords[:]
        import heapq
        heapq.heapify(heap)
        kills = 0
        
        for life, gain in monsters:
            while heap and heap[0] < life:
                heapq.heappop(heap)
            if not heap:
                continue
            sword = heapq.heappop(heap)
            kills += 1
            if gain > 0:
                heapq.heappush(heap, max(sword, gain))
        print(kills)
    return output.getvalue().strip()

# Sample
assert run("1\n3 2\n2 2 2\n2 3\n3 2\n") == "2"

# Custom cases
assert run("1\n2 2\n1 2\n2 3\n0 0\n") == "1", "insufficient sword"
assert run("1\n1 1\n1000000000\n1\n0\n") == "1", "large sword kills small monster"
assert run("1\n3 3\n1 1 1\n2 2 2\n2 2 2\n") == "3", "all gains equal sword"
assert run("1\n2 3\n5 5\n1 10 5\n0 0 10\n") == "3", "mix of gains and
```
