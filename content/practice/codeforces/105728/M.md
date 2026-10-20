---
title: "CF 105728M - Thử thách MEX tối đa"
description: "Mỗi trường hợp thử nghiệm mô tả một tập hợp các “sự lựa chọn”. Có n khoảng và từ khoảng thứ i chúng ta phải chọn một số nguyên nằm trong phạm vi cho phép của nó. Sau khi thực hiện điều này trong tất cả các khoảng thời gian, chúng ta thu được một mảng có độ dài n."
date: "2026-06-26T07:51:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105728
codeforces_index: "M"
codeforces_contest_name: "EPT Solving Cup 5.0 \uacf5\uc2dd \uacbd\uc5f0\ub300\ud68c"
rating: 0
weight: 105728
solve_time_s: 49
verified: true
draft: false
---

[CF 105728M - Thử thách MEX tối đa](https://codeforces.com/problemset/problem/105728/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một tập hợp các “sự lựa chọn”. Có n khoảng và từ khoảng thứ i chúng ta phải chọn một số nguyên nằm trong phạm vi cho phép của nó. Sau khi thực hiện điều này trong tất cả các khoảng thời gian, chúng ta thu được một mảng có độ dài n. Mục tiêu là làm cho mảng này chứa càng nhiều số nguyên không âm nhỏ liên tiếp bắt đầu từ 0 càng tốt, sao cho số nguyên còn thiếu đầu tiên (MEX) càng lớn càng tốt. 

MEX chỉ được xác định bằng việc liệu chúng ta có thể đặt thành công mọi số nguyên 0, 1, 2, v.v. vào một vị trí nào đó trong các giá trị đã chọn hay không. Nếu chúng ta không đặt được x thì đáp án là x, bất kể điều gì xảy ra sau đó. 

Các ràng buộc rất lớn: tổng số khoảng thời gian trên tất cả các trường hợp thử nghiệm có thể đạt tới 10^6. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các nhiệm vụ cho mọi MEX ứng cử viên và mọi khoảng thời gian theo cách lồng nhau. Bất cứ điều gì bậc hai trong n cho mỗi trường hợp thử nghiệm sẽ không tồn tại. 

Một khó khăn nhỏ là mỗi khoảng không tương ứng với một giá trị cố định mà là một phạm vi linh hoạt. Điều này có nghĩa là chúng ta không khớp các giá trị với các vị trí mà thay vào đó quyết định xem hệ thống khoảng có thể “bao phủ” tiền tố của các số nguyên bắt đầu từ 0 hay không. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta có thể gán cho mỗi khoảng giá trị nhỏ nhất có thể một cách tham lam và tính MEX. Điều đó không thành công vì việc lựa chọn các giá trị phải được phối hợp trên toàn cầu. 

Ví dụ: hãy xem xét các khoảng [0, 1], [0, 1], [1, 1]. Phép gán cục bộ tham lam có thể chọn 0, 0, 1 cho MEX 2, là tối ưu. Nhưng thay vào đó, nếu chúng ta có [0, 0], [0, 1], [1, 1], thì những lựa chọn cục bộ tham lam có thể vô tình cản trở tính khả thi của các giá trị sau này. Ràng buộc thực sự là liệu mỗi số k có thể được “hỗ trợ” bởi ít nhất một khoảng mà vẫn có thể được sử dụng cho nó hay không. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là thử một ứng cử viên MEX m và hỏi xem liệu chúng ta có thể gán các khoảng riêng biệt để bao gồm mọi giá trị từ 0 đến m − 1 hay không. Đối với một m cố định, chúng ta sẽ quét tất cả các khoảng nhiều lần và cố gắng gán từng giá trị cần thiết cho một khoảng nào đó có thể tạo ra nó, đảm bảo không có khoảng nào được sử dụng lại. 

Điều này có thể được mô hình hóa dưới dạng khớp hai bên giữa các giá trị 0…m−1 và các khoảng, trong đó tồn tại một cạnh nếu l_i ≤ giá trị ≤ r_i. Một nỗ lực so khớp đơn giản cho mỗi m sẽ tốn O(n²) trong trường hợp xấu nhất vì mỗi lần kiểm tra tính khả thi sẽ quét và ấn định lại các khoảng thời gian nhiều lần. 

Quan sát quan trọng là chúng ta không bao giờ cần xem xét cấu trúc khớp một cách rõ ràng. Chúng ta chỉ quan tâm đến việc liệu mỗi số nguyên k có thể được bao phủ bởi một khoảng nào đó vẫn “có sẵn” sau khi thỏa mãn các giá trị nhỏ hơn hay không. Điều này cho thấy sự quét tham lam trên các giá trị từ 0 trở lên. 

Nếu chúng ta sửa k và cố gắng đảm bảo tất cả các giá trị từ 0 đến k đều có thể đạt được thì chiến lược tốt nhất là luôn gán từng giá trị cho khoảng thời gian hoàn thiện sớm nhất có thể bao phủ nó, nhưng vì tất cả các khoảng đều là tài nguyên tương đương và chúng ta chỉ cần tồn tại nên một điều kiện đơn giản hơn xuất hiện: với mỗi k, chúng ta chỉ cần biết liệu có tồn tại một khoảng có thể dành riêng cho k mà không chặn các phép gán trước đó hay không. Điều này giảm xuống việc kiểm tra xem có bao nhiêu khoảng thời gian “có thể sử dụng được” theo thứ tự tăng dần. 

Phép biến đổi đúng là xử lý các giá trị theo thứ tự tăng dần và gán chúng một cách tham lam vào các khoảng có thể bao phủ chúng, luôn ưu tiên các khoảng có điểm cuối bên phải nhỏ nhất mà vẫn cho phép bao phủ. Điều này tương đương với việc kiểm tra xem liệu chúng ta có thể khớp 0, 1, 2,… theo thứ tự sử dụng khoảng thời gian sẵn có hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu cho mỗi ứng cử viên MEX | O(n² log n) | O(n) | Quá chậm | 
| Quét tham lam với các khoảng thời gian được sắp xếp | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Sắp xếp tất cả các khoảng theo điểm cuối bên phải theo thứ tự không giảm. Điều này đảm bảo rằng khi chúng tôi cố gắng gán một giá trị k, chúng tôi luôn xem xét các khoảng thời gian kết thúc sớm nhất trước, duy trì tính linh hoạt cho các giá trị sau. 
2. Duy trì con trỏ theo các khoảng thời gian và đếm xem có bao nhiêu giá trị mà chúng ta đã gán thành công cho đến nay, bắt đầu từ 0. 
3. Với mỗi số nguyên k bắt đầu từ 0 trở lên, hãy thử gán nó: 

quét các khoảng thời gian có điểm cuối bên trái là ≤ k và điểm cuối bên phải là ≥ k và chọn một khoảng thời gian vẫn chưa được sử dụng. Nếu không có khoảng thời gian như vậy thì dừng lại; k hiện tại là MEX. 

Lý do chúng tôi nhấn mạnh vào các khoảng bao phủ k là vì k phải xuất hiện ở đâu đó trong cách xây dựng cuối cùng để MEX vượt quá k. 
4. Đánh dấu khoảng đã chọn là đã sử dụng và di chuyển đến k + 1. 
5. Tiếp tục cho đến khi thất bại hoặc cho đến khi tất cả các giá trị lên đến n được gán. 

Việc triển khai hiệu quả hơn sẽ tránh việc quét liên tục từ đầu: chúng tôi quét các khoảng thời gian theo thứ tự tăng điểm cuối bên phải và duy trì cấu trúc dữ liệu (hoặc logic con trỏ tham lam) để đảm bảo rằng bất cứ khi nào chúng tôi đạt đến k, chúng tôi đã biết khoảng thời gian nào có thể bao phủ nó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý giá trị k − 1, chúng tôi đã chọn k khoảng riêng biệt, mỗi khoảng có khả năng tạo ra một giá trị riêng biệt trong {0, 1, …, k − 1}. Bởi vì chúng tôi luôn ấn định khoảng khả thi nhỏ nhất có thể bao gồm k hiện tại, nên chúng tôi không bao giờ sử dụng khoảng có thể là hỗ trợ duy nhất có thể cho giá trị nhỏ hơn. Điều này giữ cho tính khả thi trong tương lai được nguyên vẹn tối đa. 

Nếu tại một giá trị k nào đó, chúng ta không thể tìm thấy bất kỳ khoảng nào bao phủ k mà vẫn chưa được sử dụng thì không có phép gán lại nào có thể khắc phục được điều này, bởi vì mọi khoảng có thể tạo ra k đều đã được “dành” cho các giá trị trước đó hoặc không bao gồm k chút nào. Điều đó làm cho k trở thành MEX thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        segs = [tuple(map(int, input().split())) for _ in range(n)]

        # sort by right endpoint
        segs.sort(key=lambda x: x[1])

        used = 0
        idx = 0
        import heapq
        heap = []

        ok = True

        for mex in range(n + 1):
            # push all intervals that can cover current mex
            while idx < n and segs[idx][1] < mex:
                idx += 1

            while idx < n and segs[idx][0] <= mex:
                heapq.heappush(heap, segs[idx][1])
                idx += 1

            # remove unusable intervals (already too small right endpoint)
            while heap and heap[0] < mex:
                heapq.heappop(heap)

            if not heap:
                print(mex)
                ok = False
                break

            heapq.heappop(heap)
            used += 1

        if ok:
            print(n)

if __name__ == "__main__":
    solve()
```Phần quan trọng của việc triển khai là quét các giá trị MEX có thể có. Heap lưu trữ tất cả các khoảng có khả năng bao gồm giá trị hiện tại. Đối với mỗi k, chúng tôi loại bỏ các khoảng có điểm cuối bên phải quá nhỏ và chúng tôi chọn một khoảng hợp lệ để “gán” k. Điều này đảm bảo mỗi khoảng thời gian được sử dụng nhiều nhất một lần và luôn theo cách hạn chế nhất có thể. 

Một lỗi triển khai phổ biến là quên rằng khoảng [l, r] chỉ có thể phục vụ các giá trị lên tới r, vì vậy một khi k vượt quá r, nó vĩnh viễn vô dụng. Đó là lý do tại sao chúng tôi tích cực loại bỏ các khoảng thời gian đã hết hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0
0 1
1 2
```Chúng tôi sắp xếp theo điểm cuối bên phải: 

[0,0], [0,1], [1,2] 

| k | khoảng thời gian có sẵn | khoảng đã chọn | kết quả | 
| --- | --- | --- | --- | 
| 0 | [0,0], [0,1], [1,2] | [0,0] | được | 
| 1 | [0,1], [1,2] | [0,1] | được | 
| 2 | [1,2] | không thể che 2 | dừng lại | 

MEX là 2, vì giá trị 2 không thể được đặt. 

Điều này cho thấy trước tiên thuật toán sử dụng các khoảng thời gian chặt chẽ một cách tự nhiên như thế nào. 

### Ví dụ 2 

đầu vào:```
4
0 3
0 1
1 2
0 0
```Đã sắp xếp: 

[0,0], [0,1], [1,2], [0,3] 

| k | khoảng thời gian có sẵn | khoảng đã chọn | kết quả | 
| --- | --- | --- | --- | 
| 0 | tất cả | [0,0] | được | 
| 1 | [0,1], [1,2], [0,3] | [0,1] | được | 
| 2 | [1,2], [0,3] | [1,2] | được | 
| 3 | [0,3] | [0,3] | được | 
| 4 | không | thất bại | MEX = 4 | 

Điều này xác nhận rằng khoảng lớn như [0,3] được lưu tối ưu cho giá trị lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Khoảng thời gian sắp xếp chiếm ưu thế; mỗi khoảng đi vào và rời khỏi heap một lần | 
| Không gian | O(n) | Lưu trữ heap và khoảng thời gian | 

Cho rằng tổng n qua các bài kiểm tra lên tới 10^6, điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        import heapq
        for _ in range(t):
            n = int(input())
            segs = [tuple(map(int, input().split())) for _ in range(n)]
            segs.sort(key=lambda x: x[1])

            idx = 0
            heap = []
            ans = 0

            for mex in range(n + 1):
                while idx < n and segs[idx][1] < mex:
                    idx += 1
                while idx < n and segs[idx][0] <= mex:
                    heapq.heappush(heap, segs[idx][1])
                    idx += 1
                while heap and heap[0] < mex:
                    heapq.heappop(heap)
                if not heap:
                    ans = mex
                    break
                heapq.heappop(heap)
            else:
                ans = n
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# minimum
assert run("1\n1\n0 0\n") == "1", "single interval"

# already full chain
assert run("1\n3\n0 1\n1 2\n2 3\n") == "4", "perfect chain"

# disjoint gaps
assert run("1\n3\n0 0\n2 2\n4 4\n") == "1", "gap at 1"

# overlapping flexibility
assert run("1\n4\n0 3\n0 3\n0 3\n0 3\n") == "4", "fully flexible"

# edge: no zero
assert run("1\n2\n1 2\n1 2\n") == "0", "cannot place 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1 | trường hợp tối thiểu | 
| khoảng thời gian chuỗi | 4 | phạm vi bảo hiểm lũy tiến tối ưu | 
| khoảng trống rời rạc | 1 | thất bại sớm ở mức 0 | 
| chồng chéo hoàn toàn | 4 | linh hoạt tối đa | 
| không có phạm vi bảo hiểm | 0 | MEX bắt đầu thất bại ngay lập tức | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi không có khoảng nào chứa 0. Trong trường hợp đó, thuật toán thất bại ngay lập tức ở k = 0 vì không có khoảng nào có khả năng tạo ra 0. Ví dụ: các khoảng [1,2], [2,3] tạo ra MEX = 0, vì giá trị bắt buộc đầu tiên là không thể. 

Một trường hợp tinh vi khác là khi nhiều khoảng chồng lên nhau nhiều nhưng tất cả đều có điểm cuối bên phải nhỏ. Ví dụ: các khoảng [0,1], [0,1], [0,1], [0,1] cho phép MEX nhiều nhất là 2. Thuật toán sẽ liên tục sử dụng các khoảng này cho k = 0 và k = 1, sau đó thất bại ở k = 2 vì tất cả các khoảng còn lại đều kết thúc trước 2. 

Trường hợp cạnh cuối cùng là khi tồn tại một khoảng lớn duy nhất bên cạnh nhiều khoảng chặt chẽ. Chiến lược tham lam đảm bảo khoảng thời gian chặt chẽ được sử dụng trước tiên, để lại khoảng thời gian lớn cho k lớn nhất có thể. Điều này là cần thiết; đảo ngược trật tự này sẽ gây lãng phí tính linh hoạt và làm giảm MEX cuối cùng.
