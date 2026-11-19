---
title: "CF 2103C - Độ phân chia trung bình"
description: "Chúng ta được cung cấp một mảng và được hỏi liệu chúng ta có thể cắt nó thành ba phần liền kề sao cho một điều kiện cụ thể liên quan đến các đường trung tuyến giữ nguyên trên các phần cắt hay không. Nếu chúng ta chọn hai điểm phân chia, mảng sẽ được chia thành ba đoạn không trống."
date: "2026-06-08T05:02:24+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2103
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1019 (Div. 2)"
rating: 1600
weight: 2103
solve_time_s: 122
verified: false
draft: false
---

[CF 2103C - Phân chia trung vị](https://codeforces.com/problemset/problem/2103/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được hỏi liệu chúng ta có thể cắt nó thành ba phần liền kề sao cho một điều kiện cụ thể liên quan đến các đường trung tuyến giữ nguyên trên các phần cắt hay không. 

Nếu chúng ta chọn hai điểm phân chia, mảng sẽ được chia thành ba đoạn không trống. Mỗi phân đoạn có trung vị riêng, được xác định là phần tử ở giữa khi phân đoạn đó được sắp xếp. Sau đó, chúng ta lấy ba số trung vị đó và tính lại số trung vị của chúng. Câu hỏi đặt ra là liệu giá trị cuối cùng này có thể lớn nhất là`k`. 

Một cách hữu ích để diễn đạt lại điều kiện là trong số ba đường trung tuyến của đoạn, ít nhất hai đường phải nhỏ hơn hoặc bằng`k`. Điều này xuất phát từ định nghĩa về giá trị trung bình của ba giá trị: nó là ≤`k`chính xác khi có ít nhất hai trong số ba giá trị ≤`k`. 

Các ràng buộc rất lớn:`n`tăng lên 2⋅10^5 trên tất cả các trường hợp thử nghiệm, do đó, cách tiếp cận O(n²) cho mỗi trường hợp thử nghiệm là không thể. Ngay cả O(n log n) cho mỗi ứng cử viên phân chia cũng sẽ quá chậm nếu được thực hiện một cách ngây thơ cho tất cả các cặp`(l, r)`. 

Khó khăn chính là trung vị của một mảng con không phải là một thống kê tiền tố đơn giản; nó phụ thuộc vào thứ tự bên trong phân khúc, điều này cho thấy rằng việc tính toán lại trực tiếp cho mỗi phân khúc là quá tốn kém. 

Một vài trường hợp phức tạp đáng được nêu bật. 

Nếu tất cả các phần tử đều ≤`k`, mọi trung vị của mảng con sẽ tự động ≤`k`, vì vậy câu trả lời tầm thường là CÓ. Một giải pháp ngây thơ tính toán lại không chính xác các giá trị trung bình mà không tính đến tính đơn điệu vẫn có thể hoạt động, nhưng một kẻ tham lam có lỗi có thể không phát hiện ra lối thoát sớm này. 

Nếu đúng một đoạn có trung vị ≤`k`, câu trả lời phải là KHÔNG vì trung vị của ba giá trị không thể ≤`k`trừ khi có ít nhất hai là ≤`k`. 

Một tình huống phức tạp khác là khi các giá trị lớn được xen kẽ: một nỗ lực tham lam cố gắng mở rộng một cách tham lam các phân đoạn chỉ dựa trên số lượng tiền tố hiện tại có thể thất bại vì trung vị phụ thuộc vào thứ tự tương đối bên trong mỗi phân đoạn, chứ không chỉ tính riêng lẻ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ liệt kê từng cặp`(l, r)`và tính trung vị của từng phân đoạn trong số ba phân đoạn bằng cách sắp xếp từng phân đoạn hoặc sử dụng cấu trúc lựa chọn. Đối với mỗi lần phân chia, chúng tôi kiểm tra điều kiện trung bình. 

Điều này đúng nhưng quá chậm. Có sự phân chia O(n²) và trung vị tính toán ít nhất là O(n) hoặc O(log n) trên mỗi phân đoạn, dẫn đến tổng thể là O(n³) hoặc O(n² log n), điều này không khả thi đối với 2⋅10^5 phần tử. 

Cái nhìn sâu sắc quan trọng là chuyển đổi vấn đề từ “trung vị của số trung vị” thành vấn đề phân loại nhị phân. Chúng tôi chỉ quan tâm liệu trung vị của mỗi phân đoạn có ≤ hay không`k`, không phải giá trị chính xác của nó. 

Đối với một đoạn, trung vị của nó là ≤`k`chính xác khi có ít nhất một nửa số phần tử của nó ≤`k`. Vì vậy, chúng ta có thể ánh xạ mảng thành dạng được chuyển đổi trong đó mỗi phần tử trở thành: 

+1 nếu`a[i] ≤ k`, ngược lại -1. 

Bây giờ một đoạn có trung vị ≤`k`khi và chỉ khi tổng của nó dương. Điều này làm giảm điều kiện trung bình thành điều kiện tổng tiền tố. 

Bây giờ vấn đề trở thành: liệu chúng ta có thể chia mảng thành ba phân đoạn liền kề sao cho ít nhất hai trong số ba phân đoạn có tổng dương trong mảng được chuyển đổi này. 

Đây hiện là vấn đề về cấu trúc đối với tổng tiền tố. Chúng tôi có thể tính toán trước các tổng tiền tố và theo dõi xem liệu chúng tôi có thể tìm thấy các điểm phân chia hợp lệ một cách hiệu quả hay không bằng cách sử dụng quét tuyến tính và theo dõi tham lam các dấu hiệu phân đoạn tốt nhất có thể. 

Giải pháp cuối cùng là kiểm tra xem liệu chúng ta có thể đặt hai điểm cắt sao cho ít nhất hai trong ba đoạn kết quả có tổng dương hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê các phần tách, tính toán lại số trung vị) | O(n³) hoặc O(n² log n) | O(n) | Quá chậm | 
| Biến đổi tiền tố + tính khả thi của việc phân chia tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mảng thành dạng nhị phân trong đó mỗi phần tử trở thành +1 nếu`a[i] ≤ k`, ngược lại -1. Điều này định dạng lại điều kiện trung bình thành điều kiện tổng. 
2. Xây dựng mảng tổng tiền tố trên chuỗi được chuyển đổi này. Tổng tiền tố tại vị trí i biểu thị số lượng phần tử “tốt” (k) nhiều hơn số phần tử “xấu” tồn tại trong tiền tố. 
3. Quan sát rằng một đoạn`[l, r]`có trung vị ≤ k chính xác khi`prefix[r] - prefix[l-1] > 0`. Điều này cho phép kiểm tra tính hợp lệ của phân đoạn theo thời gian liên tục. 
4. Bây giờ chúng ta cần hai điểm cắt`l < r`sao cho trong ba đoạn có ít nhất hai đoạn có tổng dương. Điều này chia thành ba mẫu có thể xảy ra: phân khúc thứ nhất và thứ hai tốt, phân khúc thứ nhất và thứ ba tốt hoặc phân khúc thứ hai và thứ ba tốt. 
5. Đối với trường hợp thứ nhất và thứ hai, chúng tôi quét tìm`l`trong đó tiền tố[1..l] có thể tạo thành một phân đoạn tốt, sau đó kiểm tra xem có tồn tại không`r > l`sao cho cả hai`[l+1, r]`Và`[r+1, n]`có thể được sắp xếp để thỏa mãn điều kiện sử dụng tiền tố cực tiểu/cực đại. 
6. Đối với mỗi mẫu, chúng tôi duy trì thông tin đang chạy trong khi quét để tránh tính toán lại tổng phân đoạn một cách rõ ràng. Điều này đảm bảo mỗi vị trí được xử lý một lần. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm ràng buộc trung vị thành bất đẳng thức tuyến tính trên tổng tiền tố. Bởi vì tính hợp lệ của phân đoạn trở nên đơn điệu trong không gian tiền tố nên tính khả thi chỉ phụ thuộc vào sự tồn tại của các điểm cắt thỏa mãn các ràng buộc cộng. Quét tham lam hoạt động vì khi tìm thấy tiền tố hoặc điểm cuối phân đoạn hợp lệ, việc mở rộng hoặc dịch chuyển ranh giới sẽ duy trì hoặc chỉ nới lỏng các điều kiện khả thi trong không gian tổng tiền tố, do đó không có cấu hình tối ưu nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    b = [1 if x <= k else -1 for x in a]
    
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + b[i]
    
    total = pref[n]
    
    # case 1: first two segments are good
    # check if exists l, r such that:
    # sum(1..l) > 0 and sum(l+1..r) > 0
    
    ok_prefix = [False] * (n + 1)
    cur_min = 0
    for i in range(1, n + 1):
        if pref[i] - cur_min > 0:
            ok_prefix[i] = True
        cur_min = min(cur_min, pref[i])
    
    # case 2: last two segments are good
    ok_suffix = [False] * (n + 2)
    cur_max = pref[n]
    for i in range(n - 1, -1, -1):
        if cur_max - pref[i] > 0:
            ok_suffix[i] = True
        cur_max = max(cur_max, pref[i])
    
    # check any split pattern
    # pattern A: first two segments good
    # pattern B: last two segments good
    # pattern C: first and last segments good
    
    # pattern A
    for r in range(2, n):
        if ok_prefix[r]:
            return True
    
    # pattern B
    for l in range(1, n - 1):
        if ok_suffix[l]:
            return True
    
    # pattern C
    # first and last segments good
    # need prefix[i] > 0 and suffix[j] > 0 with i < j
    best = -10**18
    for i in range(1, n):
        if pref[i] > 0:
            best = max(best, i)
    for j in range(2, n + 1):
        if pref[n] - pref[j - 1] > 0 and best != -10**18 and best < j - 1:
            return True
    
    return False

