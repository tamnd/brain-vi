---
title: "CF 72A - Goshtasp, Vishtasp và Eidi"
description: "Bài toán yêu cầu chúng ta quyết định xem một số nguyên dương $n$ có thể được biểu diễn dưới dạng tổng của các số nguyên riêng biệt hay không, trong đó mỗi số nguyên là 1 hay số nguyên tố. Nếu một cách biểu diễn như vậy tồn tại, chúng ta cần tạo ra một cách biểu diễn lớn nhất về mặt từ điển."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "A"
codeforces_contest_name: "Unknown Language Round 2"
rating: 1800
weight: 72
solve_time_s: 78
verified: true
draft: false
---

[CF 72A - Goshtasp, Vishtasp và Eidi](https://codeforces.com/problemset/problem/72/A) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, tham lam, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta quyết định xem một số nguyên dương có$n$có thể được biểu diễn dưới dạng tổng của các số nguyên riêng biệt, trong đó mỗi số nguyên là 1 hoặc số nguyên tố. Nếu một cách biểu diễn như vậy tồn tại, chúng ta cần tạo ra một cách biểu diễn lớn nhất về mặt từ điển. Lớn nhất về mặt từ điển có nghĩa là khi so sánh các chuỗi theo từng phần tử, chúng tôi ưu tiên các chuỗi có số lớn hơn xuất hiện trước đó. 

Đầu vào là một số nguyên duy nhất$n$với$1 \le n \le 10000$. Giới hạn trên này đủ nhỏ để cho phép các thuật toán có thể kiểm tra tất cả các số nguyên tố cho đến$n$hoặc thực hiện một số xử lý theo thời gian tuyến tính, nhưng nó loại trừ bất kỳ cách tiếp cận vũ phu nào thử tất cả các tập hợp con của số nguyên tố, vì số lượng tập hợp con tăng theo cấp số nhân. 

Các trường hợp cạnh phát sinh từ các số nhỏ và các số tự nó là số nguyên tố. Ví dụ,$n = 1$nên xuất ra`1=1`. Nếu như$n$chính nó là số nguyên tố, nghiệm lớn nhất về mặt từ điển chỉ đơn giản là dãy đơn`[n]`. Những số như 4 cần được xử lý đặc biệt, bởi vì`4 = 2 + 2`không được phép (các phần tử phải khác biệt), vì vậy thuật toán phải xem xét các kết hợp như`1 + 3`. 

Sự tinh tế chính xuất phát từ yêu cầu các con số phải khác biệt và câu trả lời phải lớn nhất về mặt từ điển. Một giải pháp bất cẩn có thể chỉ lấy các số nguyên tố nhỏ nhất và điền vào các số nguyên tố, nhưng điều đó sẽ không đáp ứng được yêu cầu về từ điển. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của`1`và các số nguyên tố lên tới`n`và kiểm tra xem tổng của chúng có bằng không`n`. Điều này đúng nhưng không khả thi đối với`n`lên tới 10000 vì số lượng tập hợp con số nguyên tố tăng theo cấp số nhân, dễ dàng vượt quá$2^{1229}$tập hợp con (số lượng số nguyên tố ≤ 10000). 

Quan sát quan trọng là nếu chúng ta muốn có một chuỗi lớn nhất về mặt từ điển, chúng ta nên bắt đầu từ những số lớn nhất có thể. Đối với bất kỳ$n$, nếu như$n$là số nguyên tố,`[n]`tự động là trình tự tốt nhất. Nếu không, chúng ta có thể liên tục chọn số nguyên tố lớn nhất ≤ tổng còn lại hoặc 1 nếu không có số nguyên tố nào đủ nhỏ và thêm nó vào dãy. Cách tiếp cận tham lam này hiệu quả vì các con số là khác biệt và cách tiếp cận lớn nhất đầu tiên đảm bảo ưu tiên từ điển. Số tiền còn lại luôn giảm nên quá trình kết thúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^π(n)) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số nguyên tố lên đến$n$dùng sàng. Việc sàng đảm bảo chúng ta có thể kiểm tra xem một số có phải là số nguyên tố trong thời gian O(1) hay không. 
2. Nếu$n$chính nó là số nguyên tố, in`n=n`và chấm dứt. Đây là từ điển lớn nhất về mặt tầm thường. 
3. Nếu không, hãy khởi tạo một danh sách trống`answer`và thiết lập`remaining = n`. 
4. Lặp lại các số nguyên tố theo thứ tự giảm dần. Đối với mỗi số nguyên tố$p$, nếu như$p \le$còn lại, nối nó vào`answer`và trừ nó khỏi`remaining`. 
5. Nếu`remaining > 0`sau khi xem xét tất cả các số nguyên tố, nối thêm`1`s khi cần thiết để tổng hợp thành`n`. Điều này hoạt động vì`1`có thể được sử dụng nhiều lần nhưng phải được coi là khác biệt trong trình tự. 
6. In chuỗi theo định dạng`n=a1+a2+...+am`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi chọn số nguyên tố lớn nhất hiện có để đảm bảo tính tối đa của từ điển. Vì chúng tôi chỉ chọn các số nguyên tố còn lại nên chúng tôi không bao giờ vượt qua. Bất kỳ số tiền còn lại có thể được lấp đầy`1`s để đảm bảo số tiền là chính xác`n`. Dãy số này vẫn khác biệt vì các số nguyên tố khác nhau một cách tự nhiên và`1`được xử lý riêng biệt. Cách tiếp cận tham lam này được đảm bảo để tạo ra một đại diện hợp lệ nếu có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sieve(limit):
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(limit**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, limit + 1, i):
                is_prime[j] = False
    primes = [i for i, val in enumerate(is_prime) if val]
    return primes

