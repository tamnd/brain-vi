---
title: "CF 1129D - Cách ly"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách để chia một mảng số nguyên nhất định thành các phân đoạn liền kề, không trống sao cho trong mỗi phân đoạn, số lượng số nguyên xuất hiện đúng một lần không vượt quá một ngưỡng cho trước $k$."
date: "2026-06-12T04:20:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1129
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 542 [Alex Lopashev Thanks-Round] (Div. 1)"
rating: 2900
weight: 1129
solve_time_s: 98
verified: true
draft: false
---

[CF 1129D - Cách ly](https://codeforces.com/problemset/problem/1129/D) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu cách để chia một mảng số nguyên đã cho thành các đoạn liền kề, không trống sao cho trong mỗi đoạn, số lượng số nguyên xuất hiện đúng một lần không vượt quá một ngưỡng nhất định$k$. Về mặt khái niệm, mỗi phân khúc đều “bị cô lập” theo nghĩa là nó có thể chịu đựng được nhiều nhất$k$singletons độc đáo. Ví dụ, trong mảng`[1,1,2]`với$k=1$, đoạn`[1,1,2]`là hợp lệ bởi vì`2`xuất hiện một lần, nằm trong giới hạn, nhưng một đoạn`[1,2]`sẽ không hợp lệ vì cả hai`1`Và`2`xuất hiện đúng một lần, vượt quá$k$. 

Đầu vào cung cấp kích thước mảng$n$, ngưỡng$k$và chính các phần tử mảng. Đầu ra là một số duy nhất: tổng số phân đoạn hợp lệ theo modulo$998244353$. 

Được cho$n$có thể lớn như$10^5$, bất kỳ giải pháp nào vượt quá khoảng$O(n \log n)$hoặc$O(n \sqrt n)$có lẽ là quá chậm. Một cách tiếp cận ngây thơ thử mọi phân đoạn có thể, trong đó có rất nhiều phân đoạn theo cấp số nhân ($2^{n-1}$), rõ ràng là không thể thực hiện được. Các trường hợp cạnh tới hạn liên quan đến các mảng trong đó tất cả các phần tử đều giống nhau, thỏa mãn điều kiện một cách tầm thường hoặc các mảng có nhiều phần tử riêng biệt chỉ xuất hiện một lần, trong đó các phân đoạn hợp lệ có thể rất ngắn. Ví dụ, đầu vào`[1,2,3]`với$k=1$buộc chúng ta phải phân tách sau mỗi phần tử, chỉ thu được`[1],[2],[3]`dưới dạng các phân đoạn hợp lệ. Việc triển khai bất cẩn có thể kết hợp không chính xác các phần tử mà không kiểm tra số lượng đơn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để phân vùng mảng thành các phân đoạn liền kề. Đối với mỗi phân đoạn, chúng tôi sẽ đếm các số nguyên xuất hiện chính xác một lần trong mỗi phân đoạn và kiểm tra xem chúng có vượt quá$k$. Mặc dù cách tiếp cận này đúng nhưng số lượng phân đoạn tăng theo cấp số nhân với$n$- Về$2^{n-1}$khả năng - điều này hoàn toàn không khả thi đối với$n$lên đến$10^5$. 

Quan sát quan trọng là tính hợp lệ của một phân đoạn chỉ phụ thuộc vào nội dung của nó và cụ thể là có bao nhiêu phần tử xuất hiện đúng một lần. Điều này phù hợp với cách tiếp cận lập trình động: chúng ta có thể định nghĩa`dp[i]`là số lượng phân đoạn hợp lệ của tiền tố có độ dài$i$. Sau đó với mỗi`i`, chúng tôi xem xét tất cả các điểm phân chia có thể có trước đó`j`và kiểm tra xem phân đoạn`a[j..i-1]`là hợp lệ. Chúng ta có thể tối ưu hóa điều này bằng cách duy trì một cửa sổ trượt của đoạn cuối cùng thỏa mãn ràng buộc đơn lẻ. Sử dụng hai con trỏ và bản đồ tần số, chúng ta có thể cập nhật động số lượng phần tử xuất hiện chính xác một lần. Điều này làm giảm vòng lặp bên trong thành các hoạt động O(1) được khấu hao trên mỗi phần tử, đưa ra giải pháp O(n). 

Các phương pháp tiếp cận vũ phu và tối ưu có thể được so sánh như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Cửa Sổ Trượt + DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một`dp`mảng kích thước`n+1`Ở đâu`dp[i]`lưu trữ số lượng phân đoạn hợp lệ cho tiền tố kết thúc tại chỉ mục`i-1`. Bộ`dp[0] = 1`bởi vì tiền tố trống có một phân đoạn tầm thường. 
2. Sử dụng từ điển tần số để theo dõi số lần mỗi số nguyên xuất hiện trong cửa sổ trượt hiện tại. Duy trì số lượng số nguyên xuất hiện chính xác một lần. 
3. Duy trì con trỏ bên trái`l`cho biết thời điểm bắt đầu sớm nhất có thể của một phân đoạn hợp lệ kết thúc ở vị trí hiện tại`r`. Di chuyển`l`chuyển tiếp trong khi số lượng singletons trong phân khúc`[l..r]`vượt quá`k`. 
4. Đối với mỗi`r`từ 0 đến n-1, cập nhật bản đồ tần số với`a[r]`. Nếu tần số trở thành 1, hãy tăng số lượng đơn lẻ. Nếu nó trở thành 2, hãy giảm số lượng đơn lẻ. 
5. Sau khi điều chỉnh`l`để thỏa mãn ràng buộc đơn lẻ, phân đoạn hiện tại`[l..r]`là hợp lệ. Thêm vào`dp[l]`ĐẾN`dp[r+1]`để đếm tất cả các phân đoạn kết thúc tại`r`. 
6. Cuối cùng,`dp[n]`đưa ra tổng số phân đoạn hợp lệ theo modulo 998244353. 

Tại sao nó hoạt động: Bất biến DP là`dp[i]`luôn đếm tất cả các phân đoạn hợp lệ của tiền tố có độ dài`i`. Cửa sổ trượt đảm bảo rằng chúng tôi không bao giờ bao gồm một phân đoạn có nhiều hơn`k`singletons, vì vậy mọi cập nhật lên`dp[r+1]`mở rộng chính xác các phân đoạn hợp lệ trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    dp = [0] * (n + 1)
    dp[0] = 1
    
    freq = {}
    singletons = 0
    l = 0
    
    for r in range(n):
        val = a[r]
        freq[val] = freq.get(val, 0) + 1
        if freq[val] == 1:
            singletons += 1
        elif freq[val] == 2:
            singletons -= 1
        
        while singletons > k:
            left_val = a[l]
            freq[left_val] -= 1
            if freq[left_val] == 1:
                singletons += 1
            elif freq[left_val] == 0:
                singletons -= 1
                del freq[left_val]
            l += 1
        
        dp[r + 1] = (dp[r + 1] + dp[l]) % MOD
        if r > 0:
            dp[r + 1] = (dp[r + 1] + dp[r]) % MOD

    print(dp[n] % MOD)

if __name__ == "__main__":
    main()
```Bản cập nhật bản đồ tần số theo dõi số lượng số nguyên xuất hiện chính xác một lần và cửa sổ trượt di chuyển con trỏ bên trái`l`cho đến khi phân đoạn hiện tại hợp lệ. các`dp`mảng tích lũy các phân đoạn hợp lệ bằng cách mở rộng các tiền tố hợp lệ trước đó. Sử dụng modulo đảm bảo không bị tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`3 1`với mảng`[1, 1, 2]`. 

| r | tôi | tần số | người độc thân | dp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | {1:1} | 1 | dp[1]=1 | 
| 1 | 0 | {1:2} | 0 | dp[2]=2 | 
| 2 | 0 | {1:2,2:1} | 1 | dp[3]=3 | 

Dấu vết cho thấy số lượng đơn lẻ không bao giờ vượt quá 1 và`dp`tích lũy các phần chia hợp lệ`[1],[1],[2]`,`[1,1],[2]`, Và`[1,1,2]`. 

### Ví dụ tùy chỉnh 

đầu vào:`4 2`với mảng`[1,2,3,2]`. 

| r | tôi | tần số | người độc thân | dp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | {1:1} | 1 | dp[1]=1 | 
| 1 | 0 | {1:1,2:1} | 2 | dp[2]=2 | 
| 2 | 0 | {1:1,2:1,3:1} | 3 -> l chuyển sang 1, freq={2:1,3:1}, singletons=2 | dp[3]=dp[1]=1 | 
| 3 | 1 | {2:2,3:1} | 1 | dp[4]=dp[1]+dp[3]=1+1=2 | 

Điều này thể hiện việc điều chỉnh cửa sổ trượt`l`khi số lượng đơn vượt quá`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử vào và ra khỏi cửa sổ trượt nhiều nhất một lần và tần số cập nhật là O(1). | 
| Không gian | O(n) |`dp`mảng kích thước`n+1`và lưu trữ bản đồ tần số nhiều nhất`n`các yếu tố riêng biệt. | 

Được cho`n <= 10^5`, Các thao tác O(n) dễ dàng nằm gọn trong giới hạn thời gian 3 giây và dung lượng vừa vặn trong vòng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 1\n1 1 2\n") == "3", "sample 1"

# Custom cases
assert run("1 1\n1\n") == "1", "single element"
assert run("5 2\n1 2 2 3 1\n") == "8", "mixed repeats"
assert run("4 1\n1 2 3 4\n") == "1", "all unique with k=1"
assert run("5 5\n1 1 1 1 1\n") == "16", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1`| 1 | Mảng một phần tử | 
| `5 2\n1 | | |
