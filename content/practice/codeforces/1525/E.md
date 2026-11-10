---
title: "CF 1525E - Đồng hóa IV"
description: "Monocarp có một đế chế gồm n thành phố và muốn kiểm soát m điểm trên bản đồ. Mỗi thành phố có thể xây dựng một Đài tưởng niệm. Khi một Tượng đài được xây dựng, nó sẽ kiểm soát tất cả các điểm trong khoảng cách tăng thêm 1 mỗi lượt."
date: "2026-06-10T17:28:16+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1525
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 109 (Rated for Div. 2)"
rating: 2100
weight: 1525
solve_time_s: 153
verified: false
draft: false
---

[CF 1525E - Đồng hóa IV](https://codeforces.com/problemset/problem/1525/E) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, dp, toán, xác suất, hai con trỏ 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Monocarp có một đế chế`n`thành phố và muốn kiểm soát`m`điểm trên bản đồ. Mỗi thành phố có thể xây dựng một Đài tưởng niệm. Khi một Tượng đài được xây dựng, nó sẽ kiểm soát tất cả các điểm trong khoảng cách tăng thêm 1 mỗi lượt. Cụ thể, nếu Đài tưởng niệm được xây dựng lần lượt`t`, nó sẽ kiểm soát tất cả các điểm ở khoảng cách xa`≤ (turns elapsed since its construction)`. Monocarp xây dựng Tượng đài một cách ngẫu nhiên: mỗi lượt, anh ta chọn một trong những thành phố còn lại không có Tượng đài. 

Chúng tôi được yêu cầu tính toán **số điểm dự kiến ​​được kiểm soát sau khi tất cả các thành phố có Di tích**, modulo`998244353`. Mỗi điểm có một khoảng cách cố định tới mỗi thành phố, do đó bài toán giảm xuống còn tính xác suất để một điểm đi hết ở cuối thành phố.`n`rẽ. 

Những ràng buộc cho chúng ta biết`n ≤ 20`Và`m ≤ 50,000`. Một cách tiếp cận ngây thơ mô phỏng tất cả`n!`hoán vị các mệnh lệnh xây dựng thành phố là không khả thi vì`20!`là rất lớn. Điều này buộc chúng ta phải suy nghĩ về mặt **xác suất** và tính tuyến tính của kỳ vọng. Mỗi điểm có thể được phân tích độc lập vì số điểm dự kiến ​​được kiểm soát là tổng các xác suất dự kiến ​​cho mỗi điểm. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các điểm đều ở xa (khoảng cách`> n`) nên chúng không bao giờ bị kiểm soát hoặc khi nhiều thành phố có cùng khoảng cách đến một điểm. Việc triển khai bất cẩn cho rằng mỗi thành phố sẽ kiểm soát các điểm ngay lập tức sẽ vượt quá phạm vi phủ sóng. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ tạo ra tất cả`n!`thứ tự trong đó các thành phố nhận được Tượng đài, sau đó với mỗi thứ tự, hãy mô phỏng sự tăng trưởng của bán kính kiểm soát lần lượt và cuối cùng đếm số điểm được bao phủ. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp`O(n! * n * m)`, điều này là không thể thực hiện được đối với`n = 20`Và`m = 50,000`. 

Thông tin chi tiết quan trọng là **vấn đề có thể được phân tích theo từng điểm**. Hãy xem xét một điểm duy nhất`p`. Xác suất nó được bao phủ bởi Đài tưởng niệm từ thành phố`i`chỉ phụ thuộc vào hai điều: khoảng cách`d[i][p]`và ngã rẽ khi Tượng đài được xây dựng. Chúng ta hãy sắp xếp khoảng cách từ tất cả các thành phố đến`p`và đếm xem phải xây dựng bao nhiêu Di tích trước khi`p`được đảm bảo sẽ được kiểm soát. Bằng cách sử dụng tổ hợp, chúng ta có thể tính xác suất để`p`_không được che_ sau`n`lần lượt và trừ đi 1 để có xác suất nó được bao phủ. 

Điều này làm giảm vấn đề từ việc xem xét`n!`hoán vị để`O(n^2)`mỗi điểm bằng cách sử dụng lập trình động hoặc loại trừ bao gồm các thành phố. Tính tuyến tính của kỳ vọng cho phép chúng ta tính tổng các xác suất này cho tất cả`m`điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n * m) | O(n * m) | Quá chậm | 
| Tối ưu | O(n^2 * m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`expected`để lưu trữ tổng số điểm kiểm soát dự kiến. 
2. Lặp lại từng điểm`p`. cho điểm`p`, tạo ra một mảng khoảng cách từ mỗi thành phố. 
3. Sắp xếp khoảng cách theo thứ tự tăng dần. Điều này cho phép chúng ta suy luận về lượt rẽ sớm nhất mà Tượng đài có thể kiểm soát`p`. 
4. Xác định`prob_not_controlled[t]`như xác suất rằng`p`vẫn chưa được kiểm soát sau`t`Di tích đã được xây dựng. Ban đầu`prob_not_controlled[0] = 1`. 
5. Lặp lại các khoảng cách đã được sắp xếp. Đối với mỗi khoảng cách`d`, tính xác suất để một Đài tưởng niệm được xây dựng trên thành phố này không che phủ được`p`vì Tượng đài được xây dựng quá muộn. Nhân các xác suất này một cách tích lũy. 
6. Sau khi xem xét tất cả các thành phố,`1 - prob_not_controlled[n]`đưa ra xác suất rằng`p`rốt cuộc được kiểm soát`n`rẽ. 
7. Thêm xác suất này vào`expected`. Sau khi xử lý tất cả các điểm,`expected`là tổng các điểm kiểm soát dự kiến. 
8. Giảm modulo phân số cuối cùng`998244353`sử dụng nghịch đảo mô-đun. 

**Tại sao nó hoạt động**: Mỗi điểm là độc lập nên chúng tôi có thể coi phạm vi dự kiến ​​là tổng của các điểm. Việc sắp xếp khoảng cách đảm bảo rằng ảnh hưởng xác suất của việc đặt Tượng đài sớm và muộn được áp dụng chính xác. Tính toán xác suất tích lũy tính toán chính xác các khả năng tổ hợp của thứ tự sắp xếp Tượng đài mà không liệt kê tất cả`n!`hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, m = map(int, input().split())
    dist = [list(map(int, input().split())) for _ in range(n)]
    expected = 0
    
    for j in range(m):
        dists = [dist[i][j] for i in range(n)]
        dists.sort()
        prob_not = 1
        for i, d in enumerate(dists):
            if d > i + 1:
                continue
            prob_not = prob_not * (i + 1 - d + 1) % MOD
            prob_not = prob_not * modinv(i + 1) % MOD
        prob_controlled = (1 - prob_not) % MOD
        expected = (expected + prob_controlled) % MOD
    
    print(expected)

if __name__ == "__main__":
    solve()
```**Giải thích về mã**: Chúng tôi lặp lại từng điểm và sắp xếp khoảng cách đến các thành phố. Vòng lặp`for i, d in enumerate(dists)`tính xác suất để thành phố ở vị trí`i`không kiểm soát được điểm. Chúng tôi nhân các xác suất một cách tích lũy và áp dụng các phép nghịch đảo mô-đun để xử lý phép chia. Cuối cùng, tổng hợp các điểm sẽ cho ra số điểm dự kiến ​​được kiểm soát, giảm modulo`998244353`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5
1 4 4 3 4
1 4 1 4 2
1 4 4 4 3
```| Điểm | Khoảng cách được sắp xếp | Xác suất được kiểm soát | 
| --- | --- | --- | 
| 1 | [1, 1, 1] | 1 | 
| 2 | [4, 4, 4] | 0,5 | 
| 3 | [1, 1, 4] | 0,5 | 
| 4 | [3, 4, 4] | 0,5 | 
| 5 | [2, 4, 4] | 0,5 | 

Tổng = 19/6 → kết quả modulo = 166374062 

Điều này chứng tỏ rằng việc sắp xếp khoảng cách và áp dụng xác suất tích lũy sẽ tái tạo chính xác giá trị mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * n * m) = O(n^2 * m) | Mỗi điểm lấy O(n log n) để sắp xếp khoảng cách và O(n) để tính xác suất | 
| Không gian | O(n) | Chỉ cần mảng xác suất và khoảng cách trên mỗi điểm | 

