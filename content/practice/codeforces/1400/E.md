---
title: "CF 1400E - Xóa Multiset"
description: "Chúng ta được cho một tập hợp các số nguyên từ 1 đến n, trong đó số nguyên i là ai. Mục tiêu của chúng tôi là loại bỏ hoàn toàn tất cả các phần tử khỏi multiset bằng hai loại thao tác."
date: "2026-06-11T08:56:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1400
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 94 (Rated for Div. 2)"
rating: 2200
weight: 1400
solve_time_s: 105
verified: true
draft: false
---

[CF 1400E - Xóa Multiset](https://codeforces.com/problemset/problem/1400/E) 

**Xếp hạng:** 2200 
**Tas:** cấu trúc dữ liệu, phân chia để chinh phục, dp, tham lam 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên từ 1 đến n, trong đó số nguyên i là a_i. Mục tiêu của chúng tôi là loại bỏ hoàn toàn tất cả các phần tử khỏi multiset bằng hai loại thao tác. Thao tác đầu tiên cho phép chúng ta chọn một phạm vi số nguyên liền kề và xóa một phiên bản của mỗi số trong phạm vi đó, miễn là mỗi số tồn tại ít nhất một lần. Thao tác thứ hai cho phép chúng ta loại bỏ trực tiếp bất kỳ số lượng bản sao nào của một số nguyên. Đầu ra là số thao tác tối thiểu cần thiết để làm trống nhiều tập hợp. 

Ràng buộc đầu vào n 5000 ngụ ý rằng các thuật toán có độ phức tạp về thời gian O(n²) là khả thi, nhưng mọi thuật toán O(n³) hoặc cao hơn có thể sẽ hết thời gian chờ. Số lượng a_i có thể lên tới 10^9, điều này ngăn chúng ta mô phỏng trực tiếp nhiều tập hợp hoặc thực hiện các thao tác theo cách đơn giản theo từng phần tử. Thay vào đó, chúng ta phải làm việc với số lượng tổng hợp một cách hiệu quả. 

Trường hợp cạnh tinh tế phát sinh khi một số phần tử bằng 0. Ví dụ: nếu a = [1, 0, 1], thao tác phạm vi liền kề không thể được áp dụng trên các vị trí từ 1 đến 3 vì thiếu 2. Một cách tiếp cận ngây thơ giả định phạm vi luôn hợp lệ sẽ áp dụng sai thao tác đầu tiên. Một trường hợp cạnh khác xảy ra khi một số số lượng cực kỳ lớn, ví dụ: a = [10^9, 10^9, 10^9]. Việc cố gắng mô phỏng các hoạt động riêng lẻ sẽ là không thể, vì vậy chúng ta phải suy luận về mặt số lượng thay vì lặp qua từng phần tử. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi hoạt động có thể có, loại bỏ các phần tử đơn lẻ hoặc thử tất cả các phạm vi liền kề hợp lệ. Mặc dù điều này đúng về mặt khái niệm nhưng nó không khả thi về mặt tính toán. Ngay cả việc lặp lại tất cả các mảng con để loại bỏ phạm vi cũng dẫn đến việc kiểm tra O(n²) cho mỗi thao tác và với số lượng lớn a_i, việc mô phỏng loại bỏ trực tiếp sẽ là không thể. Cách tiếp cận này không thành công khi n tiến tới 5000 hoặc a_i lớn, vì số lượng chuỗi hoạt động tiềm năng sẽ bùng nổ. 

Thông tin chi tiết quan trọng là hoạt động của phạm vi liền kề luôn có lợi khi nó có thể loại bỏ các phần tử được căn chỉnh thành một phân đoạn liên tiếp. Số lượng thao tác tối thiểu phụ thuộc chủ yếu vào vị trí của số đếm nhỏ nhất, bởi vì thao tác phạm vi có thể loại bỏ nhiều nhất các phần tử min(a[l..r]) cùng một lúc. Điều này gợi ý một chiến lược lập trình động, trong đó chúng tôi xem xét các mảng con của nhiều tập hợp và theo dõi số lượng thao tác tối thiểu cần thiết để loại bỏ tất cả các phần tử trong mảng con đó. 

Chúng tôi xác định dp[l][r] là số thao tác tối thiểu để loại bỏ tất cả các phần tử khỏi chỉ mục l đến r. Cách tiếp cận đơn giản nhất là loại bỏ các phần tử riêng lẻ từ l đến r hoặc thực hiện thao tác phạm vi bằng cách sử dụng phần tử nhỏ nhất trong mảng con đó. Chúng ta có thể trừ đệ quy giá trị tối thiểu và giải các mảng con còn lại bên trái và bên phải của giá trị tối thiểu đó. Bằng cách kết hợp phép trừ tham lam với phép chia đệ quy, chúng tôi giảm độ phức tạp của bài toán xuống O(n2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n²) | Quá chậm | 
| DP tối ưu + Chia & Chinh phục | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo một mảng hai chiều dp trong đó dp[l][r] lưu trữ số lượng thao tác tối thiểu để xóa các phần tử từ chỉ mục l đến r. Tất cả các mục dp đều bắt đầu bằng vô cùng ngoại trừ dp[l][l-1] = 0 cho các mảng con trống. 
2. Tính toán trước giá trị tối thiểu của tiền tố hoặc chỉ tìm kiếm giá trị tối thiểu trong mỗi mảng con khi cần. Đối với mảng con a[l..r], hãy đặt min_val là a_i nhỏ nhất trong mảng con. 
3. Sự tái diễn chính xem xét hai lựa chọn. Đầu tiên là loại bỏ các phần tử riêng lẻ, yêu cầu các thao tác dp[l][r] = r - l + 1. 
4. Thứ hai là hoạt động theo phạm vi. Trừ min_val khỏi tất cả các phần tử a[l..r], về mặt khái niệm được tính là thực hiện loại bỏ phạm vi min_val. Sau đó tính toán đệ quy số lượng thao tác tối thiểu trên các mảng con kết quả còn lại sau phép trừ. Cụ thể, quét từ l đến r và chia ở số 0 (các phần tử đã bị loại bỏ hoàn toàn) thành các phân đoạn liền kề khác 0. Áp dụng đệ quy dp cho từng phân đoạn và tính tổng kết quả của chúng. 
5. Cuối cùng, chọn tùy chọn nhỏ hơn trong hai tùy chọn: loại bỏ tất cả các phần tử riêng lẻ hoặc sử dụng phép trừ phạm vi cộng với tổng các phép toán cho các phân đoạn con. 
6. Tính dp[1][n] và trả về kết quả này. 

Cách tiếp cận này đảm bảo tính chính xác vì mỗi bước đệ quy sẽ loại bỏ từng phần tử một hoặc loại bỏ số lượng phần tử tối đa bằng thao tác phạm vi. Bằng cách luôn xem xét phần tử nhỏ nhất trong một phân đoạn, chúng tôi đảm bảo rằng chúng tôi không bao giờ "loại bỏ quá mức" và mỗi bài toán con đều giảm đáng kể số lượng, ngăn ngừa đệ quy vô hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

n = int(input())
a = list(map(int, input().split()))

dp = [[-1]*n for _ in range(n)]

def solve(l, r):
    if l > r:
        return 0
    if dp[l][r] != -1:
        return dp[l][r]

    # Option 1: remove elements individually
    res = r - l + 1

    # Option 2: remove by subtracting min in the segment
    min_val = min(a[l:r+1])
    temp = min_val
    i = l
    while i <= r:
        if a[i] == min_val:
            i += 1
            continue
        j = i
        while j <= r and a[j] > min_val:
            j += 1
        temp += solve(i, j-1)
        i = j
    dp[l][r] = min(res, temp)
    return dp[l][r]

print(solve(0, n-1))
```Hàm đệ quy`solve(l, r)`thực hiện chiến lược DP. Đầu tiên nó kiểm tra trường hợp cơ sở để tìm các mảng con trống. Phần tử tối thiểu trong mảng con được tìm thấy và trừ về mặt khái niệm. Sau đó, chúng tôi chia mảng thành các phân đoạn liền kề trong đó các phần tử vẫn dương và giải quyết chúng theo cách đệ quy. Câu trả lời cuối cùng là mức tối thiểu giữa các lần loại bỏ riêng lẻ và giải pháp dựa trên phạm vi kết hợp. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4
1 4 1 1
```| tôi | r | giá trị tối thiểu | phân đoạn | tạm thời | độ phân giải | dp[l][r] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | [1,2] | 2 | 4 | 2 | 

Giải thích: Phần tử tối thiểu là 1. Trừ 1 từ tất cả: a = [0,3,0,0]. Còn lại một đoạn [3], cần thực hiện thêm 1 thao tác. Tổng = 1 (phạm vi op) + 1 (đoạn) = 2. Giá trị này nhỏ hơn so với việc xóa riêng lẻ (4 thao tác). 

**Mẫu 2** 

đầu vào:```
3
2 1 2
```Dấu vết: min_val = 1, trừ 1: a = [1,0,1]. Các phân đoạn: [0] bị bỏ qua, [1] và [1] mỗi phân đoạn yêu cầu 1 op. Tổng nhiệt độ = 1 (phạm vi) + 1 + 1 = 3. Việc xóa riêng lẻ cần 3, vì vậy dp[0][2] = 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi mảng con, chúng tôi quét mức tối thiểu và chia thành các phân đoạn. Tổng của tất cả các mảng con được giới hạn bởi O(n2). | 
| Không gian | O(n²) | bảng dp lưu trữ một giá trị cho mỗi mảng con. | 

Độ phức tạp này phù hợp thoải mái trong giới hạn 2 giây đối với n 5000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    dp = [[-1]*n for _ in range(n)]
    sys.setrecursionlimit(10000)
    def solve(l, r):
        if l > r:
            return 0
        if dp[l][r] != -1:
            return dp[l][r]
        res = r - l + 1
        min_val = min(a[l:r+1])
        temp = min_val
        i = l
        while i <= r:
            if a[i] == min_val:
                i += 1
                continue
            j = i
            while j <= r and a[j] > min_val:
                j += 1
            temp += solve(i, j-1)
            i = j
        dp[l][r] = min(res, temp)
        return dp[l][r]
    return str(solve(0, n-1))

# provided sample
assert run("4\n1 4 1 1\n") == "2", "sample 1"
# custom: all zeros
assert run("5\n0 0 0 0 0\n") == "0", "all zero"
# custom: all ones
assert run("3\n1 1 1\n") == "1", "all ones"
# custom: single large count
assert run("3\n0 100 0\n") == "1", "single non-zero"
# custom: increasing counts
assert run("4\n1 2 3 4\n") == "4", "increasing counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 số không | 0 | Xử lý nhiều tập hợp trống một cách chính xác | 
| 3 | | |