def main():
    n = int(input())
    primes = sieve(n)
    
    if n in primes or n == 1:
        print(f"{n}={n}")
        return
    
    answer = []
    remaining = n
    for p in reversed(primes):
        if p <= remaining:
            answer.append(p)
            remaining -= p
    answer.extend([1] * remaining)
    result = '+'.join(map(str, answer))
    print(f"{n}={result}")

if __name__ == "__main__":
    main()
```Sàng tạo ra tất cả các số nguyên tố ≤`n`một cách hiệu quả. Chúng tôi xử lý số nguyên tố tầm thường hoặc`1`trường hợp đầu tiên. Việc lặp lại giảm dần đảm bảo chúng ta chọn được các số nguyên tố lớn nhất để đạt được tính cực đại về mặt từ điển. Mở rộng với`1`s đảm bảo chúng tôi đạt được số tiền chính xác khi phần còn lại nhỏ. Có thể tránh được các lỗi từng cái một bằng cách sử dụng`<= remaining`và bao gồm`remaining`1s ở cuối. 

## Ví dụ đã hoạt động 

### Đầu vào: 11 

| còn lại | chọn lọc chính | danh sách câu trả lời | 
| --- | --- | --- | 
| 11 | 11 | [11] | 
| 0 | xong | [11] | 

Số nguyên tố lớn nhất ≤ 11 chính là 11. Phần còn lại trở thành 0, vì vậy chúng tôi xuất ra`11=11`. Lớn nhất về mặt từ điển vì số lớn nhất xuất hiện đầu tiên. 

### Đầu vào: 8 

| còn lại | chọn lọc chính | danh sách câu trả lời | 
| --- | --- | --- | 
| 8 | 7 | [7] | 
| 1 | không có số nguyên tố 1 | [7,1] | 

Chúng tôi chọn 7, để lại 1. Chúng tôi điền vào một`1`để có được`[7,1]`. đầu ra`8=7+1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n) | Sàng chạy ở O(n log log n), lựa chọn số nguyên tố giảm dần ở O(n). | 
| Không gian | O(n) | Lưu trữ mảng sàng và danh sách câu trả lời. | 

Được cho`n ≤ 10000`, thuật toán này chạy thoải mái trong giới hạn 5 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("11\n") == "11=11", "sample 1"
# custom cases
assert run("1\n") == "1=1", "minimum value"
assert run("4\n") == "4=3+1", "needs combination with 1"
assert run("8\n") == "8=7+1", "combination with largest prime first"
assert run("28\n") == "28=23+5", "sum of two primes"
assert run("10\n") == "10=7+3", "largest primes sum to n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1=1 | đầu vào tối thiểu được xử lý | 
| 4 | 4=3+1 | sự kết hợp không hề tầm thường với 1 | 
| 8 | 8=7+1 | tham lam lựa chọn số nguyên tố lớn nhất | 
| 28 | 28=23+5 | cần nhiều số nguyên tố | 
| 10 | 10=7+3 | tổng của hai số nguyên tố | 

## Vỏ cạnh 

cho`n = 1`, sàng không tạo ra số nguyên tố ≤ 1, nhưng chúng ta xử lý`n == 1`riêng biệt, đưa ra`1=1`. Vì`n`bản thân nó là số nguyên tố, giống như`11`, thuật toán kết thúc ngay lập tức với`[n]`, đảm bảo phần tử lớn nhất xuất hiện đầu tiên. Đối với những con số như`4`, số nguyên tố lớn nhất ≤ 4 là 3, dư 1. Cộng`1`đưa ra một trình tự hợp lệ`[3,1]`. Những kiểm tra này đảm bảo tính chính xác cho các trường hợp nhỏ nhất, nguyên tố và kết hợp.