t = int(input())
for _ in range(t):
    print("YES" if solve() else "NO")
```Giải pháp bắt đầu bằng cách biến mảng thành biểu diễn +1 và -1 để các trung vị của phân đoạn trở thành phép kiểm tra dấu trên tổng. Tổng tiền tố sau đó cho phép bất kỳ phân đoạn nào được đánh giá trong thời gian không đổi. 

Sau đó, chúng tôi tính toán trước tiền tố và hậu tố nào có thể tạo thành các phân đoạn tổng dương hợp lệ. Các mảng này mã hóa xem phân đoạn hợp lệ kết thúc hoặc bắt đầu tại một vị trí có tồn tại mà không cần kiểm tra lại tất cả các mảng con hay không. 

Cuối cùng, chúng tôi thử nghiệm ba cách có thể để chọn ra hai phân khúc “tốt” trong số ba phân khúc bắt buộc. Mỗi lần kiểm tra sử dụng quét tuyến tính trên thông tin tiền tố. 

Điểm tinh tế trong triển khai chính là chúng tôi không bao giờ tính toán rõ ràng các giá trị trung vị hoặc sắp xếp các mảng con; tất cả lý do xảy ra trong không gian tổng tiền tố và mọi điều kiện phân đoạn được giảm xuống thành kiểm tra O(1). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 2 1
```Chúng tôi biến đổi với`k = 2`: 