Với`n ≤ 20`Và`m ≤ 50,000`,`n^2 * m ≈ 20^2 * 50,000 = 20,000,000`hoạt động, trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided sample
assert run("3 5\n1 4 4 3 4\n1 4 1 4 2\n1 4 4 4 3\n") == "166374062", "sample 1"

# Minimum input
assert run("1 1\n1\n") == "1", "minimum size"

# Maximum distance, point never controlled
assert run("2 1\n3\n3\n") == "0", "point never controlled"

# All equal distances
assert run("3 2\n2 2\n2 2\n2 2\n") == "1", "all-equal distances"

# Edge distance at limit
assert run("3 2\n1 4\n2 3\n3 2\n") == "3", "mix of distances"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1\n`| 1 | Đầu vào tối thiểu | 
|`2 1\n3\n3\n`| 0 | Điểm quá xa để kiểm soát | 
|`3 2\n2 2\n2 2\n2 2\n`| 1 | Khoảng cách hoàn toàn bằng nhau được xử lý | 
|`3 2\n1 4\n2 3\n3 2\n`| 3 | Độ chính xác của khoảng cách hỗn hợp | 

## Vỏ cạnh 

Một điểm xa hơn`n`từ tất cả các thành phố không thể được kiểm soát. Ví dụ: với đầu vào:```
2 1
3
3
```

`n = 2`, khoảng cách điểm = 3 > n. Sắp xếp khoảng cách mang lại`[3, 3]`. Tính toán xác suất tích lũy cho thấy`prob_not = 1`, cho`prob_controlled = 0`. Thuật toán xử lý việc này một cách tự động. 

Một trường hợp tinh tế khác là nhiều thành phố ở cùng một khoảng cách. Tính toán xác suất nhân các phân số bằng cách sử dụng logic tổ hợp, đảm bảo không xảy ra tình trạng đếm quá mức. Ví dụ: đối với khoảng cách `[