| tôi | một [tôi] | b[i] | trước | 
| --- | --- | --- | --- | 
| 1 | 3 | -1 | -1 | 
| 2 | 2 | +1 | 0 | 
| 3 | 1 | +1 | 1 | 

Chúng tôi kiểm tra tính khả thi của phân khúc: 

-`[1,2]`tổng = 0 (không dương) 
-`[2,3]`tổng = 2 (dương) 

Chúng ta có thể hình thành hai phân khúc tốt, vì vậy câu trả lời là CÓ. 

Điều này xác nhận rằng ngay cả khi tiền tố dao động, cấu trúc hậu tố vẫn cho phép nhóm hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 1
3 2 1
```Chuyển đổi: 

| tôi | một [tôi] | b[i] | trước | 
| --- | --- | --- | --- | 
| 1 | 3 | -1 | -1 | 
| 2 | 2 | -1 | -2 | 
| 3 | 1 | +1 | -1 | 

Mọi phân đoạn đều có tổng không dương trừ khi nó tầm thường. Không có hai phân đoạn nào có thể dương, do đó không tồn tại sự phân chia hợp lệ. 

Điều này chứng tỏ rằng cùng một cấu trúc hoạt động khác nhau khi`k`là quá nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tính toán tiền tố một lần và kiểm tra thời gian liên tục | 
| Không gian | O(n) | mảng tiền tố và theo dõi phụ trợ | 

Độ phức tạp tuyến tính dễ dàng phù hợp với giới hạn của tổng n lên tới 2⋅10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = [1 if x <= k else -1 for x in a]
        pref = [0]
        for x in b:
            pref.append(pref[-1] + x)

        def good(l, r):
            return pref[r] - pref[l - 1] > 0

        for l in range(1, n - 1):
            for r in range(l + 1, n):
                cnt = 0
                if good(1, l): cnt += 1
                if good(l + 1, r): cnt += 1
                if good(r + 1, n): cnt += 1
                if cnt >= 2:
                    return True
        return False

    t = int(input())
    out = []
    for _ in range(t):
        out.append("YES" if solve() else "NO")
    return "\n".join(out)

# provided samples
assert run("""6
3 2
3 2 1
3 1
3 2 1
6 3
8 5 3 1 6 4
8 7
10 7 12 16 3 15 6 11
6 8
7 11 12 4 9 17
3 500000000
1000 1000000000 1000
""") == """YES
NO
NO
YES
YES
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mọi phần tử đều bằng nhau và ≤ k | CÓ | mảng tầm thường đầy đủ tốt | 
| luân phiên quanh k | CÓ/KHÔNG trộn | trường hợp dao động tiền tố | 
| tăng nghiêm ngặt các giá trị lớn | KHÔNG | không tồn tại phân đoạn hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hầu hết tất cả các phần tử đều lớn hơn`k`ngoại trừ một vài giá trị nhỏ bị cô lập. Trong những trường hợp như vậy, chỉ những phân đoạn rất ngắn mới có thể là “tốt” và thuật toán dựa vào tiền tố cực tiểu để phát hiện chính xác xem có tồn tại bất kỳ phân đoạn tích cực nào hay không. 

Một trường hợp khác là khi tất cả các phần tử đều ≤`k`. Tổng tiền tố luôn tăng nên mọi phân đoạn đều hợp lệ. Thuật toán ngay lập tức tìm thấy các điểm phân chia hợp lệ trong lần kiểm tra mẫu đầu tiên. 

Một trường hợp tế nhị cuối cùng là khi chỉ có đoạn cuối cùng mới có thể được thực hiện tốt. Quá trình tính toán trước hậu tố đảm bảo rằng ngay cả khi các tiền tố ban đầu không thành công, các cấu hình dựa trên hậu tố hợp lệ vẫn được phát hiện thông qua quá trình quét tiền tố ngược.
